## HDX Monitor Scraper Notification API
Service designed to collect success and error information from scrapers and collectors.


## Docker Usage
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
