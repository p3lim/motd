# motd

A collection of scripts that improve the Message Of The Day for SSH connections on a server.  
Designed with a home-server in mind, it will display useful information upon remote connections.

Compared to the PAM MOTD module for SSHD, this project will update the MOTD in the background every minute instead of on-demand on every login (which **will** delay SSH connections severely).

![](example.png?raw=true)

### Installation

1. Move the files to a new directory `/etc/custom-motd.d` (not to be confused with `/etc/update-motd.d`), and give them the executable bit (`chmod +x`).
2. Add the following line to the `root` user's crontab (`sudo crontab -e`):
```bash
* * * * * run-parts /etc/custom-motd.d > /etc/motd.temp && mv /etc/motd.temp /etc/motd
```

3. Disable dynamic MOTD from PAM's SSHD config:
```bash
sed -e '/pam_motd.so\s*motd=/ s/^#*/#/' -i /etc/pam.d/sshd
sed -e '/pam_motd.so\s*noupdate/ s/^#*//' -i /etc/pam.d/sshd
```

And away we go!

### Dependencies

- `10-logo`
	- [figlet](http://www.figlet.org/) (optional)
- `12-services`
	- [docker](https://docs.docker.com/install/) (optional, will exit gracefully without)
- `13-drives`
	- [snapraid](https://www.snapraid.it/) (optional, for S.M.A.R.T status)
- `14-updates`
	- Debian-based system (for aptitude)
