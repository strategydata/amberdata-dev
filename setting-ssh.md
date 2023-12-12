# How to setup Setting SSH with a jump host

if you need to VS code SSH extension to connect to a remote host through another intermediate jump host then

you need to configure ssh client rather than VS code and tell it about jump host.

## SSH Generate

you can create ssh key in jump host, you can run following command to generate ssh key in jump host and copy it to prod host

Linux

```
ssh-keygen

ssh-copy-id -i ~/.ssh/id_rsa.pub UserName@[your private IP]

```

you can create ssh key in local machine by following command to generate ssh key in local machine and copy it to jump host

Windows

```
ssh-keygen

type C:\Users\UserName\.ssh\id_rsa.pub | ssh [your public IP] "cat >> .ssh/authorized_keys"

```



## .ssh/config

you need to edit ssh config file in your local machine by following content

run following command 
```
sudo vi ~/.ssh/config

```
you will go into vim editor, you can just copy and paste following content to your config file, but don't forget to change username , id_rsa file name and IP Address to match yours.



```

Host jump.host
	HostName [your public IP]
	User UserName
	IdentityFile ~/.ssh/id_rsa
	
Host prod.host
	HostName [your private IP]
	User UserName
	ProxyCommand ssh -q -W %h:%p jump.host

```
but if you are using Windows system, path will be different


```
Host jump.host
	HostName [your public IP]
	User UserName
	IdentityFile C:\Users\UserName/.ssh/id_rsa
	
Host prod.host
	HostName [your private IP]
	User UserName
	ProxyCommand ssh -q -W %h:%p jump.host
```

