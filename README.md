# Process Slice Manager for Hestia CP

Service for managing process slices (cgroup v2) and monitoring them via systemd for Hestia CP Package.
Based on [php](https://github.com/oulfr/php/) by oulfr.

## Setting up Hestia CP Package

1. Activate the "Limit System Resources" plugin in Hestia CP plugins.
2. Create a new package or edit an existing package.
3. In System Resources of the package in CPU Quota, specify the percentage limitation. If the system has 6 cores and you want to allocate, for example, 2.5 cores to this package group, you must specify 250% in the setting.
4. For CPU Quota Period, specify a period between 100ms and 1s.

## Installation

To install the service and ensure it runs on startup, follow these steps:

1. Create the systemd <a href="https://raw.githubusercontent.com/webstudiobond/hestiacp-psm/refs/heads/main/etc/systemd/system/process-slice-manager.service" target="_blank">service</a>:
    ```bash
    sudo nano /etc/systemd/system/process-slice-manager.service
    ```

2. Create a <a href="https://raw.githubusercontent.com/webstudiobond/hestiacp-psm/refs/heads/main/usr/local/hestia/bin/process-slice-manager" target="_blank">script</a> and make it executable:
    ```bash
    sudo nano /usr/local/hestia/bin/process-slice-manager && \
    sudo chmod +x /usr/local/hestia/bin/process-slice-manager
    ```
3. Reload systemd to recognize the new service
   ```bash
   sudo systemctl daemon-reload
   ```
4. Enable service to start it at boot:
    ```bash
    sudo systemctl enable process-slice-manager.service
    ```
5. Start service:
   ```bash
   sudo systemctl start process-slice-manager.service
   ```

## Stop & disable service

1. Stop service:
   ```bash
   sudo systemctl stop process-slice-manager.service
   ```
2. Disable service:
   ```bash
   sudo systemctl disable process-slice-manager.service
   ```

## Monitoring

To monitor the service logs, open another terminal and run the following command:
```bash
sudo tail -f /var/log/process-slice-manager.log
```

Or
```bash
sudo systemd-cgtop
```

## View hierarchy

```bash
sudo systemd-cgls
```

## Package change or update

When you change the hestia package, apply the changes by sending a `HUP` signal to the process manager:
```bash
sudo kill -HUP $(cat /var/run/process-slice-manager.pid)
 ```

Or
```bash
sudo systemctl restart process-slice-manager.service
```
