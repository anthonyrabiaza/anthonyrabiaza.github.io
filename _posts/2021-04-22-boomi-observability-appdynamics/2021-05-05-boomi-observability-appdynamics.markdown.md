---
layout: post
title:  "Boomi Observability with AppDynamics"
permalink: /boomi-observability-appdynamics/
---
Implementation with a Monitoring Platform for Observability with AppDynamics
===========================================================
## Overview

![Boomi AtomSphere](/assets/boomi-observability-appdynamics/appdynamics.png)

## Machine Agent Installation

The first step consist of installing the AppDynamics Agent on each server. This step will link the server (or Cloud resource) to AppDynamics and will allows the gathering of metrics, status of server, processes, network, etc.

## Java Agent Installation

The second step consist of installing the AppDynamics Java Agent on each Boomi runtime. This step will link the Java server to AppDynamics and will allows the gathering of JVM metrics and traces.

## Transaction Detection

This part is optional and will allow to detect any process execution in Boomi. If you are using the Boomi APM Connector or only monitoring APIs and JMS Processes, you can skip this part.

![Boomi AtomSphere](/assets/boomi-observability-appdynamics/transaction-detection-a.png)

![Boomi AtomSphere](/assets/boomi-observability-appdynamics/transaction-detection-b.png)

![Boomi AtomSphere](/assets/boomi-observability-appdynamics/transaction-detection-c.png)

## Backend Detection

![Boomi AtomSphere](/assets/boomi-observability-appdynamics/backend-detection-a.png)

![Boomi AtomSphere](/assets/boomi-observability-appdynamics/backend-detection-b.png)

## APM Setup

The AppDynamics Java Agent (Jar) needs to be deployed on each node or deployed on the Shared Server accessible by all nodes, once this is done Boomi System properties needs to be updated via Boomi AtomSphere:  

![Boomi AtomSphere](/assets/boomi-observability-appdynamics/boomi-config-appdynamics.png)

All the other configuration elements like controller-host, controller-port, account-name and account-key should remain in the controller-info.xml

Example of controller-info.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<controller-info>
...  
    <controller-host>bear2021XYZXYZ.saas.appdynamics.com</controller-host>
    <controller-port>443</controller-port>
    <controller-ssl-enabled>true</controller-ssl-enabled>
    <use-simple-hostname>false</use-simple-hostname>
    <application-name></application-name>
    <tier-name></tier-name>
    <node-name></node-name>
...
    <!-- Location of the Machine Agent which can be installed on each node-->
    <agent-runtime-dir>/opt/appdynamics/machine-agent</agent-runtime-dir>

    <enable-orchestration>false</enable-orchestration>
...
    <account-name>bear2021XYZXYZ</account-name>
    <account-access-key>abcdefghijkl</account-access-key>
...
</controller-info>
```

### APM Configuration: Tracing and Custom Data Collector

#### Boomi API Processes and Boomi JMS Processes

In the context of AppDynamics, the Boomi API processes do not require any change, they will be automatically detected by AppDynamics and **instrumentation will be required only to get advanced metadata** (see next section).  
The operation name for instance /ws/rest/apm/test/MQ/ and the HTTP method will help us identifying the API process.

#### Boomi Custom Library

1. For instrumentation, you need to download an archive from AppDynamics: for instance java-agent-api-20.6.0.30246.zip
2. Unzip the package
3. Rename agent-api.jar to appdynamics-agent-api.jar
4. Upload the Jar to Boomi>Settings>Account Libraries
![APM Instrumented](/assets/boomi-observability-appdynamics/customlib-upload-jar.png)  
5. Create a Custom Library and Deploy it to the Environment which will be monitored by AppDynamics
![APM Instrumented](/assets/boomi-observability-appdynamics/customlib-create.png)

#### Boomi Scheduled Processes

The Boomi Scheduled processes will also be detected by AppDynamics due to the configuration in Boomi AtomSphere. 

But the name of the process and other details won't be available. An manual instrumentation of the process will be required to get this details: the use of [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm) will minimize the changes:  

Initial process:

![APM Instrumented](/assets/boomi-observability-appdynamics/boomi-process-3.png)  

The updated process will looks like the following:  

![APM Instrumented](/assets/boomi-observability-appdynamics/boomi-process-1-apm.png)  

The changes includes:

1.  The APM Start shape at the beginning
2.  The APM Stop shape before the last End, please note that we created a branch here as the Disk shape (Get) might not returned a Document thus an APM Stop shape after the Disk might not be called
3.  The APM Error in the try catch

The [Boomi APM Connector](https://github.com/anthonyrabiaza/boomiapm) will allow the Instrumentation of any Boomi Processes and will provide:

1.  Tracing: 3 steps: Start, Stop or Error
2.  Eventing, in Stop or Error steps and event can be sent to AppDynamics to inform of the success or failure of the process (including all details: server, time, environment, execution context...)
3.  Custom Metrics: a Boomi Process can send metrics related to business data or systems to AppDynamics

## Review of the Observability with AppDynamics

### View of Dashboard

![APM Instrumented](/assets/boomi-observability-appdynamics/dashboard.png)  

#### Top Business Transactions

![Traces](/assets/boomi-observability-appdynamics/top-business-tx.png)

####   Details of API Call to all back-end systems
We have an end-to-end view of the API Call:

![API Call](/assets/boomi-observability-appdynamics/transaction-detail.png)

Waterfall view:

![API Call](/assets/boomi-observability-appdynamics/transaction-waterfall.png)

#### Detail of Scheduled Processes

We have an end-to-end view of the Process:

![Batch](/assets/boomi-observability-appdynamics/transaction-2-detail.png)

Waterfall view:

![API Call](/assets/boomi-observability-appdynamics/transaction-2-waterfall.png)

Metadata (added by the APM Connector):

![API Call](/assets/boomi-observability-appdynamics/transaction-2-datacollector.png)