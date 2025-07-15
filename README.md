# Mosquitto & Monk

This repository contains Monk.io template to deploy Mosquitto MQTT Broker either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

## Make sure monkd is running

```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository

```bash
git clone git@github.com:monk-io/monk-mosquitto.git
```

## Load Template

```bash
cd monk-mosquitto
monk load MANIFEST
```

```bash
foo@bar:~$ monk list monk-mosquitto
✔ Got the list
Type      Template                Repository  Version  Tags
runnable  mosquitto/base         local       -        mqtt, broker, iot, messaging, publish-subscribe, lightweight, ssl, tls, open source
runnable  mosquitto/mosquitto-broker  local       -        mqtt, broker, iot, messaging, publish-subscribe, lightweight, ssl, tls, open source
```

## Deploy Stack

```bash
foo@bar:~$ monk run mosquitto/mosquitto-broker
✔ Starting the run job: local/mosquitto/mosquitto-broker... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container 3cecf5dddbe0c8b614e98f2c3a1aae09-local-mosquitto-mosquitto-broker-broker created DONE
✔ Started local/mosquitto/mosquitto-broker
🔩 templates/local/mosquitto/mosquitto-broker
 └─🧊 Peer local
    └─🔩 templates/local/mosquitto/mosquitto-broker
       └─📦 3cecf5dddbe0c8b614e98f2c3a1aae09-local-mosquitto-mosquitto-broker-broker running
          ├─🧩 eclipse-mosquitto:latest
          └─💾 /var/lib/monkd/volumes/mosquitto -> /mosquitto/data

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/mosquitto/mosquitto-broker - Inspect logs
        monk shell     local/mosquitto/mosquitto-broker - Connect to the container's shell
        monk do        local/mosquitto/mosquitto-broker/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Access MQTT Broker

The MQTT broker will be available on:
- **MQTT Port**: 1883
- **WebSocket Port**: 9001

You can test the connection using any MQTT client like mosquitto_pub/mosquitto_sub:

```bash
# Subscribe to a topic
mosquitto_sub -h localhost -p 1883 -t test/topic

# Publish to a topic
mosquitto_pub -h localhost -p 1883 -t test/topic -m "Hello World"
```

## Variables

The variables are in `mosquitto.yml` file. You can quickly setup by editing the values here.

| Variable             | Description                    | Default     |
| -------------------- | ------------------------------ | ----------- |
| mqtt-port            | MQTT Protocol Port             | 1883        |
| websocket-port       | WebSocket Port                 | 9001        |
| allow-anonymous      | Allow anonymous connections    | true        |
| max-connections      | Maximum connections (-1=unlimited) | -1      |
| log-level            | Logging level                  | information |
| persistence          | Enable message persistence     | true        |
| websocket-enabled    | Enable WebSocket support       | true        |
| retain-available     | Enable message retention       | true        |

## Stop, remove and clean up workloads and templates

```bash
monk purge -x -a
``` 