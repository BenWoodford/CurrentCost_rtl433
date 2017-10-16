# rtl433test
A Docker image for a software defined radio tuned to listen for Current Cost power consumption sensors at 433MHz.  This is based off of Marco Verleun's 
awesome rtl2mqtt image, and chriskacerguis honeywell2mqtt.

## Usage

To run the container, use the following:

```
sudo docker run --name rtl_433 -d \
-e MQTT_HOST=<mqtt-broker.example.com> \
-e MQTT_USER=username \
-e MQTT_PASS=password \
-e MQTT_TOPIC=your/topic/name \
--privileged -v /dev/bus/usb:/dev/bus/usb \
--name hijinx/CurrentCost_rtl433
```

## MQTT Data

Data to the MQTT server will look like this

```json
{
    "time" : "2017-10-16 20:53:09", 
    "model" : "CurrentCost TX", 
    "dev_id" : 3063, 
    "power0" : 617,
    "power1" : 0, 
    "power2" : 0
}
```

Its power0 that provides the reading that is desired.

**The default topic is:** ```homeassistant/sensor/currentcost```

## Hardware

This has been tested and used with the Current Cost EnviR with clamp - e.g.:
http://i.ebayimg.com/images/g/bzcAAOSwZuRZu6JB/s-l1600.jpg


## Troubleshooting

If you see this error:

> Kernel driver is active, or device is claimed by second instance of librtlsdr.
> In the first case, please either detach or blacklist the kernel module
> (dvb_usb_rtl28xxu), or enable automatic detaching at compile time.

Then run the following command on the host

```bash
sudo rmmod dvb_usb_rtl28xxu rtl2832
```

Also it may be required to determine tuning offset (ppm) per this instruction:
http://davidnelson.me/?p=371
(Currently the ppm for my receiver is hardcoded in the script... should be an env var really).
