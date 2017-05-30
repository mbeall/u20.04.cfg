`sudo -v`

## Access

#### `sudo visudo`

Add `mbeall ALL=(ALL) NOPASSWD: ALL` to bottom

#### Set hostname

`sudo hostnamectl set-hostname HOSTNAME`

#### Edit `/etc/hosts`

`sudo vi /etc/hosts`

    203.0.113.#### hostname.example.com hostname
    2600:3c01::a123:b456:c789:d0#### hostname.example.com hostname

#### Add Public Key Authentication
`ssh-keygen -t rsa -b 4096`

#### Add SSH key to https://github.com/settings/keys

#### Add SSH key from local machine
`vi .ssh/authorized_keys`

#### Recreate server keys

    sudo rm /etc/ssh/*host*
    sudo dpkg-reconfigure openssh-server

#### Edit `/etc/ssh/sshd_config`

`sudo vi /etc/ssh/sshd_config`

    PermitRootLogin prohibit-password
    PasswordAuthentication no

#### Reload ssh server

`sudo systemctl reload sshd`

#### Test SSH login from local machine

#### Install `fail2ban`

    sudo apt-get install fail2ban
    sudo ufw allow ssh
    sudo ufw enable
    sudo ufw logging on

## User

#### Reconfigure server time

`sudo dpkg-reconfigure tzdata`

#### Configure default home folders

    sudo mkdir /etc/skel/{private,public}
    sudo chmod 700 /etc/skel/private

#### Add default home folders for current user

    mkdir ~/{private,public}
    sudo chmod 700 ~/private

#### Configure git

    git config --global user.email "mbeall@starverte.com"
    git config --global user.name "Matt Beall"
    git config --global push.default simple

#### Configure hidden files

    cd ~/private
    git clone git@github.com:mbeall/dotfiles.git
    sudo cp -p dotfiles/.bin/ubuntu/* /usr/local/bin/
    cd ~

#### Configure login message

    sudo chmod -x /etc/update-motd.d/10-help-text
    sudo chmod -x /etc/update-motd.d/90-updates-available

    sudo apt-get install figlet
    sudo cp -p ~/private/dotfiles/update-motd.d/99-footer /etc/update-motd.d/

## Updates

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

#### Edit `50unattended-upgrade`

`sudo vi /etc/apt/apt.conf.d/50unattended-upgrades`

- Uncomment `"${distro_id}:${distro_codename}-updates";`
- Uncomment and edit `Unattended-Upgrade::Mail "admin@starverte.com";`
- Uncomment `Unattended-Upgrade::MailOnlyOnError "true";`

#### Install other `apt` software

`sudo apt-get install apticron apt-listchanges`

`sudo vi /etc/apticron/apticron.conf`

- `EMAIL="admin@starverte.com"`
- `CUSTOM_SUBJECT='[apticron] $SYSTEM: Update(s) available'`

## Email

#### Install outgoing email server

`sudo apt-get install libsasl2-modules postfix`

- General type of mail configuration: [Internet Site]
- System mail name: [HOSTNAME.fortcollinscreative.com]

#### Configure outgoing email server

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

#### Edit `sasl_passwd`

`sudo vi /etc/postfix/sasl/sasl_passwd`

    [smtp.gmail.com]:587 wp@fortcollinscreative.com:PASSWORD

`sudo postmap /etc/postfix/sasl/sasl_passwd`

#### Edit `virtual`

`sudo vi /etc/postfix/virtual`

    @HOSTNAME.fortcollinscreative.com admin@starverte.com

`sudo postmap /etc/postfix/virtual`

#### Lockdown configuration files

    sudo chown root:root /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db
    sudo chmod 0600 /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db

#### Restart postfix

    sudo systemctl restart postfix

## Install LEMP

#### Install Nginx

    sudo apt-get install nginx
    
#### Edit `nginx.conf`

`sudo vi /etc/nginx/nginx.conf`

    client_max_body_size 32M;
    
`sudo systemctl restart nginx`

#### Install MySQL

    sudo apt-get install mysql-server

    sudo mysql_secure_installation

#### Install PHP

    sudo apt-get install php-fpm php-mysql
    sudo apt-get install php-gd

#### Edit `php.ini`

`sudo vi /etc/php/7.0/fpm/php.ini`

    cgi.fix_pathinfo=0
    post_max_size = 32M
    upload_max_filesize = 32M
    
    ; Uncode theme recommendations
    max_execution_time = 120
    max_input_vars = 3000

`sudo systemctl restart php7.0-fpm`

## Misc

#### Install utilities

    sudo apt install secure-delete
    sudo apt install mailutils
    sudo apt install apticron

#### Crontab

`crontab -e`

    00 01  *   *   *  wpg-update-trusted &> /dev/null
    30 01  *   *   1  fccg-sizes &> /dev/null
    00 02  *   *   2  wpg-available-updates &> /dev/null
    30 03  *   *   3  wpg-cleanup &> /dev/null

`sudo crontab -e`

    00  03  01   *    *    fccg-backup

