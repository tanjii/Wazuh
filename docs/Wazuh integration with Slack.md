In order to integrate Wazuh alerts with Slack, following steps need to be followed. 

## Enabling Incoming Webhook for Slack channel

This integration uses Slack incoming webhooks and allows security professionals to receive real-time alerts directly within designated channels.

Detailed explanation can be found on the following link - https://docs.slack.dev/messaging/sending-messages-using-incoming-webhooks/ 



Steps: 

1. Create an app on the following link - https://api.slack.com/apps?new_app=1

2. Enable incoming webhooks, this can be accessed via Management dashboard → Select App → Incoming Webhooks → toggle Activate Incoming Webhooks

3. Create an incoming webhook, click on Add New Webhook to Workspace and select a channel previously created for this purpose. 

4. Webhook URL will be available, should look something like this: 
```https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX```

## Wazuh Integration with Slack 

To integrate Slack with Wazuh, ossec.conf file needs to be updated.

### Option 1 - not permanent
It is the easiest to edit it via Wazuh UI.  
Go to Wazuh -> Server management -> Settings → Edit configuration for Wazuh manager.
However doing it through Wazuh UI will not create a permanent changes to ossec.conf file, and every time docker containers are recreated, the settings applied here will be lost. 

### Option 2 - Permanent

For the permanent option, ossec.conf file must be changed. You must ssh into wazuh instance, edit the wazuh-manager.conf file, located in the ``/wazuh-docker/multi-node/config/wazuh_cluster/wazuh_manager.conf``

Append the configuration from the file: [Slack config file](https://github.com/tanjii/Wazuh/blob/main/slack_ossec.conf)


Save configuration, recreate docker manager:

```docker restart multi-node-wazuh.master-1```



For <level> parameter refer to Refer to https://documentation.wazuh.com/current/user-manual/ruleset/rules/rules-classification.html 
