---
layout: post
title:  "Prometheus Logger"
download: https://github.com/harperreed/control4-drivers/releases/download/prometheus-1/Prometheus.Logger.c4z
filename: Prometheus.Logger.c4z
release_notes: https://github.com/harperreed/control4-drivers/releases/tag/prometheus-1
release_name: Initial Release
version: 1
---

This driver is for integrating with the [Prometheus](https://prometheus.io/docs/prometheus/latest/getting_started/) Monitoring system & time series database. 

It will log device events and create a metrics endpoint that Prometheus can scrape to build dashboards based on your Control4 system.

It is VERY simple and shouldn't impact the performance of Control4 at all. 

To get started - simply install the driver. The driver will freeze your Control4 system for a couple seconds while it is querying your devices. 

To make sure it is working, You can test this by opening up: http://yourcontrollerip:9145 in a browser. You should see something like this:

    # HELP motion help
    # TYPE motion gauge

    motion{roomName="roomName",deviceName="deviceName",state="state"} 0
    # HELP blind help
    # TYPE blind gauge

    blind{roomName="roomName",deviceName="deviceName",state="state"} 0
    # HELP lock help
    # TYPE lock gauge

    lock{roomName="roomName",deviceName="deviceName",state="state"} 0

Once you have confirmed that the driver is working - You can move on to getting the metrics into prometheus. 

## Prometheus. 

To set up Prometheus to use your Control4 system as an endpoint, you will need to add this to the  `scrape_configs:` section of your `prometheus.yml`

    - job_name: 'control4'
      honor_labels: true
      static_configs:
        - targets: ['IP_OF_YOUR_CONTROL4:8082']
      metrics_path: /

Once this is set up - check in the **targets** section of your Prometheus instance and see if it shows up. 

It should look like this:

![](/imgs/prometheus-target.png)


Once that is A-OK you can start incorporating your Control4 metrics into however you use Prometheus. 

There are seven types of metrics that you can track: 

* blind
* security
* light
* thermostat
* fan
* motion
* lock

You can query any number of queries based on these metrics. 

For example: 

Here is a Prometheus query for `light{roomName="Office"}` and a graph for `light{roomName="Office", state="LIGHT_LEVEL"}`

![](/imgs/prometheus-graph.png)

Prometheus is good for testing queries and what not - but [Grafana](https://grafana.com/) works as an amazing dashboard. 

## Grafana

Here are some examples of using Grafana:

### Individual lights

![](/imgs/grafana-lights.png)

### All Lights currently on in a graph

![](/imgs/grafana-lights2.png)

### OS Sensors

![](/imgs/grafana-motion.png)


### Thermostats

![](/imgs/grafana-therm.png)

### Doors and Alarm zones

![](/imgs/grafana-doors-alarm.png)

As you can see - there is a lot of potential with tracking values coming directly off of Control4. 

Let me know how this driver works for you. 

