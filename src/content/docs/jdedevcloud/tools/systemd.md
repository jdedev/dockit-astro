---
title : Systemd
---

## Systemd Logs

### Follow Logs

!!!note "Follow Logs"

    ```shell
    journalctl -u code-server@root.service -f
    ```
### Last 50 lines

!!!note "Last 50 lines"

    ```shell
    journalctl -u code-server@root.service -n 50
    ```

### Time range

!!!note "Follow Logs"

    ```shell
    journalctl -u code-server@root.service --since "10 minutes ago"
    ```

## Create New Service

### New Service

!!!note "New Service"

    ```shell
    #!/bin/bash

    SERVICE_NAME="myservice"
    SERVICE_FILE="/etc/systemd/system/$SERVICE_NAME.service"
    SCRIPT_PATH="/usr/local/bin/myscript.sh"

    # Create the systemd service file using a here-document
    cat <<EOF | sudo tee $SERVICE_FILE >/dev/null
    [Unit]
    Description=My Custom Service
    After=network.target

    [Service]
    Type=simple
    ExecStart=$SCRIPT_PATH
    Restart=always
    User=root
    StandardOutput=journal
    StandardError=journal

    [Install]
    WantedBy=multi-user.target
    EOF

    # Reload systemd, enable, and start the service
    sudo systemctl daemon-reload
    sudo systemctl enable $SERVICE_NAME
    sudo systemctl start $SERVICE_NAME

    echo "$SERVICE_NAME service has been created and started successfully."
    echo "Check logs with: journalctl -u $SERVICE_NAME -f"
    ```

### Follow Logs

!!!note "Follow Logs"

    ```
    journalctl -u myservice -f
    ```