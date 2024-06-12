# RPi MQTT Monitor for OpenMower and Home Assistant

> [!CAUTION]
> This project has been deprecated in favor of [rpimon](https://github.com/2m/rpimon).

This repository contains configuration for [rpi-mqtt-monitor] to be used with an [OpenMower].

[rpi-mqtt-monitor]: https://github.com/hjelev/rpi-mqtt-monitor
[OpenMower]:        https://openmower.de/

## Usage

### Running on the OpenMower

Copy the [`monitor.service`][monitor-service] file to `/etc/systemd/system/` on your OpenMower.

Then enable and start the service:

```bash
sudo systemctl enable monitor.service
sudo systemctl start monitor.service
```

This will start publishing configured sensor values to OpenMower MQTT broker:

![mqttui][]

[monitor-service]: ./monitor.service
[mqttui]:          ./docs/mqttui.png

### Configuring Home Assistant MQTT bridge

[rpi-mqtt-monitor] already publishes Home Assistant discovery messages. So the only thing you need to do is to configure the MQTT bridge. Add the following configuration to your Home Assistant MQTT add-on:

```
connection openmower-to-homeassistant
address openmower.local:1883
bridge_protocol_version mqttv50
topic # in 0 monitor/ monitor/
topic # in 0 homeassistant/ homeassistant/
```

This will forward all monitoring and discovery messages from OpenMower to Home Assistant. Restart the MQTT add-on and you will see a new device under MQTT integration:

![device][]

[device]: ./docs/device.png
