<div align="center">

# switchboard

#### A syslog monitor to relay incoming call events from a Grandstream ATA to Home Assistant

[![](https://img.shields.io/github/license/noahhusby/switchboard)](https://github.com/noahhusby/switchboard/blob/main/LICENSE)

</div>

This is a Home Assistant add-on that listens for incoming calls on Grandstream ATAs and redirects the request to a Home Assistant webhook. This project was created to be able to display the incoming calls on my LG TVs using the notify service.

## Supported Devices
- Grandstream HT801
- Grandstream HT801 v2
- Grandstream HT802
- Grandstream HT802 v2
- Grandstream HT812
- Grandstream HT812 v2
- Grandstream HT813
- Grandstream HT814
- Grandstream HT814 v2
- Grandstream HT818
- Grandstream HT818 v2
- Grandstream HT841/HT881

## Installation

> [!IMPORTANT]  
> Only Home Assistant OS is supported for this addon. The Dockerfile and program can be reworked to run as a seperate docker container as well, but that is not a supported installation method.

### Install the addon
1. Manually copy the files in the `/switchboard` directory to the `/addons` folder in your local Home Assistant installation. 
2. Navigate to the Addons menu, click refresh and install.

### Configure the ATA
1. Login to your Grandstream ATA. 
2. Under `Advanced Settings`, scroll down to the Syslog box and enter the IP address of your Home Assistant instance.

### Create an automation to be executed on incoming call

Example automation:

```yaml
alias: TV Phone
description: ""
triggers:
  - trigger: webhook
    allowed_methods:
      - POST
      - PUT
    local_only: false
    webhook_id: incoming_call
actions:
  - action: notify.basement_tv
    metadata: {}
    data:
      message: |
        {% if trigger.json.caller_name is defined %}
          Incoming call from: {{ trigger.json.caller_name }} ({{ trigger.json.phone_number }})
        {% else %}
          Incoming call from: {{ trigger.json.phone_number }}
        {% endif %}
mode: single
```