# Change hostnames

To change a hostname, you should first login into the node using SSH and then type

```text
sudo nano /etc/cloud/cloud.cfg
```

Then find `preserve_hostname: false` and change it to `preserve_hostname: true`

Save and exit.

![](../.gitbook/assets/image%20%285%29.png)

Then type the following command in your terminal window

```text
sudo nano /etc/hostname
```

Change the hostname to `worker01`, for example.

Save, exit, and reboot.

```text
sudo reboot
```

