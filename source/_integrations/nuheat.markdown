---
title: NuHeat
description: Instructions on how to integrate your NuHeat Signature thermostats within Home Assistant.
ha_category:
  - Climate
ha_release: 0.61
ha_iot_class: Cloud Polling
ha_domain: nuheat
---

The `nuheat` integration lets control your connected [NuHeat Signature](https://www.nuheat.com/products/thermostats/signature-thermostat) floor heating thermostats from [NuHeat](https://www.nuheat.com/).

There is currently support for the following device types within Home Assistant:

- Climate

First, you will need to obtain your thermostat's numeric serial number or ID by logging into [MyNuHeat.com](https://mynuheat.com/) and selecting your thermostat(s).

Once you have the Thermostat ID(s), add the following information to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
nuheat:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
  devices: 12345

# Example configuration.yaml entry with multiple thermostats
nuheat:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
  devices:
    - 12345
    - 67890
```

{% configuration %}
username:
  description: The username for accessing your MyNuHeat account.
  required: true
  type: string
password:
  description: The password for accessing your MyNuHeat account.
  required: true
  type: string
devices:
  description: The serial number/ID of each thermostat you would like to integrate.
  required: true
  type: [string, integer]
{% endconfiguration %}

## Concepts

The NuHeat Thermostat supports the following key concepts.

The `target temperature` is the temperature that the device attempts to achieve. The target temperature is either determined by the schedule programmed into the thermostat (`auto mode`) or may be overridden. When the target temperature is set by Home Assistant, the thermostat will hold this temperature until the schedule is resumed.

## Attributes

The following attributes are provided by the NuHeat thermostat: `name`, `temperature_unit`, `current_temperature`, `target_temperature`, `current_hold_mode`, `current_operation`, `operation_list`, `min_temp` and `max_temp`.

### Attribute `name`

Returns the name of the NuHeat Thermostat.

| Attribute type | Description |
| ---------------| ----------- |
| String | Name of the thermostat

### Attribute `temperature_unit`

Returns the unit of measurement used for temperature by the thermostat.

| Attribute type | Description |
| ---------------| ----------- |
| String | Name of the temperature unit

### Attribute `current_temperature`

Returns the current temperature measured by the thermostat.

| Attribute type | Description |
| ---------------| ----------- |
| Integer | Currently measured temperature

### Attribute `target_temperature`

Returns the target temperature of the thermostat, when the thermostat is
not in auto operation mode.

| Attribute type | Description |
| ---------------| ----------- |
| Integer | Target temperature

### Attribute `preset_mode`

Returns the current temperature hold, if any.

| Attribute type | Description |
| ---------------| ----------- |
| String | 'temperature', 'temporary_temperature', 'auto', etc.

### Attribute `hvac_action`

Returns the current HVAC mode of the thermostat.

| Attribute type | Description |
| ---------------| ----------- |
| String | 'heat', 'idle'

### Attribute `preset_modes`

Returns the list of available preset modes.

| Attribute type | Description |
| ---------------| ----------- |
| List of String | Available preset modes

### Attribute `min_temp`

Returns the minimum supported temperature by the thermostat

| Attribute type | Description |
| ---------------| ----------- |
| Integer | Minimum supported temperature

### Attribute `max_temp`

Returns the maximum supported temperature by the thermostat

| Attribute type | Description |
| ---------------| ----------- |
| Integer | Maximum supported temperature

## Services

The following services are provided by the NuHeat Thermostat: `set_temperature`, `set_hvac_mode`, `set_preset_mode`, `resume_program`.

### Service `climate.set_hvac_mode` ([Climate integration](/integrations/climate/))

NuHeat Thermostats do not have an off concept. Setting the temperature to `min_temp` and changing the mode to `heat` will cause the device to enter a `Permanent Hold` preset and will stop the thermostat from turning on unless you happen to live in a freezing climate.

### Service `climate.set_temperature` ([Climate integration](/integrations/climate/))

If the thermostat is in auto mode, it puts the thermostat into a temporary hold at the given temperature.

If the thermostat is in heat mode, it puts the thermostat into a permanent hold at the given temperature.

### Service `climate.set_preset_mode` ([Climate integration](/integrations/climate/))

The following presets are available: `Run Schedule`, `Temporary Hold`, `Permanent Hold`.

### Service `nuheat.resume_program`

Resumes the currently active schedule.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | yes | String or list of strings that point at `entity_id`'s of climate devices to control. Use `entity_id: all` to target all.
