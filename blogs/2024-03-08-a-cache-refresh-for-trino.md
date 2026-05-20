---
title: "A cache refresh for Trino"
url: "https://trino.io/blog/2024/03/08/cache-refresh.html"
date: "2024-03-08T00:00:00+00:00"
author: "Manfred Moser"
feed_url: "https://trino.io/blog/feed.xml"
---
Thinking about our recent work on caching in Trino reminds me of the famous saying, “There are only two hard things in computer science: cache invalidation and naming things.” Well, in the Trino community we know all about caching and naming. With the recent Trino 439 release, caching from object storage file systems got a refresh. Catalogs using the Delta Lake, Hive, Iceberg, and soon Hudi connectors now get to access performance benefits from the new Alluxio-powered file system caching.
