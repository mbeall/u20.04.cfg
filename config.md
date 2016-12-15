
`sudo -v`

1. `sudo visudo`
Add `mbeall ALL=(ALL) NOPASSWD: ALL` to bottom

1. Set hostname

`sudo hostnamectl set-hostname HOSTNAME`

1. Edit `/etc/hosts`

`sudo vi /etc/hosts`

    203.0.113.10 hostname.example.com hostname
    2600:3c01::a123:b456:c789:d012 hostname.example.com hostname

1. Add Public Key Authentication
`ssh-keygen -t rsa -b 4096`

1. Add SSH key to https://github.com/settings/keys

1. Add SSH key from local machine
`vi .ssh/authorized_keys`

1. Recreate server keys

    sudo rm /etc/ssh/*host*
    sudo dpkg-reconfigure openssh-server

1. Edit `/etc/ssh/sshd_config`

`sudo vi /etc/ssh/sshd_config`

    PermitRootLogin prohibit-password
    PasswordAuthentication no

1. Reload ssh server

`sudo systemctl reload sshd`

1. Test SSH login from local machine

1. Reconfigure server time

`sudo dpkg-reconfigure tzdata`

1. Install `fail2ban`

    sudo apt-get install fail2ban
    sudo ufw allow ssh
    sudo ufw enable
    sudo ufw logging on

1. Configure default home folders

    sudo mkdir /etc/skel/{private,public}
    sudo chmod 700 /etc/skel/private

1. Add default home folders for current user

    mkdir ~/{private,public}
    sudo chmod 700 ~/private

1. Configure git

    git config --global user.email "mbeall@starverte.com"
    git config --global user.name "Matt Beall"
    git config --global push.default simple

1. Configure hidden files

    cd ~/private
    git clone git@github.com:mbeall/dotfiles.git
    sudo cp -p dotfiles/.bin/ubuntu/* /usr/local/bin/
    cd ~

1. Update debian software

    sudo apt-get update
    sudo apt-get upgrade -y
    sudo reboot

    sudo apt-flush
    sudo apt-get update
    sudo apt-get upgrade -y
    sudo apt-get dist-upgrade -y
    sudo reboot

    sudo apt-flush
    sudo apt-get update
    sudo apt-get upgrade -y
    why-reboot

1. Configure login message

    sudo chmod -x /etc/update-motd.d/10-help-text
    sudo chmod -x /etc/update-motd.d/90-updates-available

    sudo apt-get install figlet
    sudo cp -p ~/private/dotfiles/update-motd.d/99-footer /etc/update-motd.d/

1. Edit `50unattended-upgrade`

`sudo vi /etc/apt/apt.conf.d/50unattended-upgrades`

- Uncomment `"${distro_id}:${distro_codename}-updates";`
- Uncomment and edit `Unattended-Upgrade::Mail "admin@starverte.com";`
- Uncomment `Unattended-Upgrade::MailOnlyOnError "true";`

1. Install outgoing email server

`sudo apt-get install libsasl2-modules postfix`

- General type of mail configuration: [Internet Site]
- System mail name: [HOSTNAME.fortcollinscreative.com]

1. Configure outgoing email server
`sudo vi /etc/postfix/main.cf`

    myhostname = HOSTNAME.fortcollinscreative.com

    ...

    relayhost = [smtp.gmail.com]:587

    ...

    # Enable SASL authentication
    smtp_sasl_auth_enable = yes
    # Disallow methods that allow anonymous authentication
    smtp_sasl_security_options = noanonymous
    # Location of sasl_passwd
    smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd
    # Enable STARTTLS encryption
    smtp_tls_security_level = encrypt
    # Location of CA certificates
    smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt

1. Edit `sasl_passwd`

`sudo vi /etc/postfix/sasl/sasl_passwd`

    [smtp.gmail.com]:587 wp@fortcollinscreative.com:PASSWORD

`sudo postmap /etc/postfix/sasl/sasl_passwd`

1. Edit `virtual`

`sudo vi /etc/postfix/virtual`

    @HOSTNAME.fortcollinscreative.com admin@starverte.com

`sudo postmap /etc/postfix/virtual`

1. Lockdown configuration files

    sudo chown root:root /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db
    sudo chmod 0600 /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db

1. Restart postfix

    sudo systemctl restart postfix

