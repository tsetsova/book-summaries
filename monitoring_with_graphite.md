# Monitoring with Graphite by Jason Dixon
Published by O'Reilly Media, Inc., 2017 

> Without real-time monitoring and a high-performance analytical data store, we lack the perspective to qualify our current, past, or future performance. Business decisions are increasingly data-driven, and Graphite provides all the tools to help users collect, store, retrieve, and analyze data quickly and effectively.

## The challenge

> There’s been an explosion in the number of open source projects and commercial monitoring products over the last few years. Advancements in solid-state drive (SSD) storage and the persistence of Moore’s Law have made it technically and financially feasible to collect, store, retrieve, correlate, and visualize huge amounts of real-time monitoring data with commodity systems (read: the Cloud). Distributed database systems (particularly NoSQL), themselves largely driven by competition in the pursuit for Big Data analytics, have made it easier than ever before to scale out horizontally and add capacity as demand necessitates. Why do I place such an emphasis on storage? Because without fast storage (and a lot of it), we wouldn’t be able to persist all of this wonderful data to disk for later retrieval. Of course, it helps to understand what I mean by “fast storage.” Frankly, it’s not that hard to write to disk quickly, nor is it difficult to read from disk quickly. Where the challenge arises is in doing both at the same time.

Input and output (or I/O) are managed by the kernel (as well as filesystems).I/O is expensive especially if you’re trying to do a lot of one and **any of the other** at any point in time. A popular solution is buffering writes in memory with a cache (which requires a lot of memory). This has an additional benefit of providing "hot copies" of the data for queries(reads). This strategy only works, however, if you write routinely to disk in order to not lose the data, and expire frequently what's in the cache so users don't query stale data. 

## Graphite Architecture

Graphite is structured in a way that handles the above-mentioned tradeoffs. Caching is done with the help of carbon daemons, which write data to Whisper, a time-series database. These give Graphite the ability to store and retrieve time-series data quickly—and at high volume. 

### Carbon
> A network service that listens for inbound metric submissions. It stores the metrics temporarily in a memory buffer-cache for a brief period before flushing them to disk in the form of the Whisper database format.

Carbon consists of three parts:

#### Carbon relay
#### Carbon cache
#### Carbon aggregator

### Whisper
>The specification of the database file layout, including metadata and rollup archives, as well as the programming library that both Carbon and the Graphite web application use to interact with the respective database files.

## Graphite Features

* Simple metrics format
It's really easy to send metrics to Graphite - you need a dot-delimited metric name (e.g., "service.instance"), a numeric value, and an epoch timestamp. You can do this from your application derectly or with a bash script for example. 

The example below uses netcat to send data an example graphite server via TCP on port 2003:

```bash
  echo "service.instance data_point `date +%s`" | nc graphite-server.example.com 2003
```

* Graphing API
Graphite has a simple but robust API, which is consumed by it's own web UI. Graphs are easily constructed in the Composer and can be shared via URL. They refresh in real-time after any updates or edits.

* Rich statistical library and chained functions
Graphite supports an incredibly rich statistical funcionality - from identifying the min, max, or mean of a series to calculating percentiles and standard deviation of your series, to more complex algorithms such as various Holt-Winters forecasting algorithms and virtually every sorting and filtering criteria you could imagine. Data transformations are easy, since Graphite allows for the chaining of functions. For example the original raw data can be passed into a rendering function and the output of that function can then be passed onto another function— and so on and so on. Once all transformations are complete, Graphite passes the series on to your choice of rendering formats: a graph, JSON, etc.

The example below sums the number of all killed ec2 instances, summarized by 24 hours.

```
target=summarize(sumSeries(ec2.instances.*.killed), "1d")
```

## Glossary 

* Time-series data: a sequence of values collected at regular, uniformly spaced intervals
* Time-series database: a software system that’s optimized for the storage and retrieval of time-series data
* Telemetry: the collection of measurements, typically of remote instruments, for the purposes of monitoring
* Seasonality/ Periodicity: describe the characteristics of regular or predictable patterns that recur around the same time every hour/day/month/year
* Resolution: the precision and interval for which the metric’s values are captured. For example, Carbon reports its own internal statistics at a resolution of one minute for up to one year aka enough datapointsat a 60-second interval to describe one year’s worth of data (not counting leap years). **We can always request lower precision values using summarize(), but we can never ask for higher precision values than our database was configured to support at that resolution.**
