#### Documentation

##### Install and Configure a Bridge in OpenSUSE
https://community.torproject.org/relay/setup/bridge/opensuse/

##### Install and Configure a Middle Relay in Fedora
https://community.torproject.org/relay/setup/guard/fedora/

#### 1. Enable Automatic Updates

1. `su` # Remember to be root to run the following commands
2. `sudo passwd root` # If asking for a password you can assign a new one
3. `zypper install  yast2-online-update-configuration` # Installation of the automatic update package
4. `yast2 online_update_configuration` # Will prompt a CLI to enable/change some settings
	1. `Automatic Online Update`
	2. `Interval: Daily`
	3. `Skip Interactive Patches`
	4. `Agree with Licenses`
	5. `Use delta rpms`

#### 2. Tor Installation and Configuration

1. `zypper install tor` # Installing tor
2. `zypper install nano` # Installing nano for text editing purposes :p
3. `nano /etc/tor/torrc` # Delete the content and copy the following configuration and replace **[nickname]**,[email] and [orport]
	`Nickname [nickname]` # Remember no more than 19 characters and just letters
	`ContactInfo [email]` # Remember to add a valid contact email (I recommend a email forwarding)
	`ORPort [orport]` # This is the port that you need to enable on the EC2 instance firewall
	`ExitRelay 0`
	`SocksPort 0`
	`ControlPort 9051`
4. `systemctl enable --now tor` # Enabling Tor now...
5. `systemctl restart tor`
6. `systemctl status tor`
7. `journalctl -e -u tor` # Check logs

If everything goes ok, you should see a similar output

![[Pasted image 20221130020037.png]]
