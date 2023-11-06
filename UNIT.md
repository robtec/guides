path `/lib/systemd/system/pyapp.service`

```
[Unit]
Description=Python App Service
After=multi-user.target

[Service]
Type=simple
WorkingDirectory=/home/admin/
ExecStart=/usr/bin/python3 /home/admin/app/app.py
User=admin

[Install]
WantedBy=multi-user.target
```
