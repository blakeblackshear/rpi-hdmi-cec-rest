# Docker image running hdmi-cec-rest for Raspberry Pi
A REST api for your HDMI CEC devices on port 5000. Runs [this go application](https://github.com/bah2830/hdmi-cec-rest).

## Running
You will need to share `/dev/vchiq` with the container:
```
docker run -d --name="hdmi-cec-rest" -v /etc/localtime:/etc/localtime:ro --net=host --device=/dev/vchiq blakeblackshear/rpi-hdmi-cec-rest:latest
```

## Home Assistant
I built this to allow [Home Assistant](https://home-assistant.io) to more easily
talk to devices over HDMI CEC. The native component does not provider a way to
check the status of a device. An example config looks like this, but may vary
depending on your TV.

```
switch:
  - platform: rest
    name: TV
    resource: http://localhost:5000/device/0/power
    body_on: '{"state":"on"}'
    body_off: '{"state":"off"}'
    is_on_template: '{{ value_json.state == "starting" or value_json.state == "on" }}'
```
