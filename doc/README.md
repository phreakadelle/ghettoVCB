# Personal Notes

# References

* https://communities.vmware.com/docs/DOC-8760
* http://www.virtuallyghetto.com/2015/05/ghettovcb-vib-offline-bundle-for-esxi.html
* http://www.hoelzle.net/ghetto-vcb-unter-esxi-6-mit-mailinfo-und-vereinfachter-verwaltung-der-vms/#comment-28
* http://www.timharrison.me.uk/guides/vmware-esxi-5.5/file-is-read-only-when-editing-crontab-in-vmware
* http://stackoverflow.com/questions/16928004/how-to-enter-ssh-password-using-bash

# Change Package Level
```
esxcli software acceptance set --level=CommunitySupported
```

# Firewall
Step 1 - Create a file called /etc/vmware/firewall/email.xml with contains the following:
```
<ConfigRoot>
  <service>
    <id>email</id>
    <rule id="0000">
      <direction>outbound</direction>
      <protocol>tcp</protocol>
      <porttype>dst</porttype>
      <port>25</port>
    </rule>
    <enabled>true</enabled>
    <required>false</required>
  </service>
</ConfigRoot>
 ```
Step 2 - Reload the ESXi firewall by running the following ESXCLI command:
```
~ # esxcli network firewall refresh
```

Step 3 - Confirm that your email rule has been loaded by running the following ESXCLI command:
```
~ # esxcli network firewall ruleset list | grep email
email                  true
```

```
/opt/ghettovcb/bin/ghettoVCB.sh -m srv07 -g /vmfs/volumes/SSD1/auto-backup/ghettoVCB-srv07.conf -d info
```


# Cronjob
## Which file to edit
```
vi /var/spool/cron/crontabs/root
``` 

## Content
```
0 4 * * 1-5 /opt/ghettovcb/bin/ghettoVCB.sh -m srv07 -g /vmfs/volumes/SSD1/auto-backup/ghettoVCB-srv07.conf -d info >  /vmfs/volumes/nas/backup-$(date +\%s).log
```

# Cron Restart
```
kill $(cat /var/run/crond.pid)
crond
```

# Write Crontab Entry to rc.local
File: /etc/rc.local.d/local.sh
```
/bin/kill $(cat /var/run/crond.pid)
/bin/echo "0 4 * * 1-5 /opt/ghettovcb/bin/ghettoVCB.sh -m srv07 -g /vmfs/volumes/SSD1/auto-backup/ghettoVCB-srv07.conf -d info >  /vmfs/volumes/nas/backup-\$(date +\\%s).log" >> /var/spool/cron/crontabs/root
crond
```
