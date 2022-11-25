### AWS EC2 with FreeBSD and Nginx

#### Connect to your AWS EC2 instance

- You need to connect with the key file generated and the user "ec2-user"

#### Documentation

##### Install and Configure Tor on FreeBSD
	https://community.torproject.org/relay/setup/guard/freebsd/
##### Configure a Hidden Service
	https://community.torproject.org/onion-services/setup/

#### Server update and Repository configuration

1. `su` # Change to the "root" user
2. `pkg bootstrap` # Bootstrap the package manager
3. `pkg update -f` # Update the FreeBSD repository
4. `pkg install ca_root_ns` # Allow us to use HTTPS to fetch our packages and updates
5. `mkdir -p /usr/local/etc/pkg/repos` # Creating a new directory for the repository
6. `echo "FreeBSD: {url: pkg+https://pkg.freebsd.org/${ABI}/latest}" > /usr/local/etc/pkg/repos/FreeBSD.conf` # Add the proper repository to the new file
7. `pkg update -f` # Update the FreeBSD repository
8. `pkg upgrade -y -f`  # Upgrade FreeBSD

#### Installing and Configure Tor Service

1. `pkg install tor` # Installing Tor
2. `pkg install nano`  # Installing nano for easy text edition
3. `nano /usr/local/etc/tor/torrc` # Tor Relay Configuration file
4. *Uncomment*
	1. `HiddenServiceDir /var/db/tor/hidden_service/` # Creates the Hidden Service directory
	2. `HiddenServicePort 80 127.0.0.1:80` # Allows HTTP connections on port 80
5. `echo "net.inet.ip.random_id=1" >> /etc/sysctl.conf`
6. `sysctl net.inet.ip.random_id=1`
7. `sysrc tor_setuid=YES` 
8. `sysrc tor_enable=YES`
9. `service tor start` # Start the Tor Service
10. `service tor status` # Check if it's running correctly
11. `cat /var/db/tor/hidden_service/hostname` # Check the .onion address

#### Start and Configure the Nginx Web Server

1. `service nginx start` # Automatically opens a web service on port 80
2. `service nginx stauts` # Check if it's running correctly
3. `nano /usr/local/www/nginx/index.html` # Change and configure your "index" file
4. `nano /usr/local/etc/nginx/nginx.conf` # If you need to configure the nginx web server
