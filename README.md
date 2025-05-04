# Dell PowerVault MD1200 Fan Control

### When I first got my Dell PowerVault MD1200, the fans were obnoxiously loud.  I now have two units, and with the use of the official Dell password reset cables and a couple serial to USB adapters, I am able to set them at a much quieter 20% fan speed.

*** Credit to [Fohdeesha](https://fohdeesha.com/docs/) for his work on discovering this [here](https://forums.servethehome.com/index.php?threads/fun-with-an-md1200-md1220-sc200-sc220.27487/)

You can find the dell cables on Ebay by searching for "Dell Password Reset/Service Cable MN657 MD1200 MD3200".  This cable has a female DB9 connection so you will need a male DB9 to USB adapter.  Any one on Ebay or Amazon should work.

You will need to verify what ports your cables are using.  An easy way to do this is plug in the device and then run:

```
dmesg | grep -i tty
```

You should see an entry saying a device was attached to ttyS0 or ttyUSB0.  The output may vary on your device.

I have both of my MD1200s connected to the same host, one directly with the DB9 connection on the password reset cable, and one using a USB adapter.  I created a script that sends commands to each MD1200 four times, with a 2 second sleep in between.  Sometimes there can be an issue where the command doesn't take and you have to run it again.  I just have it run 4 times for each one just to make sure.

```
stty -F /dev/ttyS0 38400 raw -echoe -echok -echoctl -echoke
echo -e -n 'set_speed 20\r'> /dev/ttyS0
sleep 2
echo -e -n 'set_speed 20\r' > /dev/ttyS0
sleep 2
echo -e -n 'set_speed 20\r' > /dev/ttyS0
sleep 2
echo -e -n 'set_speed 20\r' > /dev/ttyS0

stty -F /dev/ttyUSB0 38400 raw -echoe -echok -echoctl -echoke
echo -e -n 'set_speed 20\r' > /dev/ttyUSB0
sleep 2
echo -e -n 'set_speed 20\r' > /dev/ttyUSB0
sleep 2
echo -e -n 'set_speed 20\r' > /dev/ttyUSB0
sleep 2
echo -e -n 'set_speed 20\r' > /dev/ttyUSB0
```
