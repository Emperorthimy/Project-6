## **Documentation for Project 6**

### Entire View of the newly created Block Devices 

`lsblk`
![list-of-available-volumes](./Images-Webserver/f-disk-partition.png)

### Partioning Block Devices
`sudo gdisk /dev/xvdf`
`sudo gdisk /dev/xvdg`
`sudo gdisk /dev/xvdh`
![adding-partition-xvdf](./Images-Webserver/f-disk.png)
![adding-partition-xvdg](./Images-Webserver/xvdg.png)
![adding-partition-xvdh](./Images-Webserver/xvdh.png)


### Entire View of all Partitioned Block
`lsblk`
![Entire-view-of-all-partitioned-Blocks](./Images-Webserver/Entire-partition.png)

### LVM2 Installation
`sudo yum install lvm2`
![Installing-LVM2](./Images-Webserver/installing-lvm2.png)

### LVM2 Installed
`sudo lvmdiskscan`
![Installed-LVM2](./Images-Webserver/lvmdiskscan.png)

### Marking the Partitions as Physical Volumes
`sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`
`sudo pvs`
![Marking-partitions-as-physical-volumes](./Images-Webserver/creating-physical-volume.png)


### Creating-Volume-Group
`sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`
`sudo vgs`
![Creating-Volume-Group](./Images-Webserver/vg-created.png)


### Creating-Logical-Volumes
`sudo lvcreate -n apps-lv -L 14G webdata-vg`
`sudo lvcreate -n logs-lv -L 14G webdata-vg`
`sudo lvs`
![Logical-Volumes-Created](./Images-Webserver/logical-volume-created.png)

### Formatting-Logical-Volumes-with-EXT4-Filesystem
`sudo mkfs -t ext4 /dev/webdata-vg/app-lv`
`sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`
![Formatting-Logical-Volumes](./Images-WebServer/formatting-logical-volumes.png)

### Creating-Directory-to-Store-website-files and mounting web files
`sudo mkdir -p /var/www/html`
![Directory-for-website-files](./Images-WebServer/create-dir%26mount-apps-lv.png)

### Creating-Directory-to-Store-Log-Files and mount log files
`sudo mkdir -p /home/recovery/logs`
![Directory-for-Log-files](./Images-WebServer/create%20recovery%26logfiles.png)



### Updating-fstab
`sudo vi /etc/fstab`
![fstab-updated](./Images-WebServer/wordpress-mount.png)

### Entire-setup-of-Volumes
`df -h`
![View-of-the-entire-setup-of-volumes](./Images-Webserver/mount-reload.png)
![mountpoints](./Images-WebServer/mountpoints.png)
## **Preparing Database Server**

### Volumes-Attached-for-DB-Server
`lsblk`
![volumes-attached](./Images-Database/disk.png)


### Volumes-successfully-partitioned
`sudo gdisk /dev/xvdf`
`sudo gdisk /dev/xvdg`
![volumes-successfully-partitioned](./Images-Database/xvdf-db.png)
![volumes-successfully-partitioned](./Images-Database/xvdg-db.png)

### Volumes-marked-as-physical-volumes
`sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`
![Physical-volumes-created](./Images-Database/physical-volumes.png)

### Installing LVM2 for DB Server
`sudo yum install lvm2`
`sudo lvmdiskscan`
![LVM2-Installed](./Images-Database/lvm2.png)
![LVM2-Installed](./Images-Database/lvmdiskscan.png)
### Creating Volume Groups for DBServer and Logical Volume
`sudo vgcreate vg-dbdatabase /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`
`sudo vgs`
`sudo lvcreate -n db-lv -l 14G vg-dbdatabase`
`sudo lvs`
![Volume-Group-Created](./Images-Database/vg-lv-created.png)



### Formatting-Logical-Volumes-with-EXT4-Filesystem
`sudo mkfs -t ext4 /dev/vg-dbdatabase/db-lv`
![Logical-volume-formatted](./Images-Database/format-disk.png)

### Logical Volume Mounted on DB Directory
`sudo mount /dev/vg-dbdatabase/db-lv /db`
`sudo mount -a`
![Logical-volume-Mounted](./Images-Database/mount%25reload.png)

### Updating fstab for DB
`sudo vi /etc/fstab`
![fstab-Updated-for-DB](./Images-Database/fstab.png)
![Mountpoints](./Images-Database/mountpoints.png)
### mysql installation on DBServer
`sudo yum update`
`sudo yum install mysql-server`
![my-sql-server-installed](./Images-Database/mysql.png)

## **Installing Wordpress on Webserver**
### Installing wget and its dependencies
`sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`
![wget-and-dependencies-installation](./Images-Webserver/installing-apache%26dependecies.png)

## **Installing PHP and it's dependecies**
##### Had to use another repo and key from php documentation
`sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`
`sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`
`sudo yum module list php`
`sudo yum module reset php`
`sudo yum module enable php`
`sudo yum install php php-opcache php-gd php-curl php-mysqlnd`
![fstab-Updated-for-DB](./Images-Webserver/installing-php-dependecies.png)
![fstab-Updated-for-DB](./Images-Webserver/php-installed%26enabled.png)
![fstab-Updated-for-DB](./Images-Webserver/php.png)

### Download wordpress and copy wordpress to var/www/html
` mkdir wordpress`
`cd wordpress`
`sudo wget http://wordpress.org/latest.tar.gz`
`cp -R wordpress /var/www/html/`
![wordpress-download](./Images-WebServer/wordpress.png)

### Configuring SElinux Policies
`sudo chown -R apache:apache /var/www/html/wordpress`

`sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R`

`sudo setsebool -P httpd_can_network_connect=1`
![Selinux-Policies-configuration](./Images-WebServer/selinux.png)

### Configure DB to work with WordPress
`show databases`
![show-databases-command-executed-on-webserver-to-show-list-of-databases-on-dbserver](./Images-Database/database.png)

### Configure WordPress to connect to remote database
`sudo mysql -u myuser -p -h 172.31.22.12`
![User-connected-to-Dbserver-from-Webserver](./Images-Database/connect-wordpress-with-remote-user.png)

### link to WordPress accessed from Browser
`http://3.140.246.8/wordpress/`
![WordPress-accessed-from-Browser](./Images-WebServer/Wordpress-accessed.png)

### WordPress successfully Deployed
`http://3.140.246.8/wordpress/`
![WordPress-Deployed](./Images-WebServer/successfully-deployed-wordpress.png)