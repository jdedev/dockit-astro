---
title: Rclone and Systemd
---

## Miscellaneous

### List existing services

    ```bash
    systemctl list-unit-files | grep enabled
    ```

    ```bash
    systemctl list-unit-files | grep rclone
    ```

## Setup

### Create Rclone mount Systemd service

    ```bash
    sudo nano /etc/systemd/system/rclone-mount.service
    ```

> Rclone mount Systemd service

    ```bash
    [Unit]
    Description=Mount Rclone Remote
    After=network-online.target
    Wants=network-online.target

    [Service]
    Type=simple
    ExecStart=/usr/bin/rclone mount onedrive: /home/ubuntu/rclone \
        --config /home/ubuntu/.config/rclone/rclone.conf \
        --vfs-cache-mode writes \
        --dir-cache-time 12h \
        --log-file /var/log/rclone.log \
        --log-level INFO \
        --allow-other
    ExecStop=/usr/bin/fusermount -u /home/ubuntu/rclone
    Restart=always
    User=root
    Group=root
    Environment="PATH=/usr/bin:/bin"
    ExecStartPre=/bin/sleep 10

    [Install]
    WantedBy=multi-user.target
    ```

### Create Backup folder(s) script

> Create Backup script (will be used in Systemd service)

    ```bash
    nano create_backup.sh
    ```

> Backup script

    ```bash
    #!/bin/bash

    # Define variables
    BACKUP_DIR="/home/ubuntu/pangolin"
    RCLONE_DIR="/home/ubuntu/rclone"
    HOSTNAME=$(hostname)
    DATE=$(date +"%Y-%m-%d")
    TIME=$(date +"%H-%M-%S")
    DEST_DIR="$RCLONE_DIR/backup/oci1/$HOSTNAME/$DATE"  # Folder named hostname-date
    FILENAME="$HOSTNAME-$DATE-$TIME-pangolin.tar.bz2"  # Filename with date-time

    # Define misc files
    MISC_FILES=(
      "/etc/systemd/system/backup-pangolin.service"
      "/etc/systemd/system/backup-pangolin.timer"
      "/home/ubuntu/create_backup.sh"
    )

    # Create the destination folder and misc folder inside the destination
    mkdir -p "$DEST_DIR/misc"

    # Copy misc files to the misc folder
    for FILE in "${MISC_FILES[@]}"; do
      cp "$FILE" "$DEST_DIR/misc/"
    done

    # Create the tar.bz2 backup, including the misc folder
    tar -cvjf "$DEST_DIR/$FILENAME" -C "$BACKUP_DIR" . -C "$DEST_DIR" misc

    rm -rf "$DEST_DIR/misc"

    # Optional: Log the backup creation
    echo "Backup of $BACKUP_DIR and misc files created at $DEST_DIR/$FILENAME" >> /home/ubuntu/backup_log.txt
    ```

### Create Backup Systemd Service

    ```bash
    sudo nano /etc/systemd/system/backup-pangolin.service
    ```

> Backup Systemd Service

    ```bash
    [Unit]
    Description=Backup Pangolin Folder

    [Service]
    Type=oneshot
    ExecStart=/home/ubuntu/create_backup.sh
    User=root
    Group=root
    ```

### Restart Systemd services

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl start backup-pangolin.service
    sudo systemctl status backup-pangolin.service
    sudo systemctl enable backup-pangolin.service
    ```

### Create Systemd Timer

    ```bash
    sudo nano /etc/systemd/system/backup-pangolin.timer
    ```

> Timer

    ```bash
    [Unit]
    Description=Run Backup Pangolin Daily

    [Timer]
    OnCalendar=daily
    Unit=backup-pangolin.service

    [Install]
    WantedBy=timers.target
    ```

### Restart Systemd services

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl start backup-pangolin.timer
    sudo systemctl status backup-pangolin.timer
    sudo systemctl enable backup-pangolin.timer
    ```

### Get Timer Status

!!! note ""

    ```bash
    sudo systemctl status backup-pangolin.timer
    ```

> Output

    ```bash
    ● backup-pangolin.timer - Run Backup Pangolin Daily
        Loaded: loaded (/etc/systemd/system/backup-pangolin.timer; enabled; preset: enabled)
        Active: active (waiting) since Sun 2025-03-16 22:29:17 UTC; 6min ago
        Trigger: Mon 2025-03-17 00:00:00 UTC; 1h 24min left
      Triggers: ● backup-pangolin.service

    Mar 16 22:29:17 oci01-amd01 systemd[1]: Started backup-pangolin.timer - Run Backup Pangolin Daily.
    ```
