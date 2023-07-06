## **Documentation for Project 6**

### Entire View of the newly created Block Devices 

`lsblk`
![list-of-available-volumes](./Images-Webserver/)



### Partioning Block Devices
`sudo gdisk /dev/xvdf`
![adding-partition-xvdf](./images/f-disk.png)
![adding-partition-xvdg](./images/xvdg.png)
![adding-partition-xvdh](./images/xvdh.png)

### Entire View of all Partitioned Block
`lsblk`
![Entire-view-of-all-partitioned-Blocks](./Images/Entire-partition.png)

### LVM2 Installation
`sudo yum install lvm2`
![Installing-LVM2](./Images/installing-lvm2.png)

### LVM2 Installed
`sudo lvmdiskscan`
![Installed-LVM2](./Images/lvmdiskscan.png)

### Marking the Partitions as Physical Volumes
`sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`
`sudo pvs`
![Marking-partitions-as-physical-volumes](./Images/creating-physical-volume.png)


### Creating-Volume-Group
`sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`
`sudo vgs`
![Creating-Volume-Group](./Images/vg-created.png)


### Creating-Logical-Volumes
`sudo lvcreate -n apps-lv -L 14G webdata-vg`
`sudo lvcreate -n logs-lv -L 14G webdata-vg`
`sudo lvs`
![Logical-Volumes-Created](./Images/logical-volume-created.png)

### Formatting-Logical-Volumes-with-EXT4-Filesystem
`sudo mkfs -t ext4 /dev/webdata-vg/app-lv`
`sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`
![Formatting-Logical-Volumes](./Images-WebServer/formating-logical-volumes-with-ext4-file-system.png)

### Creating-Directory-to-Store-website-files
`sudo mkdir -p /var/www/html`
![Directory-for-website-files](./Images-WebServer/creating-directory-to-store-website-files.png)

### Creating-Directory-to-Store-Log-Files
`sudo mkdir -p /home/recovery/logs`
![Directory-for-Log-files](./Images-WebServer/creating-directory-to-store-log-files.png)

### Mounting-Website-files-directory-on-app-lv
`sudo mount /dev/webdata-vg/apps-lv /var/www/html`
![Mounting-Directory-on-app-lv](./Images-WebServer/app-lv-mounted-on-directory-html-created.png)

### Mounting-Log-files-directory-on-logs-lv
`sudo mount /dev/webdata-vg/logs-lv /var/log`
![Mounting-Log-Directory-on-logs-lv](./Images-WebServer/log-files-mounted-on-logs-lv.png)

### Updating-fstab
`sudo vi /etc/fstab`
![fstab-updated](./Images-WebServer/fstab-updated.png)

### Entire-setup-of-Volumes
`df -h`
![View-of-the-entire-setup-of-volumes](./Images-WebServer/entire-setup-view-of-volumes.png)

## **Preparing Database Server**

### Volumes-Attached-for-DB-Server
`lsblk`
![volumes-attached](./Images-DBServer/Volumes-attached-for-DBServer.png)


### Volumes-successfully-partitioned
`sudo gdisk /dev/xvdf`
![volumes-successfully-partitioned](./Images-DBServer/Successful-partitioning-of-volumes.png)

### Volumes-marked-as-physical-volumes
`sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`
![Physical-volumes-created](./Images-DBServer/Volumes-marked-as-physical-volumes.png)

### Installing LVM2 for DB Server
`sudo yum install lvm2`
![LVM2-Installed](./Images-DBServer/lvm2-installed-for-dbSever.png)

### Creating Volume Groups for DBServer
`sudo vgcreate vg-dbdatabase /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`
`sudo vgs`
![Volume-Group-Created](./Images-DBServer/volumes-successfully-attached-to-a-volume-group.png)

### Creating Logical Volumes
`sudo lvcreate -n db-lv -l 20G vg-dbdatabase`
`sudo vgs`
`sudo lvs`
![Logical-Volumes-Created](./Images-DBServer/logical-volume-created-for-dbServer.png)

### Formatting-Logical-Volumes-with-EXT4-Filesystem
`sudo mkfs -t ext4 /dev/vg-dbdatabase/db-lv`
![Logical-volume-formatted](./Images-DBServer/formatting-logical-volume-with-ext4.png)

### Logical Volume Mounted on DB Directory
`sudo mount /dev/vg-dbdatabase/db-lv /db`
![Logical-volume-Mounted](./Images-DBServer/logical-volume-mounted-on-db-directory.png)

### Updating fstab for DB
`sudo vi /etc/fstab`
![fstab-Updated-for-DB](./Images-DBServer/fstab-updated.png)
### mysql installation on DBServer
`sudo yum update`
`sudo yum install mysql-server`
![my-sql-server-installed](./Images-DBServer/my-sql-server-installed-on-dbserver.png)

## **Installing Wordpress on Webserver**
### Installing wget and its dependencies
`sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`
![wget-and-dependencies-installation](./Images-WebServer/wget-and-it-dependencies-installed.png)

### Download wordpress and copy wordpress to var/www/html
` mkdir wordpress`

`cd wordpress`

`sudo wget http://wordpress.org/latest.tar.gz`

`cp -R wordpress /var/www/html/`
![wordpress-download](./Images-WebServer/WordPress-downloaded-and-copied-to-var-www-html.png)

### Configuring SElinux Policies
`sudo chown -R apache:apache /var/www/html/wordpress`

`sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R`

`sudo setsebool -P httpd_can_network_connect=1`
![Selinux-Policies-configuration](./Images-WebServer/SElinux-policies-configured.png)

### Configure DB to work with WordPress
`show databases`
![show-databases-command-executed-on-webserver-to-show-list-of-databases-on-dbserver](./Images-WebServer/show-databases-command-executed-on-webserver-to-show-list-of-databases-on-dbserver.png)

### Configure WordPress to connect to remote database
`sudo mysql -u dbuser -p -h 172.31.30.247`
![User-connected-to-Dbserver-from-Webserver](./Images-WebServer/user-connected-to-dbserver-successfully-from-webserver.png)

### link to WordPress accessed from Browser
`http://54.89.100.91/wordpress/`
![WordPress-accessed-from-Browser](./Images-WebServer/Wordpress-accessed.png)

### WordPress successfully Deployed
`http://54.89.100.91/wordpress/`
![WordPress-Deployed](./Images-WebServer/successfully-deployed-wordpress.png)