# Google Cloud RTMP Relay Server
### Introduction : 
Current open-source applications of RTMP streaming to numerous locations are proprietary and not cost-effective (such as Streamyard). This project will provide an open-source application capable of relaying an RTMP stream to many other streaming locations, such as Twitch, X, YouTube, FaceBook live, LinkeIn, Amazon Live, and Vimeo. The Google Cloud platform will be used within this project to facilitate RTMP streaming through a cost-effective and user-friendly application.


### Testbed and Software Tools:
* Google Cloud and related modules/services.
* Python3 (**programming language is tentative) 
* nginx+RTMP
* Streaming Softwares (StreamYard, OBS, YouTube, Twitch, etc.)

### Prerequisites :
 * Ubuntu 20.04 server
 * Nginx



### Steps to follow
#### Step 1 — Installing and Configuring Nginx-RTMP
 * Begin by running the following commands as a non-root user to update your package listings and install the Nginx module:
  ```sh
  sudo apt update
  sudo apt install libnginx-mod-rtmp
  ```
 * Using nano or your favorite text editor, open Nginx’s main configuration file, /etc/nginx/nginx.conf, and add this configuration block to the end of the file:
  ```sh
  sudo nano /etc/nginx/nginx.conf
  ```
  ![image](https://github.com/laxminarayanan99/Cloudprojectgrp15/assets/127360611/a24f1c4a-199f-44f1-a63a-8e81cc001cf5)
  * This provides the beginning of your RTMP configuration. By default, it listens on port 1935, which means you’ll need to open that port in your firewall. If you configured ufw as part of your initial server setup run the following command.
  ```sh
  sudo ufw allow 1935/tcp
  ```
  * reload Nginx with your changes
   ```sh
   sudo systemctl reload nginx.service
   ```
#### Step 2 — Sending Video to Your RTMP Server
  * First, install Python and its package manager then use pip to install youtube-dl. Once these are done then you can use use youtube-dl to download a video from YouTube. Now with a video file in your current directory with a title 'some_sample.mkv' . In order to stream it, you’ll want to install ffmpeg
  ```sh
  sudo apt install python3-pip
  sudo pip install youtube-dl
  sudo apt install ffmpeg  
  ```
  * use ffmpeg to send it to your RTMP server
  ```sh
   ffmpeg -re -i "some_sample.mkv" -c:v copy -c:a aac -ar 44100 -ac 1 -f flv rtmp://localhost/live/stream
  ```
#### Step 3 — Streaming Video to Your Server via OBS 
###### Step 3.1: Get a Server Box 
  * Recommended using Ubuntu for the server software for the sake of ease, but you can obviously use whatever you want. As long as you get the dependencies for nginx somewhere besides apt, you can follow this guide just fine.
  * Note to Windows users: This guide focuses on using Linux. If you want to use Windows, you can find Windows binaries for nginx with the RTMP module already included here: http://nginx-win.ecsds.eu/download/
  * Note to Mac users: You can install nginx with the RTMP module via Homebrew: http://brew.sh/homebrew-nginx/  
###### Step 3.2: Installing nginx with RTMP module
  * Log into your box, and make sure you have the necessary tools to build nginx using the following command:
  ```sh
  sudo apt-get install build-essential libpcre3 libpcre3-dev libssl-dev
  ```
  * download the nginx source code
  ```sh
  wget http://nginx.org/download/nginx-1.15.1.tar.gz
  ```
  * get the RTMP module source code from git
  ```sh
  $ wget https://github.com/sergey-dryabzhinsky/nginx-rtmp-module/archive/dev.zip
  ```
  * 
  ```sh
  tar -zxvf nginx-1.15.1.tar.gz
  unzip dev.zip
  cd nginx-1.15.1
  ```
  * build nginx
  ```sh
  ./configure --with-http_ssl_module --add-module=../nginx-rtmp-module-dev
  make
  sudo make install
  ```
  * start the server
  ```sh
  sudo /usr/local/nginx/sbin/nginx
  ```
###### Step 3.3: Configuring nginx to use RTMP
  * Open your config file, located by default at /usr/local/nginx/conf/nginx.conf and add the following at the very end of the file.
![image](https://github.com/laxminarayanan99/Cloudprojectgrp15/assets/127360611/cf3cf50c-a8ac-4fcd-b000-6d9fa1e0db54)

  * Restart nginx
  ```sh
  sudo /usr/local/nginx/sbin/nginx -s stop
  sudo /usr/local/nginx/sbin/nginx
  ```
###### Step 3.4: Server Ready
  * Create a new profile in OBS, and change your Broadcast Settings
