---
layout: post
title:  "Realtime Event Webhook driver"
download: https://github.com/harperreed/control4-drivers/releases/download/realtime-event-webhooks-3/realtime-event-webhook.c4z
filename: realtime-event-webhook.c4z
release_notes: https://github.com/harperreed/control4-drivers/releases/tag/realtime-event-webhooks-3
release_name: Added deviceType to the webhook payload
version: 3
---

**Update 11/22**

This driver is a simple realtime event webhook driver. 

It will register events for lighting, thermostat, motion, locks, and fans. When an event occurs, it will send an event notification via webhook with all of the relevant variables associated with that event and device.

The best part is that it does it via internal events and doesn't require polling. This should be better for Control4 and be much more performant. It also allows the events to execute the exact moment that it happens in Control4. 

## Getting started

First you will need to add the driver to your Control4 install. The easiest and best way is to reach out to your dealer.

Once the driver is installed you can access it via Composer HE or Pro. 

All you need to get started is a URl to accept the payload. Set that in the drive properties and then move to the actions tab and click "`Register Device events.`" This will register the event handler to send a webhook to the specified URL on an event triggering.  

If the driver doesn't receive a Status Code: 200 from the webhook URL it will deregister the events. 

You can test the webhook code by click on the "`Test Webhook`" action.
 
## Example Payload

An example JSON payload looks like this: 

    {
      "event" : {
          "description" : "Current light level (0% - 100%)",
          "deviceId" : 314,
          "deviceName" : "Cans",
          "deviceType" : "light",
          "eventId" : 1001,
          "eventValue" : "10",
          "hidden" : "0",
          "name" : "LIGHT_LEVEL",
          "readonly" : "0",
          "roomId" : 46,
          "roomName" : "Bathroom",
          "type" : "2"
      }
    }

You can use this to trigger events in another app that sits outside of your home or inside of your network. 

## Changes

* 11/22 - Added support for deviceType in the payload
* 11/20 - Initial release

## What are webhooks? 

A good place to get started with webhooks is [here](https://webhooks.pbworks.com/w/page/13385124/FrontPage). For debugging, I recommend [RequestBin](https://requestb.in)
