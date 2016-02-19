# Johns BrewPi Information
## BrewPi legacy software information
## Information from Docs.BrewPi.com  ## http://www.instructables.com/id/Multiple-Fermentation-Chamber-Control-with-BrewPi/?ALLSTEPS


sudo apt-get update
sudo apt-get upgrade

sudo apt-get install apache2
sudo apt-get install libapache2-mod-php5 php5-cli php5-common php5-cgi

sudo apt-get install php5


************SKIP*****************************

sudo nano /var/www/html/chamber1/phpinfo.php
sudo nano /var/www/html/chamber2/phpinfo.php

<?php
phpinfo();
?>

sudo chown -R www-data:www-data /var/www/html/chamber1
sudo chown -R www-data:www-data /var/www/html/chamber2

sudo rm /var/www/html/chamber1/phpinfo.php
sudo rm /var/www/html/chamber2/phpinfo.php

***********************************************

ls /dev/ttyUSB*
udevadm info -a -n /dev/ttyUSB0 | less
udevadm info -a -n /dev/ttyUSB1 | less


cd /etc/udev/rules.d/
sudo nano 99-arduino.rules


SUBSYSTEM=="tty", KERNEL=="ttyUSB0", KERNELS=="1-1.3:1.0", SYMLINK+="chamber1"
SUBSYSTEM=="tty", KERNEL=="ttyUSB1", KERNELS=="1-1.2:1.0", SYMLINK+="chamber2"

sudo /etc/init.d/udev reload


sudo apt-get install python-serial python-simplejson python-configobj python-psutil python-git
sudo apt-get install build-essential python-dev python-pip
sudo pip install psutil --upgrade
sudo pip install pyserial --upgrade
sudo apt-get install arduino-core

sudo useradd -m -k /dev/null -G www-data,dialout brewpi
sudo passwd brewpi

sudo usermod -a -G www-data pi
sudo usermod -a -G brewpi pi


sudo chown -R www-data:www-data /var/www/html/chamber1
sudo chown -R www-data:www-data /var/www/html/chamber2

sudo chown -R brewpi:brewpi /home/brewpi/chamber1
sudo chown -R brewpi:brewpi /home/brewpi/chamber2

sudo find /home/brewpi/chamber1 -type f -exec chmod g+rwx {} \;
sudo find /home/brewpi/chamber2 -type f -exec chmod g+rwx {} \;

sudo find /home/brewpi/chamber1 -type d -exec chmod g+rwxs {} \;
sudo find /home/brewpi/chamber2 -type d -exec chmod g+rwxs {} \;

sudo find /var/www/html/chamber1 -type d -exec chmod g+rwxs {} \;
sudo find /var/www/html/chamber2 -type d -exec chmod g+rwxs {} \;

sudo find /var/www/html/chamber1 -type f -exec chmod g+rwx {} \;
sudo find /var/www/html/chamber2 -type f -exec chmod g+rwx {} \;

sudo reboot



sudo apt-get install git-core


sudo -u brewpi git clone https://github.com/BrewPi/brewpi-script /home/brewpi/chamber1
cd /home/brewpi/chamber1
sudo -u brewpi git checkout legacy



sudo -u brewpi git clone https://github.com/BrewPi/brewpi-script /home/brewpi/chamber2
cd /home/brewpi/chamber2
sudo -u brewpi git checkout legacy



sudo -u www-data git clone https://github.com/BrewPi/brewpi-www /var/www/html/chamber1
cd /var/www/html/chamber1
sudo -u www-data git checkout legacy



sudo -u www-data git clone https://github.com/BrewPi/brewpi-www /var/www/html/chamber2
cd /var/www/html/chamber2
sudo -u www-data git checkout legacy



sudo nano /home/brewpi/chamber1/settings/config.cfg

scriptPath = /home/brewpi/chamber1/

wwwPath = /var/www/html/chamber1/

port = /dev/chamber1

altport = /dev/null

boardType = uno




sudo nano /home/brewpi/chamber2/settings/config.cfg

scriptPath = /home/brewpi/chamber2/

wwwPath = /var/www/html/chamber2/

port = /dev/chamber2

altport = /dev/null

boardType = uno


sudo nano /var/www/html/chamber1/config_user.php


<?php
        // The default settings in config.php are overruled by the settings in config_user.php
        // To use custom settings, copy this file to config_user.php and make your changes in config_user.php
        // do not add a php closing tag, because newlines after closing tag might be included in the html

        // Do not include a trailing slash on the path
        $scriptPath = '/home/brewpi/chamber1';




sudo nano /var/www/html/chamber2/config_user.php


<?php
        // The default settings in config.php are overruled by the settings in config_user.php
        // To use custom settings, copy this file to config_user.php and make your changes in config_user.php
        // do not add a php closing tag, because newlines after closing tag might be included in the html

        // Do not include a trailing slash on the path
        $scriptPath = '/home/brewpi/chamber2';







sudo nano /etc/cron.d/brewpi_chamber1


PYTHON=/usr/bin/python
SCRIPTPATH=/home/brewpi/chamber1

* * * * * brewpi $PYTHON $SCRIPTPATH/brewpi.py --config $SCRIPTPATH/settings/config.cfg --checkstartuponly --dontrunfile; [ $? != 0 ] && $PYTHON -u $SCRIPTPATH/brewpi.py --config $SCRIPTPATH/settings/config.cfg 1>$SCRIPTPATH/logs/stdout.txt 2>>$SCRIPTPATH/logs/stderr.txt &




sudo nano /etc/cron.d/brewpi_chamber2

PYTHON=/usr/bin/python
SCRIPTPATH=/home/brewpi/chamber2

* * * * * brewpi $PYTHON $SCRIPTPATH/brewpi.py --config $SCRIPTPATH/settings/config.cfg --checkstartuponly --dontrunfile; [ $? != 0 ] && $PYTHON -u $SCRIPTPATH/brewpi.py --config $SCRIPTPATH/settings/config.cfg 1>$SCRIPTPATH/logs/stdout.txt 2>>$SCRIPTPATH/logs/stderr.txt &

sudo /etc/init.d/cron restart

sudo reboot





