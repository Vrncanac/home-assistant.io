---
title: Axion Lighting
description: Integration for controlling Axion Lighting systems using DMX protocol.
ha_category:
  - Lighting
ha_release: '2024.08'
ha_iot_class: Local Polling
ha_config_flow: true
ha_codeowners:
  - '@Vrncanac'
ha_domain: axion_dmx
ha_integration_type: integration
---

This integration enables control of the complete [Axion Lighting](https://axionlighting.com/) ecosystem via the Axion PoE & Wi-Fi DMX Controller. Using Home Assistant, you can add White, Tunable White, RGB, RGBW, and RGBWW lighting loads. The module provides accurate brightness scaling, quick responsiveness, and easy integration of any DMX address from 1-512.

## Prerequisites

To use this integration, you will need an Axion Lighting system that supports DMX control. Ensure that your DMX controller is properly connected to your Home Assistant instance.

### Network Requirements

- The DMX controller must be on the same network as Home Assistant.
- The controller should have a static IP or a DHCP reservation.
- Ensure that firewall settings allow communication on the necessary ports (consult Axion Lighting documentation for specific port requirements).
- IPv4 is required; IPv6 support is currently unknown.

{% include integrations/config_flow.md %}

### Configuration Options

The Axion Lighting integration requires the following parameters during setup:

| Parameter      | Required | Description |
|--------------|----------|-------------|
| `ip_address` |  Yes  | The IPv4 address of the Axion DMX controller. IPv6 is currently unsupported. |
| `password`   |  Yes  | The password required to authenticate with the DMX controller (minimum 8 characters, mix of letters and numbers recommended). |
| `dmx_channel` |  Yes  | The starting DMX channel for the light entity. Valid range: 1-512. |
| `light_type` |  Yes  | The type of light entity to control. Valid options: `White`, `Tunable White`, `RGB`, `RGBW`, `RGBWW`. |

These options must be provided during the configuration process through Home Assistant’s UI. Once set up, they can be modified in the integration settings.

## Light Entities

This integration supports the following light entities:

- **White**: Controls the brightness of a single white channel.
- **Tunable White**: Controls the temperature of white light by adjusting the balance between warm and cool white channels.
- **RGB**: Controls Red, Green, and Blue channels with brightness scaling.
- **RGBW**: Controls Red, Green, Blue, and White channels with brightness scaling.
- **RGBWW**: Controls Red, Green, Blue, Warm White, and Cold White channels with brightness scaling.

### Usage

Once configured, your Axion Lighting devices will appear in Home Assistant, and you can control the lights using the UI. The integration supports standard Home Assistant light services, such as turning lights on/off, adjusting brightness, and changing colors (for RGB, RGBW, and RGBWW modes).

#### Example Configuration

```yaml
light:
  - platform: axion_dmx
    ip_address: "192.168.1.50"
    password: "your_password"
    dmx_channel: 1
    light_type: "RGBW"
```

#### Example UI Controls

<p class='img'>
  <img src='/images/screenshots/Axion_lighting_localUI.png' alt='Screenshot of Axion Lighting UI displaying integration options' />
</p>

### Services

#### axion_dmx.set_level

Set the brightness of the light entity.

| Parameter    | Description |
|-------------|-------------|
| `channel`   | The DMX channel number (1-512) to be controlled. |
| `brightness` | The brightness level (0-255) to apply. |

**Example YAML Call:**

```yaml
service: axion_dmx.set_level
data:
  channel: 1
  brightness: 128
```

#### axion_dmx.set_color

Set the color of the light entity using DMX channels - 3 channels will be occupied.

| Parameter    | Description |
|-------------|-------------|
| `channel`   | The DMX channel number (1-512) to be controlled. |
| `rgb_color` | The color to set, specified as an RGB value. |

**Example YAML Call:**

```yaml
service: axion_dmx.set_color
data:
  channel: 2
  rgb_color: [255, 100, 50]
```

#### axion_dmx.set_rgbw

Set the RGBW color of the light entity using DMX channels - 4 channels will be occupied.

| Parameter    | Description |
|-------------|-------------|
| `channel`   | The DMX channel number (1-512) to be controlled. |
| `rgbw_color` | The color to set, specified as an RGBW value. |

**Example YAML Call:**

```yaml
service: axion_dmx.set_rgbw
data:
  channel: 3
  rgbw_color: [255, 100, 50, 200]
```

#### axion_dmx.set_rgbww

Set the RGBWW color of the light entity using DMX channels - 5 channels will be occupied.

| Parameter    | Description |
|-------------|-------------|
| `channel`   | The DMX channel number (1-512) to be controlled. |
| `rgbww_color` | The color to set, specified as an RGBWW value. |

**Example YAML Call:**

```yaml
service: axion_dmx.set_rgbww
data:
  channel: 4
  rgbww_color: [255, 100, 50, 200, 180]
```

### Error Handling

- Ensure the `channel` value is within the range of 1-512.
- Verify the `rgb_color`, `rgbw_color`, and `rgbww_color` values are in the correct format (list of integers between 0-255).
- If authentication fails, check that the `password` is correct and meets security recommendations.

## Removal Instructions

To remove the Axion Lighting integration from Home Assistant:

1. Navigate to **Settings > Devices & Services**.
2. Locate the **Axion Lighting** integration.
3. Click **Options** (three-dot menu) and select **Delete**.
4. Confirm the removal.

This will delete the integration and remove all associated entities from Home Assistant. If needed, you can re-add the integration at any time following the installation steps.

---
