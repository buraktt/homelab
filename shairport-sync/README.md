Configuration for auto connecting to bluetooth devices on booth.\
Devices must be paired via `bluetoothctl` before using them

install blueetooth, pulseaudio and necessay modules, edit pulse audio config and enable modules (I don't remember exactly which modules)

`/etc/pulse/system.pa`

`nano /usr/local/bin/auto-connect-bluetooth.sh`

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

`chmod +x /usr/local/bin/auto-connect-bluetooth.sh`

`nano /etc/systemd/system/auto-connect-bluetooth.service`

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

`systemctl enable auto-connect-bluetooth.service`

Useful commands:

bluetoothctl connect 68:F6:3B:D6:87:77

paplay -vvv ./file_example_WAV_1MG.wav.1 --device=bluez_sink.68_F6_3B_D6_87_77.a2dp_sink

pactl list sinks short