nut-client
==========

Role to install nut client to monitor a remote nut-server for local-server
Also install this script as dependance : 
- [nutNotify](https://github.com/belgotux/nutNotify)


Role dependancies
-----------------
No need dependancies anymore 

Role Variables
--------------
The role can work as it with the [default configuration](defaults/main.yml).

## Needed
- `notifynut_method` can be [mail|pushbullet|telegram] (default mail)
- `notifynut_mailto` mail adresse or alias to send notification (default root)
- `nut_mode` can be [netclient|netserver] netserver is for the server that is attach to UPS by USB for IP card (default netclient)
- `nut_ups_name` your ups name
- `nut_ups_ip` your ups server ip
- `nut_ups_port` nut port  (default 3493)
- `nut_user` nut user to monitor the ups
- `nut_password` nut password to monitor the ups

## Notification
### Common
- `notifynut_logfile` (default `/var/log/nutNotify/nutNotify.log`)
- `notifynut_flagfile` (default `/var/log/nutNotify/nutShutdown.flag`)
- `notifynut_curlBin` (default `/usr/bin/curl`)
- `notifynut_mailBin` (default `/usr/bin/mail`)
- `notifynut_subjectDefault` Suject use by all methods if not override (default `"$HOSTNAME UPS event $argument on $ups@$server !"`)
- `notifynut_bodyDefault` Message use by all methods if not override (default `"UPS event $argument on $ups at $(date +'%d-%m-%y %H:%M:%S')"`)
### Need one method at least
- `notifynut_mailfrom` Mail sender
- `notifynut_mailto` Mail recipient
- `notifynut_sms_provider` Betamax/Dellmont provider with sms API like jumblo.com
- `notifynut_sms_username` Username
- `notifynut_sms_password` Password
- `notifynut_sms_number` GSM number international format +32xxxxxxxx
- `notifynut_pushbullet_accessToken` Pushbullet token like o.xxxxxxxxxxxxxxxxxxxxxxxxxxxx
- `notifynut_pushbullet_providerApi` Provider API (default `"https://api.pushbullet.com/v2/pushes"`)
- `notifynut_pushbullet_subject` Short subject for pushbullet (default `"UPS event $argument"`)
- `notifynut_telegram_accessToken` Telegram token
- `notifynut_telegram_chatID` Telegram chat ID
- `notifynut_telegram_providerApi` Telegram API to change if you use your own server (default `"https://api.telegram.org"`)
- `notifynut_telegram_subject` Short subject for pushbullet (default `"UPS event $argument"`)


Example Playbook
----------------
### For clients

```yml
- hosts: [homeservers]
  remote_user: root
  roles:
    - role: basic
    - role: nut-client
      vars:
        nut_mode: netclient
        notifynut_method: pushbullet
        notifynut_pushbullet_accessToken: o.xxxxx
```

### For the ups server :
```yml
- hosts: [upsserver]
  remote_user: root
  roles:
    - role: basic
    - role: nut-client
      vars:
        nut_mode: netserver
        notifynut_method: telegram
        notifynut_mailfrom: "NUT $HOSTNAME <home@yourserver.tld>"
        notifynut_mailto: "your@mail.tld"
        notifynut_telegram_accessToken: xxx
        notifynut_telegram_chatID: xxx
```


License
-------

[GPL v3](https://www.gnu.org/licenses/gpl-3.0.en.html)

Author Information
------------------

Belgotux
[MonLinux](https://www.monlinux.net)

## Link
See [this article about nut UPS notifications](https://www.monlinux.net/2018/03/nut-ups-notifications-mails-et-arret/) for more informations (in french)