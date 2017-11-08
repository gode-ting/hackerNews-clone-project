# Service Level Agreement

## Introduction
This document includes the service-level agreement between Group B and Group C, wherein we will outline the mutual SLA for the monitoring of the Hacker News system developed and maintained by Group C.

The SLA will include agreed-upon metrics for which the HackerNews system will be maintained by Group B, particularly with regards to the average uptime of the system under observation, mean response time of the system, mean time to recovery on failure, failure frequency, maintenance group alarms, and the timeframe in which the maintenance group must respond to inquiries by the observing group.

## Up
Up is a simple metric that lets you know whether the service is up or down. It should never be down unless the system is under maintenance. I everything is working it will say Yes.

## Uptime
The total uptime calculated in days.

## Avg uptime (coming soon)
In accordance with the original assignment specification, the minimum acceptable percentile of system uptime must be held at or above 95% of total system lifespan.


## Number of requests (200) from /latestn endpoint
A graph showing all status code 200 requests hitting the /latest endpoint.


