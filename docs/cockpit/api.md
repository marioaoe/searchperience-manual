# API Endpoints

Searchperience Cockpit offers various endpoints with different functionality.

## Crawl endpoint

!!! warning
    The domain of the URL has to be whitelisted by Searchperience. Otherwise, the request will fail.

Used to add url to the crawler queue.

```bash
$ curl -X POST http://<searchperience cockpit url>/api/crawl?url=https://my-domain/url-to-crawl 
```

Endpoint is duplicate functionality described in [Crawler Add URL](crawler.md#add-url)

## Health endpoint

Used to get related services status like: mysql, indexer, frontend and s3 bucket.

```bash
$ curl -X POST http://<searchperience cockpit url>/api/health
```

## Ping endpoint

Used to get Cockpit status for the liveness probe.

```bash
$ curl -X POST http://<searchperience cockpit url>/api/ping
```