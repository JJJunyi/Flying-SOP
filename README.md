# Flying-SOP (Programming)

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

### First step:

✔ Create a Virtual Environment (Avoid)

    python3 -m venv myenv

    source myenv/bin/activate

    pip install PyYAML mavproxy

✔ Then install MAVProxy 

    sudo usermod -a -G dialout chun (Figure Permission denied)

    mavproxy.py --master=/dev/ttyACM1 --baudrate 57600

<img width="1037" height="270" alt="Screenshot from 2025-08-01 16-36-29" src="https://github.com/user-attachments/assets/4b577e1b-c7ef-4d77-95de-1f6efa70dc91" />

### Second step:

✔ Check Status:

    status: 顯示無人機的整體狀態，包括電池電量、GPS 狀態、飛行模式等。

    param show: 顯示所有參數的列表。你也可以加上參數名稱來查看特定參數，例如 param show arming_check。

✔ Unlock and lock

    arm throttle: 解鎖無人機，讓馬達可以轉動。

    disarm: 鎖定無人機，馬達會立即停止轉動。

✔ Flying Mode:

    ode: 顯示目前可用的所有飛行模式。

    mode GUIDED: 切換到 GUIDED 模式。

    mode LOITER: 切換到 LOITER 模式。

    mode RTL: 切換到 RTL (Return to Launch) 模式。

✔ Flying Control:

這些指令讓你可以在 GUIDED 模式下進行簡單的飛行控制。

    takeoff: 讓無人機起飛。

    goto 緯度 經度 高度: 讓無人機飛到指定的 GPS 座標。

    land: 讓無人機在當前位置降落。

### Execution Results

✔ 後續再更新

<img width="1126" height="701" alt="Screenshot from 2025-08-07 15-45-22" src="https://github.com/user-attachments/assets/d02cc3b8-ba5b-43f4-b29c-bac4fcbb331c" />

### Like QGroundControl 能設定飛行任務，主要會使用 MISSION 模式。在 MAVProxy 中，可以透過一系列指令來上傳、啟動和管理一個完整的飛行任務。

✔ Step Detail

    1. 切換到 AUTO 或 AUTO.LOITER 模式: 這是執行自動任務的模式。
    
    2. 建立任務檔案: 任務檔案通常是 .txt 格式，包含了起飛、航點、返航等指令。
    
    3. 上傳任務檔案: 將任務檔案上傳到飛控板。
    
    4. 啟動任務: 執行指令讓無人機開始執行任務。

#### 1. 建立任務檔案 (mission.txt)
   
先手動建立一個文字檔案 mission.txt，其中包含你的飛行任務指令。每個航點或指令都佔據一行，格式如下：

    序列號 指令編號 參數1 參數2 參數3 參數4 緯度 經度 高度 航點延遲時間

✔ 列一個簡單的任務範例，包含起飛、飛到兩個航點，然後返航。

    0   16   0   0   0   0   0   0   0   0   0   # 任務開始標誌
    
    1   22   0   0   0   0   0   0   0   10.0   # Takeoff 起飛到 10m
    
    2   16   0   0   0   0   0   0   25.074320   121.439530   10.0  # Waypoint 1
    
    3   16   0   0   0   0   0   0   25.074450   121.439600   10.0  # Waypoint 2
    
    4   20   0   0   0   0   0   0   0   0   0   # Return to Launch (RTL)

##### WP_ 指令代碼說明：(這個部份再去細看)

    16 (NAV_WAYPOINT): 飛行到指定的航點。

    22 (MAV_CMD_NAV_TAKEOFF): 垂直起飛到指定高度。

    20 (MAV_CMD_NAV_RETURN_TO_LAUNCH): 返回起飛點並降落。

#### 2. 上傳任務檔案

✔ MAVProxy 的控制台 (MAV>) 中，使用 mission 指令來上傳你剛才建立的任務檔案。

    MAV> mission load mission.txt

IF 上傳成功，MAVProxy 會顯示類似 Loaded 5 waypoints from mission.txt 的訊息。或使用 mission list 來確認任務是否成功上傳。
    
    MAV> mission list

#### 3. 切換飛行模式並啟動任務

✔ 當準備好讓無人機執行任務時，需要將其切換到 AUTO 模式。

##### 1. 解鎖無人機: 確保無人機已解鎖。

    MAV> arm throttle

##### 2. 切換到 AUTO 模式: 這會自動開始執行任務。

    MAV> mode AUTO

✔ 當你切換到 AUTO 模式後，無人機就會開始依序執行你設定的任務，從起飛點開始，依序飛到每個航點，最後返回起飛點。

✔ Commands (MAV_CMD) resourse: https://mavlink.io/en/messages/common.html#mav_commands

✔ 其他重要的 MAVLink 任務指令代碼

    MAV_CMD_NAV_LOITER_UNLIM (17): Loiter around this waypoint an unlimited amount of time (param5, param6, param7: 經緯度，高度)

    MAV_CMD_NAV_LOITER_TURNS (18): 

    MAV_CMD_NAV_RETURN_TO_LAUNCH (20): Return to launch location

    MAV_CMD_NAV_LAND (21): Land at location.

    MAV_CMD_NAV_TAKEOFF (22): vertical takeoff from ground / hand. Vehicles that support multiple takeoff modes (e.g. VTOL quadplane).(param7: 起飛高度 (m))

    MAV_CMD_DO_CHANGE_SPEED (178): Change speed and/or throttle set points. The value persists until it is overridden or there is a mode change (param1: 速度類型<br/>param2: 速度 (m/s))
