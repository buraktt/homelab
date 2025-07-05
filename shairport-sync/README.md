
/usr/local/bin/[auto-connect-bluetooth.sh](http://auto-connect-bluetooth.sh)

```
#!/bin/bash
sleep 10  # Wait for 10 seconds to ensure Bluetooth is ready
devices=("68:F6:3B:D6:C4:95" "68:F6:3B:D6:87:77") # Add MAC addresses here

for device in "${devices[@]}"; do
    echo "Connecting to $device..."
    bluetoothctl <<EOF
connect $device
EOF
done
```

/etc/systemd/system/auto-connect-bluetooth.service

```
[Unit]
Description=Auto-connect Bluetooth devices
After=bluetooth.service
Requires=bluetooth.service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/auto-connect-bluetooth.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```