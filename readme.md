!/bin/bash
s3_bucket="upgradhemadri"
file="/var/www/html/inventory.html"
sudo apt update -y
sudo apt-get install apache2
if [ $(/etc/init.d/apache2 status | grep -v grep | grep 'Apache2 is running' | wc -l) > 0 ]
then
 echo "Apache is running."
else
  echo "Apache is not running."
  sudo systemctl start apache2
fi
sudo update-rc.d apache2 enable
tar -zcf /tmp/hemadri-httpd-logs-$(date '+%d%m%Y-%H%M%S').tar /var/log/apache2/*.log/apache2/*.log
sudo apt update
sudo apt install awscli
aws s3 \
cp /tmp/hemadri-httpd-logs-$(date '+%d%m%Y-%H%M%S').tar \
s3://$(s3_bucket)/hemadri-httpd-logs-$(date '+%d%m%Y-%H%M%S').tar

if [[ -f "$file" ]];
then
    echo " found."
else
    echo "not found."
    vi /root/Automation_Project/inventory.html
fi

00 08-16 * * * /root/Automation_Project/automation.sh
#
