# Flying-SOP

## 1. drone SOP setting step

## Install QGroundControl

Please Follow the step in website: - https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html

Please Follow the step in second website: - https://beyonderwei.com/2019/06/12/PX4%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%EF%BC%88Ubuntu1804-QGC-Qt-Creator-%EF%BC%89/

## Install Mission Planner

lease Follow the step in website: - https://ardupilot.org/planner/docs/mission-planner-installation.html

If Ubuntu and need to add the Mono repository to your system - 

### step 1. Add the Mono repository to your system (Ubuntu 20.04 and later)

    sudo apt install ca-certificates gnupg
    sudo gpg --homedir /tmp --no-default-keyring --keyring /usr/share/keyrings/mono-official-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
    echo "deb [signed-by=/usr/share/keyrings/mono-official-archive-keyring.gpg] https://download.mono-project.com/repo/debian stable-raspbianbuster main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list
    sudo apt update

### step 2. Install Mono

    sudo apt install mono-devel

### step 3. Download Mission Planner as a zip file from website(2), unzip to directory and execute:

    mono MissionPlanner.exe

## Control the flight control panel by executing a program
