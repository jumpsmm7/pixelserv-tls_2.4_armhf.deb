# pixelserv-tls_2.3.1-1_armhf.deb

## Install on Raspberry Pi

Binary packages are available from this [Github](https://github.com/jumpsmm7/). Should work on all Raspberry Pi's running Raspbian (Debian 10). For installation issues, you may refer to this [tracker](https://github.com/kvic-z/pixelserv-tls/issues/32).

#### Pre-built binary
Commands used to install pixelserv-tls on Raspbian (Debian 10).
````
sudo -i
cd /tmp
curl -O https://github.com/jumpsmm7/pixelserv-tls_2.3.1-1_armhf.deb/raw/master/pixelserv-tls_2.3.1-1_armhf.deb
dpkg -i pixelserv-tls_2.3.1-1_armhf.deb
````

Enable Pixelserv-tls as a Service
Required for Reboots
````
systemctl enable pixelserv-tls
````

Check Status of Pixelserv-tls
````
systemctl status pixelserv-tls
````

#### Configuration
For adjusting DAEMON_ARGS in '/etc/default/pixelserv-tls':

Default:
````
# Configuration file for pixelserv-tls

# Options to pass to pixelserv-tls:
DAEMON_ARGS="-z /var/cache/pixelserv"
````

Modified Example:
````
# Configuration file for pixelserv-tls

# Options to pass to pixelserv-tls:
DAEMON_ARGS="192.168.1.10 -z /var/cache/pixelserv"
````
More options can be found at [Daemon_Args Options Guide](https://github.com/kvic-z/pixelserv-tls/wiki/Command-Line-Options).

Generating CA 

Generating CA certificate (Advanced CA):
(Optional Method)
````
cd /var/cache/pixelserv
openssl genrsa -out ca.key 2048
openssl req -key ca.key -new -x509 -days 3650 -sha256 -extensions v3_ca -out ca.crt -subj "/CN=Pixelserv CA"
````

Generating CA certificate (Recommended CA):
(Recommended Method)<------ For less strain on system.
````
cd /var/cache/pixelserv
openssl genrsa -out ca.key 1024
openssl req -key ca.key -new -x509 -days 3650 -sha256 -extensions v3_ca -out ca.crt -subj "/CN=Pixelserv CA"
````

New Arguments to take Effect
````
service pixelserv-tls restart
````

Optional Uninstall
````
sudo -i
systemctl disable pixelserv-tls
service pixelserv-tls stop
dpkg --purge pixelserv-tls
rm -rf /var/cache/pixelserv
````
