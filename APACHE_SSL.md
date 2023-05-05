# Updating Apache SSL

```
sudo vi /etc/apache2/sites-available/site.com.ssl.conf

sudo tail -n 200 /var/log/apache2/error.log

sudo systemctl restart apache2.service
```
