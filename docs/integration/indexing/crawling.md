Crawling allows Searchperience to index web documents and offer them as part of search results.

## Crawling a document

Users can use two different methods to set up which pages should be crawled:

1. through the [Cockpit UI](../../cockpit/crawler.md#add-url).
2. through the [Cockpit API](../../cockpit/api.md#crawl-endpoint).

## Configuration

- Searchperience will just crawl configured allowed sites. 
- Searchperience will crawl individual pages or follow any links in a `sitemap.xml`.
- Searchperience crawler respects the no-index indicator `<meta name="robots" content="noindex">.