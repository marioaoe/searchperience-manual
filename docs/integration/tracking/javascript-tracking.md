Include the following snippet in the page footer right before the closing `</body>` tag:

```html
<script type="text/javascript">
var ____tu = '___PROJECT_DOMAIN____/'; // server expecting the events
var ____td = ____td || []; // can be left out
var ____tq = ____tq || [];
</script>
```

You then include the JavaScript file:

Through a `</script>` tag:

```html
<script type="text/javascript" src="http://cnd.somewhere.aoe.host/t.min.js"></script>
```

Or through JavaScript:

```html
<script type="text/javascript">
(function() {
    var tr = document.createElement('script');
    tr.type = 'text/javascript';
    tr.async = true;
    tr.src = 'http://cnd.somewhere.com/t.min.js';
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(tr, s);
})();
</script>
```

!!! Warning
	It is important to load the  JavaScript file _after_ the variables are initialized.

For the integration following variables should be configured:

| Variable | Description                                                                                                     |
|:---------|:----------------------------------------------------------------------------------------------------------------|
| `____td` | Used to configure default values for events. This values will influence all events tracked (see example below). |
| `____tq` | Queue array which provides the main API of the tracker described in the following parts.                        |
| `____tu` | Used to configure the tracker's URL, which serves the JavaScript library and where events are saved.            |

To configure and trigger actual events just push them to the queue - like this:

```javascript
// a "click" event for the item "268543" will be added to the tracking queue
____tq.push({item: 'apple_100252828_variant', itemmaster: 'apple_100252828-en_GB-master_foreignID', track: 'click', channel: 'master', locale: 'en_US'});
```

## Event configuration

The event description is build with an JavaScript object and some base properties:

| Name    | Description                                                                                                  | Default    | Multivalues | Required |
|:--------|:-------------------------------------------------------------------------------------------------------------|:-----------|:------------|:---------|
| `when`  | HTML element or CSS selector for the element which should be observed for events.                            | `document` | ✗           | ✗        |
| `is`    | Event type(s) which will trigger the tracking.                                                               | `fire`     | ✓           | ✗        |
| `given` | Condition(s) which need to be meat before an event is tracked. Conditions can be used in a asynchronous way. | -          | ✓           | ✗        |
| `use`   | Enables to "collect" assets which are passed as information to the tracking.                                 | -          | ✓           | ✗        |
| `track` | Describes the "action" value which is actually tracked.                                                      | -          | ✗           | ✓        |

A full example using all of the options could look like this:

```javascript
____tq.push({when: 'button.green', is: 'clicked', track: 'click', use: 'itemmaster apple_100252828-en_GB-master_foreignID', ... });
```

!!! Notice
    In case there is a `when` in the event object, an event will be triggered only after the referenced element is manipulated as described in the `is` property.

### Convenience aliases

| Name          | Description                                     | Default | Multivalues | Required |
|:--------------|:------------------------------------------------|:--------|:------------|:---------|
| item          | Alias for `use: "item apple_123_variant"`       | -       | no          | no       |
| itemmaster    | Alias for `use: "itemmaster apple_123_main"`    | -       | no          | no       |
| user          | Alias for `use: "user 123-435-123"`             | -       | no          | no       |
| query         | Alias for `use: "query xxxx"`                   | -       | no          | no       |
| searchResults | Alias for `use: "searchResults 1234,3456,5678"` | -       | -           | no       |
| widget        | Alias for `use: "widget brandsByIds"`           | -       | -           | no       |
| brandCode     | Alias for `use: "brandCode xxxx"`               | -       | no          | no       |
| retailerCode  | Alias for `use: "retailerCode xxxx"`            | -       | no          | no       |
| categoryCodes | Alias for `use: "categoryCodes aaa,bbb,ccc"`    | -       | no          | no       |
| channel       | Alias for `use: "channel xxxx"`                 | -       | no          | no       |
| locale        | Alias for `use: "locale xxxx"`                  | -       | no          | no       |
| referer       | Alias for `use: "referrer xxxx"`                | -       | no          | no       |

An example using an alias could look like this:

```javascript
____tq.push({ track : 'search', searchResults : "foreignId_12,foreignId_34,foreignId_78", query : 'someUserQuery', locale : 'en_US', channel: 'master', referer : 'homepage'});
```

### Event types

The options available for events depend on the setup of the specific event. The current options are described below.

| Variable       | Description                                                                                                              |
|:---------------|:-------------------------------------------------------------------------------------------------------------------------|
| blurred        | Is triggered when an element lost focus.                                                                                 |
| clicked        | Is triggered when a click happens on an element.                                                                         |
| focussed       | Is triggered when an element gains focus.                                                                                |
| hovered over   | Is triggered when the cursor enters the elements bounding box.                                                           |
| hovered out of | Is triggered when the cursor leaves the elements bounding box.                                                           |
| scrolled       | Is triggered when an element is scrolled. On "normal" pages all scroll events will originate from the "document" object. |
| submitted      | Only relevant for `<form>` elements.                                                                                     |
| changed        | Only relevant for `<input>`, `<textarea>` and `<select>` elements.                                                       |
| loaded         | Alias for "fire"                                                                                                         |
| ready          | Alias for "fire"                                                                                                         |
| fire           | Just passes through the event processing and fires the tracking right away. The setting is used as default.              |

### Condition types

The options available for conditions depend on the setup. The current options are described below.

| Condition | Description                                                                                                                                                                                          |
|:----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XX times  | Makes sure that an event has been seen XX times before actually being tracked. Useful for scrolling and hovering related events.                                                                     |
| XXs delay | Once triggered, it will delay the tracking for XX seconds. This can, e.g., be used to make sure that a user did not leave the page within a specific time frame. Useful when tracking `view` events. |

### Asset types

The options available for any assets depend on the setup of the specific tracker.

| Asset                     | Description                                                                                                                                |
|:--------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------|
| user XXX                  | Uses the XXX value as the unique user identifier.                                                                                          |
| item XXX                  | Uses the XXX value as the item identifier. This value should be used to differentiate between the variants of a product (marketplaceCode). |
| itemmaster XXX            | Uses the XXX value as the identifier for the main product (foreign ID).                                                                    |
| query XXX                 | Uses the XXX value as the query parameter.                                                                                                 |
| searchResults XXX,YYY,ZZZ | Uses XXX,YYY,ZZZ as the identifiers of a collection of products. Use comma separated non-spaced values.                                    |
| widget XX                 | Uses the XX value as the widget alias, this value should be included when tracking widgets interactions.                                   |
| brandCode XX              | Uses the XX value as the brandCode of the product, this value should be included when possible.                                            |
| retailerCode XX           | Uses the XX value as the retailerCode of the product, this value should be included when possible.                                         |
| categoryCodes XXX,YYY,ZZZ | Uses the XXX,YYY,ZZZ values as categories codes of the product, this value should be included when possible.                               |
| channel XX                | Uses the XX value as the current channel, this value should be included when possible (just one).                                          |
| locale XX                 | Uses the XX value as the locale for the event.                                                                                             |
| referer XX                | Uses the XX value as the referrer for the action being tracked. For example, `homepage` (no specific schema atm)                           |

As already stated in a previous section, it is possible to use assets directly as properties in an event object.

```javascript
{ ... use: 'item apple_100252828_variant' ... } == { ... item: 'apple_100252828_variant' ... }
```

### Events

| Event       | Description                                                                                | Necessary elements                              | Optional elements                                 |
|:------------|:-------------------------------------------------------------------------------------------|:------------------------------------------------|---------------------------------------------------|
| `addToCart` | Tracks the placement of a product in the shopping cart from any location.                  | `itemmaster`, `item`,`channel`,`locale`         | `user`,`brandCode`,`retailerCode`,`categoryCodes` |
| `checkout`  | Tracks a purchase event. It should be triggered when a purchase is most certain. [1]       | `user`,`itemmaster`, `item`, `channel`,`locale` | `brandCode`,`retailerCode`,`categoryCodes`        |
| `click`     | Indicates that a product received a click. Relevant only when displaying a product teaser. | `itemmaster`,`channel`,`locale`,                | `user`,`referer`                                  |
| `search`    | Indicates a search event.                                                                  | `query`, `searchResults`,`channel`,`locale`     | `user`,`referer`                                  |
| `view`      | Should be used when a user spends a certain amount of seconds on the product detail page.  | `itemmaster`,`channel`,`locale`                 | `user`,`brandCode`,`retailerCode`,`categoryCodes` |

[1] In order to differentiate the channel/category/brand information on every single product, please create a request for each product with the corresponding itemmaster and item values.

#### Events names & aliases

!!! Warning
    Any events with a invalid event name will return a `BAD REQUEST` error.

!!! Notice
    The event names that can be used for tracking can be found at `http://sp-tracker.location.cloud/events`. 

In order to differentiate between different interactions within the scope of the same event it is possible for the same event to have multiple aliases. For example:
 
```
search : [
    "search",
    "livesearch",
    "mobilesearch"
]
```

This implies that one can use `search`, `livesearch` and `mobilesearch` track a search event. This helps us to get more specific metrics and improve our services.

#### Examples

```
# addToCart
{  
   when:'button.toBasket',
   is:'clicked',
   track:'addToCart',
   itemmaster:'apple_100252828-en_US-master',
   item:'apple_100252828_variant',
   brandCode:'blue',
   retailerCode:'TheRetailer',
   categoryCodes:'Technology,Technology.Travel',
   channel:'master',
   locale:'en_US'
}
```

```
# checkout
{  
   when:'pay.button',
   is:'clicked',
   track:'checkout',
   user:'some-id',
   itemmaster:'apple_100252828-en_US-master',
   item:'apple_100252828_variant',
   brandCode:'blue',
   retailerCode:'TheRetailer',
   categoryCodes:'Technology,Technology_Travel',
   channel:'master',
   locale:'en_US'
}
```

```
# click
{  
   when:'button.clicked',
   track:'click',
   itemmaster:'apple_100252828-en_US-master',
   channel:'master',
   locale:'en_US'
}
```

```
# widgetclick
{  
   when:'widget.clicked',
   track:'widgetclick',
   itemmaster:'apple_100252828-en_US-master',
   widget:'newArrivals',
   channel:'master',
   locale:'en_US'
   referer:'homepage'
}
```

```
# search
{  
   track:'search',
   query:'shiny apple',
   searchResults:'apple_111-en_US-master,apple_222-en_US-master,apple_333-en_US-master,apple_444-en_US-master,apple_555-en_US-master',
   channel:'master',
   locale:'en_US'
}
```

```
# livesearch
{  
   track:'livesearch',
   query:'fast apple',
   searchResults:'apple_111-en_US-master,apple_222-en_US-master,apple_333-en_US-master,apple_444-en_US-master,apple_555-en_US-master',
   channel:'master',
   locale:'en_US'
}
```

```
# widgetshow
{  
   track:'widgetshow',
   searchResults:'apple_111-en_US-master,apple_222-en_US-master,apple_333-en_US-master,apple_444-en_US-master,apple_555-en_US-master',
   widget:'productsByBrandName'
   channel:'master',
   locale:'en_US',
   referer:'homepage'
}
```

```
# view
{  
   when:'document',
   is:'scrolled',
   given:'10s delay',
   track:'view',
   itemmaster:'apple_100252828-en_US-master',
   brandCode:'blue',
   retailerCode:'TheRetailer',
   categoryCodes:'Technology,Technology_Travel',
   channel:'master',
   locale:'en_US'
}
```

### Global default Values

The `____td` array can be used to maintain global default values for assets. This can be done with:

```javascript
____td.push({asset: '<name>', value: '<value>'});
```

It will affect all following event chains - it won't affect already existing chains. Useful when using a userID as any event tracked after setting the default will contain this value.

## Resulting Request

The tracker transforms each element in the tracking queue into a request. For example:

```javascript
____tq.push({track: 'search', query: "someQuery", results : "itemA,itemB,itemC,itemD", channel: 'master', locale: 'en_US' });
```

is transformed into

```javascript
___PROJECT_DOMAIN____/_?ucookie=bbccf8d2-c038-4864-85a7-0f084982d97a&usession=49c82dcd-612e-4d80-8225-66c4f9710ee2&action=search&q=someQuery&results=itemA,itemB,itemC,itemD&locale=en_US&channel=master
```

where the `usession` and `ucookie` elements were automatically generated by the Searchperience JavaScript.

## Alternative use of the tracker

If you are generating your own calls, please include the user cookie `ucookie` (which is provided by the tracker through the at the `/` endpoint ) and user session `usession` as there are required for a valid request.

The following table offers a reference of the elements pushed by the tracking queue and the keys used in a tracking request. Check the `Resulting Request` part for an example.

| Element         | Key             | Always Required |
|-----------------|-----------------|-----------------|
| `user session`  | `usession`      | yes             |
| `user cookie`   | `ucookie`       | yes             |
| `track`         | `action`        | yes             |
| `user`          | `uid`           | no              |
| `itemmaster`    | `imaster`       | no              |
| `item`          | `item`          | no              |
| `query`         | `q`             | no              |
| `searchResults` | `results`       | no              |
| `brandCode`     | `brandCode`     | no              |
| `retailerCode`  | `retailerCode`  | no              |
| `categoryCodes` | `categoryCodes` | no              |
| `channel`       | `channel`       | yes             |
| `locale`        | `locale`        | yes             |
| `referer`       | `referer`       | no              |
| `raw`           | `raw`           | no              |
