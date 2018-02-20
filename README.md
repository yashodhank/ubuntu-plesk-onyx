# Plesk Onyx custom setup on Ubuntu 16.04 LTS

**1) System update and packages cleanup**

```
apt-get update && apt-get upgrade -y && apt-get autoremove -y && apt-get clean
```

**2) Install useful packages**
```
sudo apt install haveged curl git unzip zip htop -y
```

**3) Tweak Kernel sysctl configuration**
```
wget -O /etc/sysctl.conf https://raw.githubusercontent.com/VirtuBox/ubuntu-nginx-web-server/master/etc/sysctl.conf
sysctl -p
echo never > /sys/kernel/mm/transparent_hugepage/enabled
wget -O /etc/security/limits.conf https://raw.githubusercontent.com/VirtuBox/ubuntu-nginx-web-server/master/etc/security/limits.conf
```

**4) Install netdata monitoring**
```
bash <(curl -Ss https://my-netdata.io/kickstart.sh) all

# save 40-60% of netdata memory
echo 1 >/sys/kernel/mm/ksm/run
echo 1000 >/sys/kernel/mm/ksm/sleep_millisecs

# disable email notifications
wget -O /etc/netdata/health_alarm_notify.conf https://raw.githubusercontent.com/VirtuBox/ubuntu-nginx-web-server/master/etc/netdata/health_alarm_notify.conf
service netdata restart
```

**5) Install MariaDB 10.1** (do not set any root password)
```
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup |
sudo bash -s -- --mariadb-server-version=10.1 --skip-maxscale

sudo apt update
sudo apt install mariadb-server
```

**6) Install the lastest Plesk Onyx release**
```
sh <(curl https://autoinstall.plesk.com/plesk-installer || wget -O - https://autoinstall.plesk.com/plesk-installer)
```

**7) Enable vps_optimized mode**
```
plesk bin vps_optimized --turn-on
```

**8) Enable pci_compliance configuration**
```
plesk sbin pci_compliance_resolver --enable all
```

**9) Compile the last Nginx release with plesk-nginx bash script**
```
bash <(wget -O - https://raw.githubusercontent.com/VirtuBox/plesk-nginx/master/plesk-nginx.sh)
```

**10) Set Panel.ini custom configuration**
```
wget https://raw.githubusercontent.com/VirtuBox/ubuntu-plesk-onyx/master/usr/local/psa/admin/conf/panel.ini -O /usr/local/psa/admin/conf/panel.ini

```
