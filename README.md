## Scraper Monitoring API
Service designed to collect success and error information from scrapers and collectors.
[![Build Status](https://travis-ci.org/luiscape/hdx-monitor-scraper-status.svg)](https://travis-ci.org/luiscape/hdx-monitor-scraper-status)

## Usage
The API has the following working methods:

* `/` **GET**: Retrieves a running list of scraper status.
* `/` **POST**: Stores a record of a scraper status. It needs the following arguments:
 * `id`: Scraper id. Scrapers should have unique id.
 * `status`: Either `error` or `ok`.
 * `message`: A string with the message. Required in case of `error`.
 * `time`: An [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) time stamp (up to seconds).
 * `datasets`: An array with dataset ids (not the hashes). 

Example request:

```shell
$ curl -X POST localhost:4000/ \
  -d "id=scraper-test&status=error&message='Failed to \
  connect to API.'&time=2015-06-01T14:34:01&datasets=ebola-data,hospitals-dataset"
```

## Docker Setup
[![](https://badge.imagelayers.io/luiscape/hdx-monitor-scraper-status:latest.svg)](https://imagelayers.io/?images=luiscape/hdx-monitor-scraper-status:latest 'Get your own badge on imagelayers.io')

Review the `Dockerfile` and run it linking to a MongoDB instance. `make setup` will try to setup its own collection in the instance (called `scraper_status`). This image doesn't need a volume mounted, but it needs the following environment variables in order to work appropriately:

* `MONOGDB_SCRAPER_STATUS_USER_NAME`: Dedicated user name for manipulating collections.
* `MONGODB_SCRAPER_STATUS_USER_PASSWORD`: Password for the user above.

Those should be passed when running the image.

```shell
$ docker run -d --name scraper_status \
  --link mongo:mongo \
  -e MONOGDB_SCRAPER_STATUS_USER_NAME=foo \
  -e MONGODB_SCRAPER_STATUS_USER_PASSWORD=bar \
  luiscape/hdx-monitor-scraper-status:latest
```
