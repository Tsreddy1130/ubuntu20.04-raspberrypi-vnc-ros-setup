# ubuntu20.04-raspberrypi-vnc-ros-setup
 installing desktop in ubuntu20.04 server and vncserver setup on Raspberrypi
# Setting Up Ubuntu 20.04 Server on Raspberry Pi with Desktop Environments and VNC Server

This guide provides step-by-step instructions to install Ubuntu 20.04 server on a Raspberry Pi, set up lightweight desktop environments (XFCE4 or LXDE), and configure a remote VNC server compatible with ROS tools like RViz.

---

## Prerequisites
1. Raspberry Pi with Ubuntu 20.04 Server installed.
2. connect pi via SSH or any diplay,keyboard(if avilable)
3. Internet connectivity.

---

## Step 1: Update and Upgrade System
```bash
sudo apt update
sudo apt upgrade -y
```

---
### **Note**: If you want better UI and graphics go with LXDE(recomended)
## Step 2: Install XFCE Desktop Environment
XFCE is a lightweight desktop environment suitable for remote desktop setups.
```bash
sudo apt install xfce4 xfce4-goodies -y
```

---

## Step 3: Install and Configure TightVNC Server
1. Install TightVNC Server:
   ```bash
   sudo apt install tightvncserver -y
   ```
2. Start VNC server and set a password:
   ```bash
   vncserver
   ```
3. Stop the VNC server to configure it:
   ```bash
   vncserver -kill :1
   ```
4. Configure the `~/.vnc/xstartup` file:
   ```bash
   vim ~/.vnc/xstartup
   ```
   Add the following content:
   ```bash
   #!/bin/sh
   DESKTOP_SESSION=xfce
   export DESKTOP_SESSION
   startxfce4
   vncserver-virtual -kill $DISPLAY
   ```
   Save and exit.
5. Reboot the system:
   ```bash
   sudo reboot
   ```
6. Start VNC server:
   ```bash
   vncserver
   ```
   7.Intsall vnc client on your laptop (tightvncclient (or) tigervncclient ,You need to type your localip:1 to connect to  vnc client.exmaple:`192.168.0.12:1`
   To kow your ip adress:
   ```bash
   ipa
   ```
   ```bash
   hostname -I
   ```

### **Note**: XFCE and tightvncserver may have compatibility issues with some ROS tools like RViz. For better compatibility, proceed with LXDE and tigervncserver (or) you can use LXDE and tightvnserver which vnc you like,LXDE has more graphics ui than xfce.

---

## Step 4: Install LXDE Desktop Environment
LXDE is an even lighter desktop environment, ideal for minimal resource usage.
```bash
sudo apt install lxde -y
```
## Step 5: Install TigerVNC Server
TigerVNC is recommended for better compatibility with ROS tools like RViz.
1. Remove other VNC server installations:
   ```bash
   sudo apt purge tightvncserver -y
   ```
2. Install TigerVNC Server:
   ```bash
   sudo apt install tigervnc-standalone-server -y
   ```
   3. Configure the `~/.vnc/xstartup` file for LXDE:
   ```bash
   vim ~/.vnc/xstartup
   ```
   Add the following content:
   ```bash
   #!/bin/sh
   DESKTOP_SESSION=LXDE
   export DESKTOP_SESSION
   startlxde
   # Alternative:
   # dbus-launch --exit-with-session startlxde
   vncserver-virtual -kill $DISPLAY
   ```
3. Reboot the system:
   ```bash
   sudo reboot
   ```
---
4. Start TigerVNC Server with `localhost` access disabled:
   ```bash
   vncserver :1 -localhost no
   ```
5. Configure the firewall to allow VNC traffic:
   ```bash
   sudo ufw allow 5901/tcp
   sudo ufw status
   ```
   6.Intsall vnc client on your laptop (tightvncclient (or) tigervncclient ,You need to type your localip:1 to connect to  vnc client.exmaple:`192.168.0.12:1`
   To kow your ip adress:
   ```bash
   ipa
   ```
   ```bash
   hostname -I
   ```

## Additional Notes for ROS Compatibility
1. Ensure all desktop environments have the necessary tools for running ROS GUI applications.
2. For better ROS visualization (e.g., RViz), TigerVNC is more reliable than TightVNC.

---

## References
- [RealVNC Custom xstartup Files](https://help.realvnc.com/hc/en-us/articles/360003474792-Why-does-RealVNC-Server-in-Virtual-Mode-on-Linux-appear-to-hang-show-a-gray-screen-or-not-start-at-all#custom-xstartup-files-0-1)
- [How to Install a GUI on Ubuntu](https://phoenixnap.com/kb/how-to-install-a-gui-on-ubuntu)
