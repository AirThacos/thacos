# THACOS

## Prerequisites and assumptions

The only hard dependency for now is `docker`.

All the services defined in the `docker-compose.yml` file currently run on a Raspberry Pi 4B.

To avoid the need of a public IP, DNS setup, firewall/NAT configuration, the Pi also runs `cloudflared` and makes use of [Cloudflare Tunnels](https://www.cloudflare.com/products/tunnel/) and [Cloudflare Access](https://www.cloudflare.com/products/zero-trust/access/) (for secure access).

The DNS zone management is also done on their platform.

Provisioning scripts for the Cloudflare setup is not part of the (initial) scope and should be done by hand for the first iteration.

## Included services

The following services are included in the `docker-compose.yml` file:

| service name                                     | local bind address | public URL                                                              | description                                                            |
| ------------------------------------------------ | ------------------ | ----------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [ESPHome](https://esphome.io/)                   | `localhost:6052`   | [esphome.for-work.space](https://esphome.for-work.space/)               | ESP firmware management                                                |
| [Home Assistant](https://www.home-assistant.io/) | `localhost:8123`   | [home-assistant.for-work.space](https://home-assistant.for-work.space/) | device control and orchestration (and other automation, in the future) |
| [InfluxDB](https://www.influxdata.com/)          | `localhost:8086`   | [influxdb.for-work.space](https://influxdb.for-work.space/)             | the TSDB for persistant data storage                                   |
| [Grafana](https://grafana.com/)                  | `localhost:3000`   | [grafana.for-work.space](https://grafana.for-work.space/)               | dashboards (and alerts, in the future)                                 |
| [mosquitto](https://mosquitto.org/)              | `localhost:1883`   | -                                                                       | MQTT broker                                                            |

## Getting started

-   clone the repository
-   run `chown -R 1883:1883 mosquitto`
-   run `cp .env.example .env`
-   update the required values in the `.env`
-   run `DOCKER_INFLUXDB_INIT_MODE=setup docker compose up influxdb` to run the influxdb setup script
-   open the influxdb admin UI and get the organization ID and put it in `INFLUXDB_ORG_ID` in the `.env` file
-   run `docker compose up` to run all the services

## TODO

-   include a PM sensor and configure the firmware and dashboards
-   include `telegraf` as a colector (the HA influxdb only saves data points when values change)
-   setup reproductibility improvements:
    -   provision grafana: dashboards, datasources and alerts
    -   provision cloudflare tunnel setup
