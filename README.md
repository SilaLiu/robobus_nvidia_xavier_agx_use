## Robobus_Camera_Use
### You need to install the camera-related configuration package first
  tztek-cam-app_2.0.0_arm64 .deb                        https://drive.google.com/file/d/1VMStmcU8CqG8Z3s5tzHAZI6SKpdOQdei/view?usp=drive_link
  tztek-jetson-service-camera-config-510v-v2.7.deb      https://drive.google.com/file/d/1Uvw3luBPjzQcYSYDCBiPwiE4mRPPmlK1/view?usp=drive_link
  tztek-jetson-tool-camera-show-v1.0 .deb               https://drive.google.com/file/d/1tfz0euIGGZex7b8H-FqikjuLivq_lZ3x/view?usp=drive_link
  tztek-jetson-tool-internal-trigger-camera-v1.1.deb    https://drive.google.com/file/d/1DbjF7zrKQX9ddb6Uu8_oAsT-Gdgsgbmu/view?usp=drive_link

  sudo dpkg -i tztek-jetson-service-camera-config-510vp-v2.7.deb
  sudo dpkg -i tztek-cam-app_2.0.0_arm64.deb
  sudo dpkg -i tztek-jetson-tool-camera-show-v1.0.deb
  sudo dpkg -i tztek-jetson-tool-internal-trigger-camera-v1.1.deb
  
### Modify configuration
  sudo vim /etc/configure-camera/cam_cfg.ini
  
### Initialize the camera service
  sudo systemctl restart jetson-cam-cfg.service
  
### View the log initialization result
  sudo journalctl -u jetson-cam-cfg.service
