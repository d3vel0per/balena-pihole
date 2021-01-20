# balena-pihole

If you're looking for a way to quickly and easily get up and running with a [Pi-hole](https://pi-hole.net/) device for your home network, this is the project for you.

This project is a [balenaCloud](https://www.balena.io/cloud) stack with the following services:

- [Pi-hole](https://pi-hole.net/)
- [PADD](https://github.com/pi-hole/PADD)
- [Unbound](https://unbound.net)

balenaCloud is a free service to remotely manage and update your Raspberry Pi through an online dashboard interface, as well as providing remote access to the Pi-hole web interface without any additional configuation.

## Getting Started

You can one-click-deploy this project to balena using the button below:

[![deploy button](https://balena.io/deploy.svg)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/klutchell/balena-pihole&defaultDeviceType=raspberrypi3)

## Manual Deployment

Alternatively, deployment can be carried out by manually creating a [balenaCloud account](https://dashboard.balena-cloud.com) and application, flashing a device, downloading the project and pushing it via either Git or the [balena CLI](https://github.com/balena-io/balena-cli).

### Device Variables

Device Variables apply to all services within the application, and can be applied fleet-wide to apply to multiple devices. If you used the one-click-deploy method, the default environment variables will already be added for you to customize as needed.

| Name                | Example            | Purpose                                                                                                                                                                                                                          |
| ------------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TZ`                | `America/Toronto`  | To inform services of the timezone in your location, in order to set times and dates within the applications correctly. Find a [list of all timezone values here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). |
| `DNSMASQ_LISTENING` | `eth0`             | We set this to `eth0` to indicate we want DNSMASQ to listen on the ethernet interface of the Raspberry Pi. If you're connecting to your network with WiFi replace this with `wlan0`                                              |
| `INTERFACE`         | `eth0`             | As above.                                                                                                                                                                                                                        |
| `WEBPASSWORD`       | `mysecretpassword` | _(optional)_ password for accessing the web-based interface of Pi-hole - you won’t be able to access the admin panel without defining a password here.                                                                           |
| `DNS1`              | `127.0.0.1#5053`   | _(optional)_ Tell Pi-hole where to forward DNS requests that aren’t blocked. We’re using the [Unbound](https://unbound.net) project here but you can specify your own.                                                           |
| `DNS2`              | `127.0.0.1#5053`   | _(optional)_ Secondary DNS server - see above.                                                                                                                                                                                   |
| `ServerIP`          | `x.x.x.x`          | _(recommended)_ Set to your server's LAN IP, used by web block modes and lighttpd bind address.                                                                                                                                  |

## Usage

### Pi-hole

Check out our blog post on how to deploy network-wide ad-blocking with Pi-hole:

<https://www.balena.io/blog/deploy-network-wide-ad-blocking-with-pi-hole-and-a-raspberry-pi/>

Connect to the Pi-hole admin interface at `http://device-ip/admin` or enable
the Public Device URL in the dashboard and append `/admin` to device URL.

### PADD

Check out this blog post to add a display to your Pi-hole for monitoring and stats:

<https://www.balena.io/blog/add-a-display-to-your-pi-hole-for-monitoring-and-stats/>

All of the below values should be set in the device configuration within balenaCloud.

|Name|Value|Purpose|
|---|---|---|
|RESIN_HOST_CONFIG_gpu_mem|`64`|Note: this is one of the default options so you can edit the existing value. Allocates the specified amount of RAM (in MB) to the GPU. The minimum you'll be able to get away with will vary depending on the resolution you're running at.|
|RESIN_HOST_CONFIG_dtoverlay|`pitft35-resistive,rotate=270,speed=25000000,fps=20`|To load the driver for the display and set the rotation correctly (this may change depeding on your display).|
|RESIN_HOST_CONFIG_hdmi_cvt|`480 320 60 1 0 0 0`|CVT means custom video timings, so we're using this to set a custom resolution for the display.|
|RESIN_HOST_CONFIG_hdmi_force_hotplug|`1`|Setting this means the Pi will operate as if a monitor was connected via HDMI - we then copy the HDMI output to the PiTFT.|
|RESIN_HOST_CONFIG_hdmi_group|`2`|This is essentially specifying that we are working with a monitor rather than a TV.|
|RESIN_HOST_CONFIG_hdmi_mode|`87`|Mode 87 is an unspecified mode allowing us to specify a custom one (with CVT above).|

After you've set these configuration variables and rebooted the device, the software should detect the display and automatically enable the PADD output.

### Unbound

The included config should work for Unbound and was taken mostly from this guide:

<https://docs.pi-hole.net/guides/unbound/>

## Help

If you're having trouble getting the project running,
submit an issue or post on the forums at <https://forums.balena.io>.

## Contributing

Please open an issue or submit a pull request with any features, fixes, or changes.
