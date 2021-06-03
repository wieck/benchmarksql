
# Collecting OS performance metric data

## Problem Statement

A database server should not be a single computer, or worse
one of many services on a single computer. Databases have
hot-standby read replicas, continuous write-ahead-log based backup
(sometimes to cloud storage), connection pools, load balancers and
more. And if that is how we intend to deploy our database in
production, then why would we benchmark it as a single, stand-alone
database? That adds a lot of complexity to our benchmarking efforts.

When running a benchmark on a production like deployment
we don't just want to learn a single number, like "this system peaks
at n NEW_ORDER transactions per minute. That is a very important 
number when running a TPC-C benchmark, but it doesn't tell us if
the hot standby servers were able to keep up, if the backup system
was able to keep up with saving the write-ahead-log and so on.
And if they do, we want to know if there is still some headroom.

What we need is metric data that is not readily available inside
the benchmark driver, BenchmarkSQL itself. We need things like the
CPU usage, disk IO and network IO of the database server,
the hot standby server and so on. If possible we also want to
see the replication lag and write-ahead-log archive backlog.

To make matters worse, since the benchmark driver (BenchmarkSQL)
represents the end user and application, it should not be running
on the database server. The CPU and other resources used by the
benchmark driver itself would affect the result in a very different
way than a real application would. Especially if the application
was properly deployed on separate systems.
And even if the benchmark driver ran on the same system as the
database, it could not directly access performance information of a
remote system like the standby server.

The problem therefore is to be able to collect OS level performance
metric data from arbitrary systems and include them in the benchmark
report.

## How to collect OS metrics

There is a large selection of tools for collecting, visualizing
and alerting on metric data. BenchmarkSQL cannot support all of
them directly. Instead BenchmarkSQL implements Python scripts
that are meant to extract the data from the underlying time series
data store and save it in a common format for the report
generator. The file format is described in detail at the end of
this page.

The two solutions we cover for this tutorial series are

[*prometheus* and *node_exporter*](HOW-TO-prometheus.md)

or

[*collectd* and *graphite*](HOW-TO-collectd+graphite.md).

## The *os-metric.json* file format


