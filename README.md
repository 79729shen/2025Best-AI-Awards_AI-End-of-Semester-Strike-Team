# 2025智慧創新大賞_AI期末特攻隊
開發總資料請參考github上>>2025-YOLOV8無人偵查車-開發總資料.zip,
或至notion網站參考 https://www.notion.so/Ai-1c0c6410541b8040a663c21301536917

For the complete development data, please refer to the file 2025-YOLOV8無人偵查車-開發總資料.zip on GitHub, 
or check the Notion website https://fuchsia-sardine-dbc.notion.site/Ai-1c0c6410541b8040a663c21301536917

# ros2_security_pi5_runtime.zip 使用說明:

以 ROS2、Docker、DDS 安全通訊為核心設計。

| 路徑 | 功能說明 |

1. launch/ >> 一鍵啟動 GUI + Token 驗證 + DDS + 指紋 SDK |
2. src/ >> 所有 ROS2 節點（PyQt GUI、GPIO 測試、UART 指紋、相機錄影） |
3. dds_security/>> DDS 安全憑證（governance.xml、permissions.xml、cert.pem 等） |
4. records/>> 登入成功後的錄影影片儲存目錄（會自動建立） |

# 解壓並進入專案資料夾

unzip ros2_security_pi5_runtime.zip
cd ros2_security_ws

# 安裝相依套件（第一次執行）

sudo apt update && sudo apt install python3-pyqt5 python3-opencv python3-colcon-common-extensions python3-pip -y
pip3 install pyserial

# 編譯並啟動系統

colcon build
source install/setup.bash
ros2 launch launch secure_layers.launch.py

 完成後會自動打開 GUI、啟動指紋模組、Token 驗證、GPIO 測試、相機錄影功能。
