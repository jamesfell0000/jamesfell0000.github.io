+++
title = 'RStudio on ARM Chromebook'
date = 2022-11-13T20:50:58+11:00
+++
After a long wait, the ARM64 build of RStudio is coming out. I've managed to install it from the (unstable) daily builds. Code is below, installed on an ARM64 Chromebook running Debian 11 virtual machine (you'll need to have Linux installed first: [chromeos.dev/en/linux/setup](https://chromeos.dev/en/linux/setup)).
<!--more-->

```shell
#!/bin/bash
sudo apt install r-base
sudo apt install pandoc pandoc-citeproc apt-utils gdebi libnss3-dev libgdk-pixbuf2.0-dev libgtk-3-dev libxss-dev

#https://dailies.rstudio.com/rstudio/
wget https://s3.amazonaws.com/rstudio-ide-build/electron/focal/arm64/rstudio-2023.12.0-daily-234-arm64.deb

sudo gdebi rstudio-2023.12.0-daily-234-arm64.deb
```


![alt text](/RStudio.png)