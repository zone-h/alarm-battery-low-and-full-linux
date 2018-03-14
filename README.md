# alarm-battery-low-and-full-linux

sumber : https://askubuntu.com/questions/470880/how-can-i-play-a-custom-sound-when-my-battery-is-low

How can I play a custom sound when my battery is low?
or
how to create audio play if battery low and full ?

![image1](https://raw.githubusercontent.com/zone-h/alarm-battery-low-and-full-linux/master/Screenshot%20from%202018-03-14%2013-33-02.png)

	Step 1: Install mpg3

sudo apt-get install acpi mpg123

	Step 2: Save the following command in ~/bin/battery_alert
```sh
#!/bin/bash
PATH=/opt/someApp/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
if [ `acpi -b | awk ' { print ($3)}'`  == "Discharging," ] ; then
    # Discharging
    # Monitor for low battery
    if [ `acpi -b | awk ' { print ($4)-0}'`  -le "15" ] ; then
        pactl set-sink-volume 0 75\% && pactl set-sink-mute 0 0 && mpg123 /home/user/battery_low.mp3 ;
    fi
else
    # Charging
    if [ `acpi -b | awk ' { print ($4)-0}'`  -eq "100" ] ; then
        # Fully charged
        pactl set-sink-volume 0 75\% && pactl set-sink-mute 0 0 && mpg123 /home/user/battery_full.mp3 ;
    fi
fi

```

	Step 3: Make the file executable using the following command.

chmod +x /bin/battery_alert

	Step 4: Execute this file using cron by adding the following command to the end of the file opend by crontrab -e command.

```sh
*/5 * * * * /bin/battery_alert
```
(Don't forget to have an empty line after this command)

cat:
sesuaikan dengan user masing-masing di baris ke 21 dan 27.
