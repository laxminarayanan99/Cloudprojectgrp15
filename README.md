# Google Cloud RTMP Relay Server
### Introduction : 
Current open-source applications of RTMP streaming to numerous locations are proprietary and not cost-effective (such as Streamyard). This project will provide an open-source application capable of relaying an RTMP stream to many other streaming locations, such as Twitch, X, YouTube, FaceBook live, LinkeIn, Amazon Live, and Vimeo. The Google Cloud platform will be used within this project to facilitate RTMP streaming through a cost-effective and user-friendly application.
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
  * 
