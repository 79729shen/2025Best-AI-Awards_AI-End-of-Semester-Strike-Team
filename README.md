# 2025智慧創新大賞_AI期末特攻隊
開發總資料請參考github上>>2025-YOLOV8無人偵查車-開發總資料.zip,
或至notion網站參考 https://www.notion.so/Ai-1c0c6410541b8040a663c21301536917

For the complete development data, please refer to the file 2025-YOLOV8無人偵查車-開發總資料.zip on GitHub, 
or check the Notion website https://fuchsia-sardine-dbc.notion.site/Ai-1c0c6410541b8040a663c21301536917

# 無人偵查車避障功能展示:
https://youtu.be/JkV125E-7Qs

# 使用說明 – ros2_dds_secure_final_package 指紋驗證系統整合包
以 ROS2、Docker、DDS 安全通訊為核心設計。

適用對象：

本系統適用於使用 Raspberry Pi 5 或 Linux 環境，結合指紋驗證與 ROS2 DDS-Security 資安傳輸場景，如門禁控制、自主設備啟動、機器人授權啟動等。

 一、安裝套件方式（適用於 Raspberry Pi）
 安裝 `.deb` 套件
```bash
sudo dpkg -i ros2-dds-secure-fingerprint_1.0.0_all.deb
```

安裝後會在：
```
/opt/ros2-dds-secure-fingerprint/
```
# 二、一鍵執行系統

指令：
```bash
cd /opt/ros2-dds-secure-fingerprint/scripts
./run_all.sh
```
會執行：

1. 指紋驗證
2. 成功則產生 CN 憑證
3. 自動產生 XML 權限
4. 啟動 ROS2 加密節點（pub/sub）
5. 上傳紀錄到 API

---

# 三、systemd 自動啟動（可選）
# 安裝服務
```bash
sudo cp ros2_dds_secure.service /etc/systemd/system/
sudo systemctl daemon-reexec
sudo systemctl enable ros2_dds_secure.service
sudo systemctl start ros2_dds_secure.service
```

---

# 四、Docker 部署（跨平台）

# 1.解壓並建置映像

```bash
unzip ros2_dds_secure_docker_context.zip
cd ros2_dds_secure_docker
docker build -t ros2-dds-secure .
```

# 2.執行容器（含 UART）

```bash
docker run --rm -it --device=/dev/ttyAMA0 ros2-dds-secure
```
---

# 五、指紋模組接線說明（Waveshare）

| 模組腳位 | 功能            | Raspberry Pi GPIO 對應 |
| ---- | ------------- | -------------------- |
| VIN  | 電源 3.3V       | Pin 1                |
| GND  | 地             | Pin 6                |
| TX   | 模組 TX → Pi RX | GPIO15 / Pin 10      |
| RX   | 模組 RX ← Pi TX | GPIO14 / Pin 8       |
| RST  | 休眠控制          | GPIO24 / Pin 18      |
| WAKE | 喚醒偵測          | GPIO23 / Pin 16      |

# 六、Log API 上傳格式（驗證紀錄）

每筆驗證結果會 POST 至 API，例如：

```json
{
  "timestamp": "2025-05-03 14:00:00",
  "user_id": "CN=User1",
  "status": "success",
  "device": "ros2-dds-secure",
  "match_code": 0
}
```

# 修改位置：
請修改 `fingerprint_uart_verify.py` 中的：

```python
API_URL = "https://your-api-server.com/api/log"
```
---

# 七、加密通訊設定（DDS-Security）

| 項目     | 設定                              |
| ------ | ------------------------------- |
| 加密演算法  | AES-GCM                         |
| 驗證演算法  | SHA-256                         |
| 憑證格式   | X.509 (RSA 2048 bit)            |
| 身份控管   | 依 CN=UserX 自動建立 permissions.xml |
| 公開話題範圍 | 限定於 `secure_topic`，僅授權節點能收發     |

---

# 八、驗證流程摘要

```plaintext
1. 指紋比對成功
↓
2. 自動建立 CN=UserX 憑證
↓
3. 自動建立/更新 XML 權限
↓
4. 啟動 ROS2 加密節點（talker 或 listener）
↓
5. 上傳驗證紀錄到 API
```

---

# 附加說明：

1. 可在 `/opt/ros2-dds-secure-fingerprint/security/` 中放置自己的 CA 憑證 (`ca.key.pem`, `ca.cert.pem`)
2. 可擴充不同 CN 對應不同話題（限制 `/camera`, `/cmd_vel` 等）
3. 若需 GPIO 控制開門、蜂鳴器、LED 可後續加入

---


