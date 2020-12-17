Through the Amazon Simple Queue Service `SQS` it is possible to send document information to the Searchperience Indexer.
Based on the `type` field, the document will be added to or removed from Searchperience.

## Add Message 

```json
[
  {
    "foreignId": "talisker-en_GB-master",
    "source": "masterdataportal",
    "type": "add",
    "mimeType": "application/json+sp-brand",
    "payload": {
      "foreignId": "talisker-en_GB-master",
      "code": "talisker",
      "channel": "master",
      "media": [
        {
          "reference": "/media/images/86/2018-11-30-08_37_26-Flamingo-Blazing-fast-modern-commerce-experience-layer-perfect-for-your-micr.png",
          "usage": "logo",
          "mimeType": "image/jpg",
          "type": "image-api",
          "title": "Front View"
        }
      ],
      "locale": "en_GB",
      "type": "default",
      "title": "Talisker",
      "formatVersion": 1
    }
  }
]
```

## Delete Message

```json
  {
    "foreignId": "walker-and-hall_100169-en_GB-master",
    "source": "pim",
    "type": "delete",
    "mimeType": "application/json+sp-product"
  }
```

## Json Schema validation of SQS Messages

During pull process from `SQS` queue, each message and its payload goes throw JSON schema validation.

There is two step of validation:
 * Message structure validation; validates by indexer system core basic fields required by sqs.json schema depending on message type (add/delete)

!!! Danger
    In case of SQS message format validation error, the whole SQS message processing will be skipped and marked as `delete from SQS queue`, no documents will be processed at all out of not valid SQS message. 

| Field name | Field type | Required | Message type |
|------------|:----------:|:--------:|:------------:|
| foreignId  |   string   |   yes    |  add/delete  |
| source     |   string   |   yes    |  add/delete  |
| type       |   string   |   yes    |  add/delete  |
| mimeType   |   string   |   yes    |  add/delete  |
| payload    |   object   |   yes    |     add      |

## `sqs.json` schema: 

```json
{
  "description": "SQS message schema",
  "$schema": "http://json-schema.org/draft-06/schema#",
  "definitions": {
    "sqsDeleteMessage": {
      "type": "object",
      "required": ["foreignId", "source", "type", "mimeType"],
      "properties": {
        "foreignId": {
          "type": "string",
          "pattern": "^([A-Za-z0-9-_]+)$"
        },
        "source": {
          "type": "string"
        },
        "type": {
          "type": "string",
          "enum": ["delete"]
        },
        "mimeType": {
          "type": "string",
          "pattern": "^([A-Za-z0-9-_+\/.]+)$"
        }
      }
    },
    "sqsAddMessage": {
      "type": "object",
      "required": ["foreignId", "source", "type", "mimeType", "payload"],
      "properties": {
        "foreignId": {
          "type": "string",
          "pattern": "^([A-Za-z0-9-_]+)$"
        },
        "source": {
          "type": "string"
        },
        "type": {
          "type": "string",
          "enum": ["add"]
        },
        "mimeType": {
          "type": "string",
          "pattern": "^([A-Za-z0-9-_+\/.]+)$"
        },
        "payload": {
          "type": "object"
        }
      }
    },
    "sqsMessage": {
      "oneOf": [
        { "$ref": "#/definitions/sqsAddMessage" },
        { "$ref": "#/definitions/sqsDeleteMessage" }
      ]
    },
    "sqsMessageArray": {
      "type": "array",
      "minItems": 1,
      "items": { "$ref": "#/definitions/sqsMessage" }
    }
  },
  "oneOf": [
    { "$ref": "#/definitions/sqsMessage" },
    { "$ref": "#/definitions/sqsMessageArray" }
  ]
}
```

* Payload validation; validates project specific document type, like brand, location, product etc. against defined schema.

!!! Danger
    In case of payload validation error, the document will be skipped and not processed at all, no documents will be added to the indexer DB and to elasticsearch. 

## `brand.json` schema validation example:
 
```json
{
  "description": "Brand schema",
  "$schema": "http://json-schema.org/draft-06/schema#",
  "required": ["foreignId", "code", "channel", "locale", "title"],
  "properties": {
    "foreignId": {
      "type": "string",
      "pattern": "^([A-Za-z0-9-_]+)$"
    },
    "code": {
      "type": "string",
      "pattern": "^([A-Za-z0-9-_]+)$"
    },
    "channel": {
      "type": "string"
    },
    "shortDescription": {
      "type": "string"
    },
    "media": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "reference": {
            "type": "string"
          },
          "usage": {
            "type": "string"
          },
          "mimeType": {
            "type": "string"
          },
          "type": {
            "type": "string"
          },
          "title": {
            "type": "string"
          }
        }
      }
    },
    "locale": {
      "type": "string"
    },
    "title": {
      "type": "string"
    },
    "formatVersion": {
      "type": "integer"
    },
    "teaser": {
      "type": "string"
    }
  }
}
```

All information about skipped messages or documents with not valid payload is logged.
 