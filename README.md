# Temporary documentation for JTN setup with Ubuntu 20.04 (JetPack 4.6 - L4T 32.6.1)

## Step 1: Download image

[Qengineering](https://github.com/Qengineering/Jetson-Nano-Ubuntu-20-image/tree/main) has an image for the NVIDIA Jetson Nano running JetPack 4.6 and Ubuntu 20.04, in addition to OpenCV compiled with CUDA support, TensorFlow, PyTorch, TorchVision, TensorRT and jtop.

As an example, this is what my machine outputs after I flashed the image and ran it on the JTN.

![image](https://github.com/kiers-neely/jtn-ubuntu-20/assets/161119406/80ca9916-8868-4e53-af06-15ccf56555e5)

You can follow the [installation steps he has listed](https://github.com/Qengineering/Jetson-Nano-Ubuntu-20-image/tree/main#installation) to download and flash your own SD card.

## Step 2: Configure the JTN with the new image

For the first time logging into the Jetson, you will have to connect the JTN to your laptop via microUSB. SSH in with the commands

```sh
xhost +
ssh jetson@192.168.55.1
```

Password: `jetson`

Once inside the JTN, you should see

```sh
jetson@nano:~$
```

To set up your JTN to the UCSDRoboCar WiFi access point:

Activate network.

```sh
sudo systemctl start networking.service
```

List available networks.

```sh
sudo nmcli device wifi list
```

Connect to a RoboCar network.

```sh
sudo nmcli device wifi connect UCSDRoboCar password UCSDrobocars2018
```

Connection should be confirmed for `wlan0`. Now get the IP address of your JTN to connect remotely.

```sh
ifconfig
```

Take note of the IP address at `inet` (i.e. `192.168.11.63`). 

To connect via WiFi, open a terminal and SSH using the IP you just noted.

```sh
ssh jetson@192.168.11.63
```

The password should still be the same: `jetson`. Once you're in, you can change the hostname and password. First, turn off Power Saving for the WiFi device.

```sh
sudo iw dev wlan0 set power_save off
```

Install `nano` for simple file editing from the command line.

```sh
sudo apt-get update
sudo apt-get install nano
```

Edit the file to make the WiFi settings consistent.

```sh
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```

Change `wifi.powersave` from `3` to `2`.

```sh
wifi.powersave = 2
```

Now let's change the hostname and password. View the current configuration:

```sh
hostnamectl
```

Change the hostname by editing the file with `nano`.

```sh
sudo nano /etc/hosts
```

You might see the following output:

```
127.0.0.1    nano
127.0.0.0    jetson
```

For this example, I changed `nano` to `ucsdrobocar-148-exp` for experimental.

```
127.0.0.1    ucsdrobocar-148-exp
127.0.0.0    jetson
```

Now change the password.

```
passwd
```

After running this command you will be prompted to enter the old password and then your new password.









