# Personal Notes

# References

* https://communities.vmware.com/docs/DOC-8760
* http://www.virtuallyghetto.com/2015/05/ghettovcb-vib-offline-bundle-for-esxi.html

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
