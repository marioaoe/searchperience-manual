In OM3 *retailer*, *brand* and *location* data comes from MasterDataPortal (MDP).

## Flow

```mermaid
graph LR
  A[Client] -- Data and media --> B(MasterDataPortal)
  B -- API Crawling --> C(Importer)
```

## Importer
An integration application, which is responsible to crawl the MasterDataPortal API and converts retailer, brand and location information in the Searchperience format.

!!! info 
    Importer just crawls MDP entities that are publicly available i.e. `internal = false`  

### Crawling

| Frequency    | Description                                                                 |
|--------------|-----------------------------------------------------------------------------|
| Every 1 hour | All entities configured - depends on the project - are crawled and updated. |

### Updates
In case of an update MDP will send a message to importer indicating that the updated entity should be crawled again. It should take around 10 min to see the changes in Searchperience API. 