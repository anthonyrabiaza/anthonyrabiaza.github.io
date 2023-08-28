---
layout: post
title:  "Boomi Observability with Elastic"
permalink: /boomi-observability-elastic/
---
Implementation with a Monitoring Platform for Observability with Elastic
===========================================================

## Overview

![Boomi AtomSphere](/assets/boomi-observability-elastic/elasticapm.png)

## FileBeat and MetricsBeat Installation

The first step consist of installing the FileBeat and Metric Beat on each server. This step will link the server (or Cloud resource) to the ELK stack and will allows the gathering of logs, metrics, status of server, processes, network, etc.

## APM Server Installation

The second step consist of installing the APM Server. This step will link the Java server to Elastic and will allows the connection of APM Client to the ELK Stack.

## Java Agent Installation

The last step consist of installing the  Java Agent on each Boomi runtime. This step will link the Java server to Elastic and will allows the gathering of JVM metrics and traces.

## APM Setup

The Elastic Java Agent (Jar) needs to be deployed on each node or deployed on the Shared Server accessible by all nodes, once this is done Boomi System properties needs to be updated via Boomi AtomSphere:  

![Boomi AtomSphere](/assets/boomi-observability-elastic/boomi-config-elastic.png)

All the other configuration elements will be defined in elasticapm.properties (located in the same folder as the Java Agent Jar)

Example of elasticapm.properties
```
service_name=boomiatom
environment=development
...
secret_token=
server_urls=http://<APMServer>:8200
```

### APM Configuration: Tracing and Custom Data Collector

#### Boomi JMS Processes

Boomi JMS processes do not require any change, they will be automatically detected by Elastic APM.

#### Boomi Scheduled Processes

The Boomi Scheduled processes won'tibe detected by Elastic APM and requires a manual instrumentation using [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm) will minimize the changes:  

Initial process:

![APM Instrumented](/assets/boomi-observability-elastic/boomi-process-3.png)  

The updated process will looks like the following:  

![APM Instrumented](/assets/boomi-observability-elastic/boomi-process-1-apm.png)  

The changes includes:

1.  The APM Start shape at the beginning
2.  The APM Stop shape before the last End, please note that we created a branch here as the Disk shape (Get) might not returned a Document thus an APM Stop shape after the Disk might not be called
3.  The APM Error in the try catch

The [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm) will allow the Instrumentation of any Boomi Processes and will provide with 3 steps: Start, Stop or Error

#### Boomi API Processes

In the context of Elastic APM, the Boomi API processes require manual instrumentation using the [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm). The changes required are the same as a Boomi Scheduled Process (see above).

## Review of the Observability with Elastic

### Observability Overview

![APM Instrumented](/assets/boomi-observability-elastic/overview.png)  

#### Host Metrics

![Traces](/assets/boomi-observability-elastic/metrics.png)

#### Logs

![Logs](/assets/boomi-observability-elastic/logs.png)

#### APM Overview

![APM](/assets/boomi-observability-elastic/apm-overview.png)

#### Traces

![Traces](/assets/boomi-observability-elastic/traces.png)

#### Detail of Scheduled Processes
We have an end-to-end view of the Process:

![Transaction](/assets/boomi-observability-elastic/apm-transaction-details.png)

Metadata (added by the APM Connector)

![Transaction Metadata](/assets/boomi-observability-elastic/apm-transaction-metadata.png)