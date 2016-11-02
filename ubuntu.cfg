# $ sudo -v

# $ sudo visudo
## Add 'mbeall ALL=(ALL) NOPASSWD: ALL' to bottom

# $ sudo hostnamectl set-hostname ultar
# $ sudo vi /etc/hosts
# >>> 203.0.113.10 hostname.example.com hostname
# >>> 2600:3c01::a123:b456:c789:d012 hostname.example.com hostname

## Add Public Key Authentication
# $ ssh-keygen -t rsa -b 4096

## Add SSH key to https://github.com/settings/keys

## Add SSH key from local machine
# $ vi .ssh/authorized_keys

# $ sudo rm /etc/ssh/*host*
# $ sudo dpkg-reconfigure openssh-server

# $ sudo vi /etc/ssh/sshd_config
# >>> PermitRootLogin prohibit-password
# >>> PasswordAuthentication no

# $ sudo systemctl reload sshd

## Test SSH login from local machine

# $ sudo dpkg-reconfigure tzdata

# $ sudo apt-get install fail2ban
# $ sudo ufw allow ssh
# $ sudo ufw enable
# $ sudo ufw logging on

# $ sudo mkdir /etc/skel/{private,public}
# $ sudo chmod 700 /etc/skel/private

# $ mkdir ~/{private,public}
# $ sudo chmod 700 ~/private

# $ git config --global user.email "mbeall@starverte.com"
# $ git config --global user.name "Matt Beall"
# $ git config --global push.default simple

# $ cd ~/private
# $ git clone git@github.com:mbeall/dotfiles.git
# $ sudo cp -p dotfiles/.bin/ubuntu/* /usr/local/bin/
# $ cd ~

# $ sudo apt-get update
# $ sudo apt-get upgrade -y
# $ sudo reboot

# $ sudo apt-flush
# $ sudo apt-get update
# $ sudo apt-get upgrade -y
# $ sudo apt-get dist-upgrade -y
# $ sudo reboot

# $ sudo apt-flush
# $ sudo apt-get update
# $ sudo apt-get upgrade -y
# $ why-reboot

# $ sudo chmod -x /etc/update-motd.d/10-help-text
# $ sudo chmod -x /etc/update-motd.d/90-updates-available

# $ sudo apt-get install figlet
# $ sudo cp -p ~/private/dotfiles/update-motd.d/99-footer /etc/update-motd.d/

# $ sudo vi /etc/apt/apt.conf.d/50unattended-upgrades
## Uncomment `"${distro_id}:${distro_codename}-updates";`
## Add to blacklist:
### `"*apache*";` OR `"*nginx*";`
### `"*mysql*";` OR `"*mariadb*";`
### `"*php*";` OR `"*hhvm*";`
## Uncomment and edit `Unattended-Upgrade::Mail "admin@starverte.com";`
## Uncomment `Unattended-Upgrade::MailOnlyOnError "true";`

