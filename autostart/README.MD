A reliable way to auto start and stop virtual machines based on host startup / shutdown

Create a service file and add the content below, make sure to replace vmname with
the name of your virtual machine, and update myusername to your username.

```
sudo nano /etc/systemd/system/vmname.service

[Unit]
Description=vmname
After=network.target virtualbox.service
Before=runlevel2.target shutdown.target
 
[Service]
User=myusername
Group=vboxusers
Type=forking
Restart=no
TimeoutSec=5min
IgnoreSIGPIPE=no
KillMode=process
GuessMainPID=no
RemainAfterExit=yes
 
ExecStart=/usr/bin/VBoxManage startvm vmname --type headless
ExecStop=/usr/bin/VBoxManage controlvm vmname acpipowerbutton
 
[Install]
WantedBy=multi-user.target

```

After creating the file, reload services, and configure
the new service to run at startup
```
sudo systemctl daemon-reload
sudo systemctl enable vmname.service
```
start and stop the service to test it out.
```
#list running vm's
vboxmanage list runningvms

#start service
sudo systemctl start vmname.service

#list running vm's
vboxmanage list runningvms

#stop service
sudo systemctl stop vmname.service

#list running vm's
vboxmanage list runningvms
```


reference: https://linux.m2osw.com/autostart-virtualbox-vms
