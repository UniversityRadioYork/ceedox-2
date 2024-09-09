## SSH Access

[Secure Shell](http://en.wikipedia.org/wiki/Secure_Shell), or SSH, is a secure way to remotely get shell (command-line) access to a server. We use it a lot in URY, so it's important that you're able to access URY servers using it.

This might need some configuration, depending on whether or not you're trying to SSH into a server from a computer that's on the campus network, or one that's off it.

### On Campus

If in the campus network, you should be able to SSH into **ury.york.ac.uk**, **uryfs1.york.ac.uk**, etc directly.

### Off Campus

You can't SSH directly into URY from outside campus; what you need to do is tunnel through a campus machine you have access to. For computer science students, the Computer Science teaching server (**csteach1.york.ac.uk**) should work.

The most preferable way to access is using the York SSH gateway (**ssh.york.ac.uk**) to use this you can ask IT Services to enable access for you, then use it to jump to internal servers. This gateway supports the use of key authentication, which no other Uni provided service does.

For non computer science students then [VPN](.vpn_access) access into the university will always work.

#### Configuring the SSH Client

You can edit your **~/.ssh/config** to easily add tunnels by using proxy connections:

    Host *
    # use my local keys on remote machines
    ForwardAgent yes
    # keep the connection alive for finicky servers, and kill it when the
    # connection has died
    ServerAliveInterval 15
    ServerAliveCountMax 3
    # allow local and proxy commands
    PermitLocalCommand yes

    Host ssh.york.ac.uk
    Hostname ssh.york.ac.uk
    User [itservicesuser]

    Host csteach csteach1
    Hostname csteach1.york.ac.uk
    User [itservicesuser]

    Host csteach0
    Hostname csteach0.york.ac.uk
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [itservicesuser]

    Host ury
    Hostname ury.york.ac.uk
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host uryfw0
    Hostname uryfw0.york.ac.uk
    LocalForward 3000 studio1:5900
    LocalForward 3001 studio1guest:5900
    LocalForward 3002 studio2:5900
    LocalForward 3003 studio2guest:5900
    LocalForward 3004 wallpc1:5900
    LocalForward 3005 wallpc2:5900
    LocalForward 3006 production:5900
    LocalForward 4001 studio1:3389
    LocalForward 4002 studio2:3389
    LocalForward 4004 wallpc1:3389
    LocalForward 4005 wallpc2:3389
    LocalForward 4006 production:3389
    LocalForward 5000 eccleston:80
    LocalForward 5001 tennant:80
    LocalForward 5002 smith:80
    LocalForward 20000 10.64.160.18:80
    LocalForward 20010 10.64.160.18:4321
    LocalForward 20001 10.64.160.19:80
    LocalForward 20011 10.64.160.19:4321
    LocalForward 20002 10.64.160.20:80
    LocalForward 20012 10.64.160.20:4321
    LocalForward 20003 10.64.160.21:80
    LocalForward 20013 10.64.160.21:4321
    LocalForward 20004 10.64.160.22:80
    LocalForward 20014 10.64.160.22:4321
    LocalForward 30000 10.64.160.14:80
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host urybsod
    Hostname urybsod.york.ac.uk
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host uryrrod
    Hostname uryrrod.york.ac.uk
    LocalForward 9002 localhost:3000
    LocalForward 3498 localhost:3498
    LocalForward 3499 localhost:3499
    LocalForward 3501 localhost:3501
    LocalForward 3502 localhost:3502
    LocalForward 3503 localhost:3503
    LocalForward 3504 localhost:3504
    LocalForward 3505 localhost:3505
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host urysteve
    Hostname urysteve.ury.york.ac.uk
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host dolby
    Hostname dolby.ury.york.ac.uk
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host studio1guest
    Hostname studio1guest.york.ac.uk
    ProxyCommand ssh uryfw0 -W %h:%p
    User ury

    Host urybackup0
    Hostname urybackup0.york.ac.uk
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host urystv
    Hostname urystv.york.ac.uk
    ProxyCommand ssh ssh.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host eccleston
    Hostname eccleston.york.ac.uk
    ProxyCommand ssh uryfw0.york.ac.uk -W %h:%p
    User [uryserveruser]

    Host tennant
    Hostname tennant.york.ac.uk
    ProxyCommand ssh uryfw0.york.ac.uk -W %h:%p
    User [uryserveruser]

#### University VPN

Alternatively you could use the [university VPN](.vpn_access).

### Ex-Student

Prior to getting approval for direct external ssh access uryfw0 has been set up to create reverse tunnels using autossh. To use this you will need an external server to act as your jumpbox.

1.  On the jumpbox run ''useradd -m -s /bin/false autossh''
2.  On uryfw0 change user ''sudo su -s /bin/bash autossh''
3.  Generate yourself a key with ''ssh-keygen -f ~/.ssh/id_rsa-user'' ensure to change ''user'' to your username to not overwrite other people's keys.
4.  Copy the generated public key to your jumpbox in the file: ''/home/autossh/.ssh/authorized_keys''
5.  Add your jumpbox config to ''/home/autossh/.ssh/config'' using the below example.
6.  Add the connection line to ''/etc/networking/if-up.d/autossh'' to start up the tunnel upon network connection using the below example.

Example host config:
Host example
HostName example.co.uk
User autossh
IdentityFile /home/autossh/.ssh/id_rsa-user
RemoteForward 1350 localhost:22

Example connection line:
su -s /bin/sh autossh -c '/usr/bin/autossh -M 0 -f -NTq -F /home/autossh/.ssh/config example'

Change ''example'' to match the host given in ''.ssh/config''.

You can now use your jumpbox as the ProxyCommand in your ssh config. Simply change the ProxyCommand for uryfw0 to:
ProxyCommand ssh example -W localhost:1350
And update the other hosts to use uryfw0 instead of ssh.york.ac.uk
