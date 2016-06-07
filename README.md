# Deis Monitor v2
[![Docker Repository on Quay](https://quay.io/repository/deisci/grafana/status "Docker Repository on Quay")](https://quay.io/repository/deisci/grafana)
[![Docker Repository on Quay](https://quay.io/repository/deisci/influxdb/status "Docker Repository on Quay")](https://quay.io/repository/deisci/influxdb)
[![Docker Repository on Quay](https://quay.io/repository/deisci/telegraf/status "Docker Repository on Quay")](https://quay.io/repository/deisci/telegraf)

Deis (pronounced DAY-iss) is an open source PaaS that makes it easy to deploy and manage applications on your own servers. Deis builds on [Kubernetes][k8s-home] to provide a lightweight, easy and secure way to deploy your code to production.

For more information about the Deis workflow, please visit the main project page at https://github.com/deis/workflow.

# About
This repository aims to contain all the necessary components for a production quality monitoring solution that runs on top of the kubernetes cluster scheduler. It provides part of the [TICK](https://influxdata.com/time-series-platform/) stack which is produced by the influxdata team.

## Current State
Currently this repo provides only 3 components (Influxdb, Telegraf, and Grafana). Telegraf is the metrics collection agent that runs using the daemon set API. For more infomation please read [this](telegraf/README.md).

Also provided is an Influxdb container which only runs 1 instance of the database. It also does not write any data to the host filesystem so it is not a durable system right now. For more information please read [this](influxdb/README.md)

Lastly, Grafana is a stand alone graphing application. It natively supports Influxdb as a datasource and provides a robust engine for creating dashboards on top of timeseries data. We provide a few out of the box dashboards for monitoring Deis Workflow and Kubernetes but please feel free to use them as a starting point for creating your own dashboards.

# Architecture Diagram

```
                           ┌────────┐                            
                           │ Router │                            
                           └────────┘                            
                               │                    ┌──────┐    
                           Log File         ┌──────▶│Logger│    
                               │            │       └──────┘    
                               ▼            │                   
┌────────┐                ┌─────────┐       │                   
│App Logs│───Log file────▶│ fluentd │──UDP/Syslog               
└────────┘                └─────────┘       │       ┌──────────┐
                                            │       │ stdout   │
                                            └──────▶│  metrics │
┌─────────────┐                                     └──────────┘
│ HOST        │          ┌───────────┐          Wire      │     
│  Telegraf   │────┬────▶│ InfluxDB  │◀───────Protocol────┘     
└─────────────┘    │     └───────────┘                          
                   │           │                                
┌─────────────┐    │           │                                
│ HOST        │    │           ▼                                
│  Telegraf   │────┤     ┌──────────┐                           
└─────────────┘    │     │ Grafana  │                           
                   │     └──────────┘                           
┌─────────────┐    │                                            
│ HOST        │    │                                            
│  Telegraf   │────┘                                            
└─────────────┘                                                                                                                                                                                         
```


## License
Copyright 2013, 2014, 2015, 2016 Engine Yard, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[k8s-home]: http://kubernetes.io/
[issues]: https://github.com/deis/monitor/issues
[prs]: https://github.com/deis/monitor/pulls
