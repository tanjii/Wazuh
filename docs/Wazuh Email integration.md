To enable email alerts on the Wazuh server, it is necessary to set posfix on the server first. and using Google App password to authenticate. 



## Creating Google app password

Create app password - https://myaccount.google.com/apppasswords


## Creating SMTP Server with authentication

Wazuh email alerts do not support SMTP servers with authentication, such as Gmail. However, you can send these emails via a server relay like Postfix.

The detail instruction is on the link - (https://documentation.wazuh.com/current/user-manual/manager/alert-management.html#smtp-server-with-authentication) 


## Configure Postfix with Gmail

Install required packages
```
apt-get update && apt-get install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules
```

Edit the ``/etc/postfix/main.cf`` and append the following: 
```
relayhost = [smtp.gmail.com]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
smtp_use_tls = yes
smtpd_relay_restrictions = permit_mynetworks, permit_sasl_authenticated, defer_unauth_destination
inet_interfaces = all
inet_protocols = ipv4

mynetworks = 127.0.0.0/8, 172.18.0.0/16
```


Please see below how to find out the mynetworks settings, in the Docker gateway IP section

Set the credentials
```
echo [smtp.gmail.com]:587 email@gmail.com:<AppPasswrd> > /etc/postfix/sasl_passwd
postmap /etc/postfix/sasl_passwd
```
Change ```<AppPassword>``` with app password from the email address.

Secure password DB file
```
chown root:root /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
chmod 0600 /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
```
Restart postfix service
```
systemctl restart postfix
```
Enable postfix service
```
systemctl enable postfix
```
Test the configuration
```
echo "Test mail from postfix" | mail -s "Test Postfix" -r "<CONFIGURED_EMAIL>" <RECEIVER_EMAIL>
```
```<CONFIGURED_EMAIL>``` replace with your configured email address.

```<RECEIVER_EMAIL>``` replace with the email address of the recipient.


## Configure email notifications in the ```ossec.conf ```

To change ossec.conf file, you must ssh into wazuh instance, edit the ```wazuh-manager.conf``` file, located in the ```/wazuh-docker/multi-node/config/wazuh_cluster/wazuh_manager.conf```

Append the following configuration from the file: [email_ossec.conf](https://github.com/tanjii/Wazuh/blob/main/email_ossec.conf)




``<email_notification>`` toggles the use of email alerting.

``<smtp_server>`` defines the SMTP server to use to deliver alerts. For this we will use Docker gateway IP. 

``<email_from>`` specifies the email address of the configured sender. Replace <USERNAME> with your configured username of your email address.

``<email_to>`` specifies the email address of the recipient of alerts. Replace <RECEIVER_EMAIL> with the email address of the recipient.



Save configuration and recreate the manager container
```
docker restart multi-node-wazuh.master-1
```



## How to find out the Docker gateway IP 

Enter the container
```
docker exec -it multi-node-wazuh.master-1 bash
```

Find out the gateway
```
cat /proc/net/route
```

Take the Gateway value and reverse the hex
 



