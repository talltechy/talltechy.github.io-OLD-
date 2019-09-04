# wiki.service - Run as service

There are several solutions to run Wiki.js as a background service. We'll focus on systemd in this guide as it's available in nearly all linux distributions.

## 1. Create a new file named <code>wiki.service</code> inside directory <code>/etc/systemd/system</code>

```bash
nano /etc/systemd/system/wiki.service
```

## 2. Paste the following contents (our wiki is installed at <code>/wiki2</code>)

```ini
[Unit]
Description=Wiki.js
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/node server
Restart=always
# Consider creating a dedicated user for Wiki.js here:
User=root
Environment=NODE_ENV=production
WorkingDirectory=/wiki2

[Install]
WantedBy=multi-user.target
```

## 3. Save the service file (CTRL+X, followed by Y)

## 4. Reload systemd

```bash
systemctl daemon-reload
```

## 5. Run the service

```bash
systemctl start wiki
```

## 6. Enable the service on system boot

```bash
systemctl enable wiki
```

## 7. Check if the service is up

```bash
systemctl status wiki -l
```

> Note: You can see the logs of the service using <code>journalctl -u wiki -l</code>
