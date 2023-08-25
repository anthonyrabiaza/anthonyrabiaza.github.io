---
layout: post
title:  "Boomi Observability with AWS CloudWatch and X-Ray"
permalink: /boomi-observability-cloudwatch/
---
Implementation with a Monitoring Platform for Observability with AWS CloudWatch and X-Ray
===========================================================

## Overview

![Boomi AtomSphere](/assets/boomi-observability-aws-cloudwatch-xray/cloudwatch+x-ray.png)

## CloudWatch Stack Installation

The first step consist of installing the CloudWatch elements on each server:

- CloudWatch Agent (see [installation steps](https://CloudWatch.com/docs/CloudWatch-cloud/agent/){:target="_blank"})
- Fluent-D with CloudWatch plugin (see [installation steps](https://github.com/fluent-plugins-nursery/fluent-plugin-cloudwatch-logs){:target="_blank"} and [sample configuration](https://github.com/anthonyrabiaza/opentelemetry-log-parser#promtail-configuration){:target="_blank"})
- AWS Distro for OpenTelemetry Collector for CloudWatch (see [installation steps](https://docs.aws.amazon.com/eks/latest/userguide/configure-cw.html){:target="_blank"} with a [sample configuration](https://github.com/anthonyrabiaza/opentelemetry-log-parser#configure-opentelemetry-collector){:target="_blank"})
- Prometheus Exporter (see [installation steps](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-PrometheusEC2.html#CloudWatch-Agent-Prometheus-Java){:target="_blank"})

This step will link the server to CloudWatch Cloud and will allows the gathering of logs, metrics, status of server, processes, network, etc.

## Java Agent Installation

The second step consist of installing the Java Agent on each Boomi runtime. This step will link the Java server to the OpenTelemetry Collector and will allows the gathering of JVM metrics and sending of traces.

## APM Setup

The CloudWatch Cloud Java Agent (Jar) needs to be deployed on each node or deployed on the Shared Server accessible by all nodes. Once this is done, Boomi System properties needs to be updated via Boomi AtomSphere:  

![Boomi AtomSphere](/assets/boomi-observability-aws-cloudwatch-xray/boomi-config-cloudwatch.png)

### APM Configuration: Tracing

#### Boomi Scheduled Processes

The Boomi Scheduled processes will be detected by CloudWatch Cloud APM but requires a manual instrumentation using [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm){:target="_blank"} to add the tracing metadata:  

Initial process:

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-process-1.png)  

The updated process will looks like the following:  

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-process-1-apm.png)  

The changes includes:

1.  The APM Start shape at the beginning
2.  The APM Stop shape before the last End, please note that we created a branch here as the Disk shape (Get) might not returned a Document thus an APM Stop shape after the Disk might not be called
3.  The APM Error in the try catch

The [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm){:target="_blank"} will allow the Instrumentation of any Boomi Processes and will provide:

1.  Tracing: 3 steps: Start, Stop or Error
2.  Eventing, in Stop or Error steps and event can be sent to CloudWatch Cloud to inform of the success or failure of the process (including all details: server, time, environment, execution context...)
3.  Custom Metrics: a Boomi Process can send metrics related to business data or systems to CloudWatch Cloud

#### Boomi API Processes

In the context of CloudWatch Cloud, the Boomi API processes also require manual instrumentation using the [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm){:target="_blank"}. 

Boomi API before changes:

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-process-2.png)  

Boomi API after changes:

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-process-2-apm.png)  

The changes are similar to the changes done in Scheduled Process except that the APM Operations are using different configuration:

- For a Start Operation: 

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-op-start.png)  

The Operation will use the the HTTP W3C Headers send by the API Consumer to correlate the context with current one.

- For a Stop Operation:

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-op-stop.png)

#### Boomi JMS Processes

In the context of CloudWatch Cloud, the Boomi API processes require a similar manual instrumentation as the API Process. You can also use the same APM Operation as they are working in the context of API and JMS Processes.

Boomi JMS Process before changes:

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-process-3.png)  

Boomi JMS Process after changes:

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/boomi-process-3-apm.png)  

## Review of the Observability with CloudWatch Cloud

### CloudWatch X-Ray Traces overview

![APM Instrumented](/assets/boomi-observability-aws-cloudwatch-xray/cloudwatch-traces.png)  

#### After selecting a trace

![Traces](/assets/boomi-observability-aws-cloudwatch-xray/cloudwatch-trace-overview.png)

#### View of annotations

Select the first "ExecutionTask.call" segment to see Resources, Annotations, Metadata or Exception 

![Traces](/assets/boomi-observability-aws-cloudwatch-xray/cloudwatch-trace-annotations.png)

You can now scroll down to the Logs associated to this trace (traces and logs need to be linked using the Boomi Log Framework)

![Traces](/assets/boomi-observability-aws-cloudwatch-xray/cloudwatch-trace-and-logs.png)

#### View of the logs (only)

To view more details on the logs, click on "View on CloudWatch Logs Insights"

![Dependencies](/assets/boomi-observability-aws-cloudwatch-xray/cloudwatch-logs-screen.png)

Click on "Run query"

![Dependencies](/assets/boomi-observability-aws-cloudwatch-xray/cloudwatch-logs-only.png)