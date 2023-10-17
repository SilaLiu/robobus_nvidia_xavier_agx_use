## Robobus_Camera_Use
---

  GEAC91VP User Manual v1.1
  https://drive.google.com/file/d/1RDw9sllvRE_9UQxhfdQE6hTSu7PLRZjA/view?usp=drive_link
  
### You need to install the camera-related configuration package first
  tztek-cam-app_2.0.0_arm64 .deb                        
  https://drive.google.com/file/d/1VMStmcU8CqG8Z3s5tzHAZI6SKpdOQdei/view?usp=drive_link
  
  tztek-jetson-service-camera-config-510v-v2.7.deb      
  https://drive.google.com/file/d/1Uvw3luBPjzQcYSYDCBiPwiE4mRPPmlK1/view?usp=drive_link
  
  tztek-jetson-tool-camera-show-v1.0 .deb               
  https://drive.google.com/file/d/1tfz0euIGGZex7b8H-FqikjuLivq_lZ3x/view?usp=drive_link
  
  tztek-jetson-tool-internal-trigger-camera-v1.1.deb    
  https://drive.google.com/file/d/1DbjF7zrKQX9ddb6Uu8_oAsT-Gdgsgbmu/view?usp=drive_link

  sudo dpkg -i tztek-jetson-service-camera-config-510vp-v2.7.deb
  sudo dpkg -i tztek-cam-app_2.0.0_arm64.deb
  sudo dpkg -i tztek-jetson-tool-camera-show-v1.0.deb
  sudo dpkg -i tztek-jetson-tool-internal-trigger-camera-v1.1.deb
  
### Modify configuration
  sudo vim /etc/configure-camera/cam_cfg.ini

  master disposition
![telegram-cloud-photo-size-5-6156727128398345738-y](https://github.com/SilaLiu/robobus_nvidia_xavier_agx_use/assets/39790272/43276e27-9407-4fad-8244-203791048da2)

  desktop disposition
  ![image](https://github.com/SilaLiu/robobus_nvidia_xavier_agx_use/assets/39790272/c4b48a3e-48a4-4e5b-86a9-54bb0d729738)

  
### Initialize the camera service
  sudo systemctl restart jetson-cam-cfg.service
  
### View the log initialization result
  sudo journalctl -u jetson-cam-cfg.service

### Execution trigger, where ttyTHS4: software interface, 30:30 hz, 1000: high level 1000ms
  sudo tztek-jetson-tool-internal-trigger-camera /dev/ttyTHS4 30 1000

  <img width="587" alt="image" src="https://github.com/SilaLiu/robobus_nvidia_xavier_agx_use/assets/39790272/1d37b444-e151-4d92-9458-d5b5f8cc4216">

### Video output
"The first camera v1 picture"
sudo tztek-jetson-tool-camera-show-cuda /dev/video0 1920 1080



### QA Please check the user manual “GEAC91VP User Manual v1.1”
  If you are not clear, please raise issues with me, I will check regularly.
  thank you
