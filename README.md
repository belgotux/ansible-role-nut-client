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
- `notifynut_method` can be [mail|pushbullet|telegram|pushover|sms] (default `mail`)
- `notifynut_mailto` mail adresse or alias to send notification (default root)
- `nut_mode` can be [netclient|netserver] netserver is for the server that is attached to UPS by USB for IP card (default netclient)
- `nut_type` can be [slave|master]. Master when the server is attached to UPS by USB. Slave when the server need to connect to another server (default master)
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
- `notifynut_pushover_appToken` Pushover Application token
- `notifynut_pushover_userkey` Pushover User key
- `notifynut_pushover_providerApi` Provider API (default `"https://api.pushover.net/1/messages.json"`)
- `notifynut_pushover_subject` Short subject for pushover (default `"UPS event $argument"`)

### Doing different notification method in case of event
- `notifynut_method_online` one method or a list of method in this format `(telegram mail)` (default same value of `$methodDefault`)
- `notifynut_method_onbatt` idem
- `notifynut_method_lowbatt` idem
- `notifynut_method_fsd` idem
- `notifynut_method_shutdown` idem
- `notifynut_method_comm` idem
- `notifynut_method_serveronline` idem


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
        nut_type: slave
        notifynut_method: pushbullet
        notifynut_pushbullet_accessToken: o.xxxxx
        #if you don't want notification from clients i.e.
        notifynut_method: none
        notifynut_method_comm: telegram
        notifynut_method_serveronline: mail
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
        # all notifications for the server and special for online i.e.
        notifynut_method: telegram
        notifynut_method_online: (telegram mail)
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