---
layout: post
title:  "Boomi Observability with Grafana Cloud"
permalink: /boomi-observability-grafanacloud/
---
Implementation with a Monitoring Platform for Observability with Grafana Cloud
===========================================================

## Overview

![Boomi AtomSphere](/assets/boomi-observability-grafanacloud/grafanacloud.png)

## Grafana Stack Installation

The first step consist of installing the Grafana Stack on each server:

- Grafana Agent (see [installation steps](https://grafana.com/docs/grafana-cloud/agent/){:target="_blank"})
- Promtail (see [installation steps](https://grafana.com/docs/loki/latest/clients/promtail/installation/){:target="_blank"} and [sample configuration](https://github.com/anthonyrabiaza/opentelemetry-log-parser#promtail-configuration){:target="_blank"})
- OpenTelemetry Collector (see [installation steps](https://opentelemetry.io/docs/collector/getting-started/){:target="_blank"} with a [sample configuration](https://github.com/anthonyrabiaza/opentelemetry-log-parser#configure-opentelemetry-collector){:target="_blank"})
- OpenTelemetry Log Parser (see [here](https://github.com/anthonyrabiaza/opentelemetry-log-parser){:target="_blank"})

This step will link the server to Grafana Cloud and will allows the gathering of logs, metrics, status of server, processes, network, etc.

## Java Agent Installation

The second step consist of installing the Java Agent on each Boomi runtime. This step will link the Java server to the OpenTelemetry Collector and will allows the gathering of JVM metrics and sending of traces.

## APM Setup

The Grafana Cloud Java Agent (Jar) needs to be deployed on each node or deployed on the Shared Server accessible by all nodes. Once this is done, Boomi System properties needs to be updated via Boomi AtomSphere:  

![Boomi AtomSphere](/assets/boomi-observability-grafanacloud/boomi-config-grafanacloud.png)

### APM Configuration: Tracing

#### Boomi Scheduled Processes

The Boomi Scheduled processes will be detected by Grafana Cloud APM but requires a manual instrumentation using [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm){:target="_blank"} to add the tracing metadata:  

Initial process:

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-process-1.png)  

The updated process will looks like the following:  

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-process-1-apm.png)  

The changes includes:

1.  The APM Start shape at the beginning
2.  The APM Stop shape before the last End, please note that we created a branch here as the Disk shape (Get) might not returned a Document thus an APM Stop shape after the Disk might not be called
3.  The APM Error in the try catch

The [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm){:target="_blank"} will allow the Instrumentation of any Boomi Processes and will provide with 3 steps: Start, Stop or Error

#### Boomi API Processes

In the context of Grafana Cloud, the Boomi API processes also require manual instrumentation using the [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm){:target="_blank"}. 

Boomi API before changes:

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-process-2.png)  

Boomi API after changes:

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-process-2-apm.png)  

The changes are similar to the changes done in Scheduled Process except that the APM Operations are using different configuration:

- For a Start Operation: 

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-op-start.png)  

The Operation will use the the HTTP W3C Headers send by the API Consumer to correlate the context with current one.

- For a Stop Operation:

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-op-stop.png)

#### Boomi JMS Processes

In the context of Grafana Cloud, the Boomi API processes require a similar manual instrumentation as the API Process. You can also use the same APM Operation as they are working in the context of API and JMS Processes.

Boomi JMS Process before changes:

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-process-3.png)  

Boomi JMS Process after changes:

![APM Instrumented](/assets/boomi-observability-grafanacloud/boomi-process-3-apm.png)  

## Review of the Observability with Grafana Cloud

### Grafana Dashboard

![APM Instrumented](/assets/boomi-observability-grafanacloud/overview.png)  

#### Logs view (Loki)

![Traces](/assets/boomi-observability-grafanacloud/logs.png)

After selecting a line, you can view the details including the traceId

![Traces](/assets/boomi-observability-grafanacloud/logs_details.png)

#### Traces (Tempo)

![Dependencies](/assets/boomi-observability-grafanacloud/logs_traces.png)