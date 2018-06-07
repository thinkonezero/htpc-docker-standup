# HTPC Docker Standup
This is a simple docker-compose configuration to standup a new HTPC. It's based on running on an Ubuntu server, but could easily be adapted for other opertaing systems with Docker support.

It includes the following Services

- [Plex Media Server](https://www.plex.tv/) - for managing media and serving files to Plex Clients
- [Deluge](https://deluge-torrent.org/) + [Private Internet Access VPN](https://www.privateinternetaccess.com/pages/buy-vpn/toz) - for downloading torrents... "safely"
- [Sonarr](https://sonarr.tv/) - for TV Series Management
- [Radarr](https://radarr.video/) - for Movie Management
- [Jackett](https://github.com/Jackett/Jackett) - for Torrent Tracker feeds
- [Tautulli](http://tautulli.com/) - for Plex library statistics and usage
- [Ombi](https://ombi.io/) - for requesting additional library content
- [Portainer](https://portainer.io/) - for managing all of your Docker containers
- [Watchtower](https://github.com/v2tec/watchtower) - for automatically updating running containers
- [NetData](https://my-netdata.io/) - for system resource monitoring
- [Muximux](https://github.com/mescon/Muximux) - for simple web based management
- [Duplicati](https://www.duplicati.com/) - for backing up data to your favorite provider
- [InfluxDB](https://www.influxdata.com/) - for time series based database storage
- [Chronograf](https://www.influxdata.com/time-series-platform/chronograf/) - for making pretty dashboards out of the database data
- [SpeedTest](https://github.com/sivel/speedtest-cli/) - for performing a speedtest and posting data to the database
- [Nginx Proxy](https://github.com/jwilder/nginx-proxy) + [Let's Encrypt](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) - for easily accessing services on SSL Enabled Hostnames

This project was heavily inspired by the [MediaBox](https://github.com/tom472/mediabox) project... Many Thanks!

## Known Issues
- [ ] Sometimes the Deluge + VPN Container disconnects and can't re-establish a forwarded port connection. 
- [ ] Plex can't be assigned Hostname + SSL on `Host` Network - [Nginx Reverse Proxy](https://github.com/jwilder/nginx-proxy#multiple-networks)


## Install Instructions

### Prerequisites
- [Ubuntu 16.04 LTS](https://www.ubuntu.com/)
- [VPN Account from PIA](https://www.privateinternetaccess.com/pages/buy-vpn/toz)
- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)
- [Python 2.7](https://www.python.org/)
- [Docker-Compose](https://docs.docker.com/compose/)

### Server Configuration
1. [Install Ubuntu Server](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-server) and get it updated - `sudo apt update` then `sudo apt upgrade`
2. If you have existing media on a separate NTFS formatted drive:
 - Mount NTFS Drive w/ Appropriate Permissions, [Link 1](https://askubuntu.com/questions/113733/how-do-i-correctly-mount-a-ntfs-partition-in-etc-fstab#comment684545_113746)
 - `sudo blkid` - Find the block ID of your media drive
 - `sudo mkdir /media/Media` - make the mount point
 - Add to `/etc/fstab`: `UUID=BC6617A366175D88 /media/Media ntfs-3g uid=1000,gid=1000,dmask=022,fmask=133 0 0`
3. [Setup Samba Share](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated%2C%20Simple%20and%20Brief%20Way%21) (to access media outside of HTPC)
4. [Install Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository)
 - You'll need to add your user running docker to the `docker` group. See [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/)
5. [Install Docker Compose](https://docs.docker.com/compose/install/#install-compose)
6. Clone Repo `git clone https://github.com/phikai/htpc-docker-standup.git`
7. Copy Sample Environement File `cp sample.env .env`
8. Edit `.env` to match your environment
9. Run Docker Environment `docker-compose up -d`
10. ???
11. Profit.

### Environment File
- `LOCALUSER=` - This is the local user of your linux account and account running docker
- `HOSTNAME=` - Hostame of the server, can be found by executing `hostname` from command line
- `IP_ADDRESS=` - Local IP Address of the server, should be static
- `PUID=` - UID of the local user, can be found by executing `id` from the command line
- `PGID=` - GID of the local user, can be found by executing `id` from the command line
- `PMSTOKEN=` - The claim token for the server to obtain a real server token. If not provided, server is will not be automatically logged in. If server is already logged in, this parameter is ignored. You can obtain a claim token to login your server to your plex account by visiting [https://www.plex.tv/claim](https://www.plex.tv/claim)
- `VPNUNAME=` - Your Private Internet Access username from [PIA VPN](https://www.privateinternetaccess.com/pages/buy-vpn/toz)
- `VPNPASS=` - Your Private Internet Access password from [PIA VPN](https://www.privateinternetaccess.com/pages/buy-vpn/toz)
- `VPNPROVIDER=` - Your VPN provider, name must match a folder specified in `ovpn`. This defaults to PIA if you copied `sample.env`.
- `VPN_REMOTE=` - The remote server you want to connect to (must support port forwarding), [List of Servers](https://helpdesk.privateinternetaccess.com/hc/en-us/articles/219460187-How-do-I-enable-port-forwarding-on-my-VPN-)
- `CIDR_ADDRESS=` - IP/netmask entries which allow access to the server without requiring authorization. We recommend you set this only if you do not sign in your server. For example `192.168.1.0/24,172.16.0.0/16` will allow access to the entire `192.168.1.x` range and the `172.16.x.x`
- `TZ=` - Set the timezone inside the container. For example: `Europe/London`. The complete list can be found here: [https://en.wikipedia.org/wiki/List_of_tz_database_time_zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
- `PMSTAG=` - Which version of Plex Media Server, available options: `latest`, `plexpass` and `public`. You should use `public` unless you are a Plex Pass account
- `EMAIL=` - Email address to be used for Let's Encrypt SSL certificate validation - `someone@somewhere.com`
- `DOMAIN=` - Public domain to use for accessing services via a public domain - `server.domain.com`
- `WATCHTOWER_EMAIL=` - Email address you'd like Watchtower to notify for any notifications - `someone@somewhere.com`
- `SMTP_FROM=` - From address that your SMTP server uses to send email - `someone@somewhere.com`
- `SMTP_SERVER=` - Servername of your SMTP server - `smtp.domain.com`
- `SMTP_PORT=` - Port that your SMTP server uses to connect - `587`
- `SMTP_USER=` - Username that your SMTP server uses to authenticate
- `SMTP_PASS=` - Password for your SMTP user to authenticate
- `SPEEDTEST_INTERVAL=` - Number of seconds between tests to the [Speedtest.net](http://www.speedtest.net/) services

### Email/SMTP Service
- [Mailgun](https://documentation.mailgun.com/en/latest/quickstart.html) has an excellent QuickStart Guide
- Check out the sending via [SMTP](https://documentation.mailgun.com/en/latest/quickstart-sending.html#send-via-api)
- Make sure to also [verify your domain](https://documentation.mailgun.com/en/latest/quickstart-sending.html#verify-your-domain)

---

## Tips and Tricks

- [Deluge + PIA FAQ](https://lime-technology.com/forums/topic/44108-support-binhex-general/?tab=comments#comment-433613)
- [Removing Old/Completed Torrents from Deluge](https://www.cuttingcords.com/home/2015/2/4/auto-deleting-finished-torrents-from-deluge)
- [Create Series Folder in Sonarr](https://forums.sonarr.tv/t/adding-new-series-path-issues/2751/2)
- [Proper Encoding of the Portainer Password](https://github.com/portainer/portainer/issues/1506)
- Open Shell in a Container - [Link 1](http://phase2.github.io/devtools/common-tasks/ssh-into-a-container/), [Link 2](https://stackoverflow.com/a/30173220)

[Potential Script to setup, renew and copy SSL for Plex](https://www.npcglib.org/~stathis/blog/2017/05/13/plex-media-server-over-https-with-letsencrypt-certificates/)

---

If this project has helped you in anyway, and you'd like to say thanks...

[![Donate](https://img.shields.io/badge/Donate-SquareCash-brightgreen.svg)](https://cash.me/$phikai)
[![Donate with Bitcoin](https://en.cryptobadges.io/badge/micro/15JCkpHhjjVmWYaTBc2fJn4tcKHEd194gY)](https://en.cryptobadges.io/donate/15JCkpHhjjVmWYaTBc2fJn4tcKHEd194gY)

---

# Disclaimer

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.