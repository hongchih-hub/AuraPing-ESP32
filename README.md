# AuraPing (LumiSense) — Silent Office Corridor Indicator
**光語・無聲無線走廊動態指示器**

> A silent, wireless breathing-light indicator that alerts your office team when someone enters the hallway.  
> 一款無聲的無線呼吸燈指示器，當有人進入走廊時即時提醒辦公團隊。

---

## 📖 Overview / 產品簡介

**AuraPing** is a simple, wireless visitor alert system designed for open offices. Instead of using loud buzzers or disruptive notifications, it relies on soft, ambient breathing lights to let your team know when someone enters the corridor—keeping the workspace quiet, focused, and distraction-free.

**AuraPing（光語）** 是一套專為開放式辦公環境設計的無線訪客預警與團隊共享指示系統。它捨棄傳統刺耳的蜂鳴器或干擾性的跳窗通知，透過柔和的環境呼吸燈光影變化，在人員進入走廊的第一時間無聲提醒團隊成員，讓大家在保持高度專注的同時，也能從容應對外部來訪。

### ✨ Key Features / 核心特色

* **Quiet & Subtle (無聲柔光不擾人):** Soft PWM breathing lights catch your eye via peripheral vision without annoying coworkers or disrupting focus.
* **Team-Shared View (360 度團隊共享視野):** A mini frosted glass-bottle LED diffuser sits atop the office partition, making it visible to colleagues from all angles.
* **Zero Company Wi-Fi Needed (免連公司內網):** Uses direct point-to-point **ESP-NOW** with Long-Range (LR) mode and 3.1cm external copper antennas for strong wall penetration without touching corporate networks.
* **Smart Auto-Maintenance (全自動智慧維運):** Features a 1-hour silent heartbeat check and instant auto-reconnection after battery replacements or PC restarts—no pairing buttons required.

---

## ⚙️ System Architecture & Light Status / 系統架構與燈號說明

### 1. Hardware Architecture / 雙機分離式硬體架構
* **Corridor Sensor Node (走廊感應端 — 電池供電):**  
  Powered by 3x AAA alkaline batteries (4.5V). Uses an ultra-low-power **AM312 PIR sensor** paired with an **ESP32-C3 SuperMini** in deep sleep mode, waking up instantaneously upon motion or every 1 hour for a silent health check.
* **Desk Partition Indicator (辦公區接收端 — 分離式燈頭設計):**  
  * **Light Head (屏風頂端燈頭):** A mini cork-stoppered glass bottle housing a single **5mm diffused common-cathode Red/Green LED**, mounted on top of the office partition for 360-degree visibility.
  * **Main Controller (電腦後方主機):** Connected via 3 DuPont extension wires routed down the partition edge to an **ESP32-C3 SuperMini** tucked behind the monitor, powered directly via USB Type-C.

### 2. Light Status Guide / 燈號狀態判讀

| Light Pattern (燈號表現) | System Status (系統狀態) | Description & Action (說明與建議動作) |
| :--- | :--- | :--- |
| **Slow Green Breathing**<br>綠燈・柔和慢呼吸 (3.0s cycle) | **Normal Standby**<br>系統正常運作中 | Sensor battery is healthy and wireless link is active. Hallway is clear.<br>感應器電力充足、連線穩定，目前走廊無人靠近，可安心工作。 |
| **Fast Red Breathing**<br>紅燈・急促呼吸 (0.7s cycle) | **Corridor Motion Alert**<br>走廊有人靠近 | Motion detected in the hallway. Alerts the team for **5 seconds** before smoothly returning to standby.<br>偵測到人員進入走廊，提醒持續約 5 秒後自動恢復為綠燈待機。 |
| **Slow Yellow Breathing**<br>黃燈・微弱慢呼吸 (3.5s cycle) | **Low Battery / Offline**<br>感應器沒電／失聯 | No signal (`ALERT` or hourly `PING`) received for over **2.5 hours**. Replace the 3x AAA batteries in the hallway unit; it will auto-reconnect immediately.<br>超過 2.5 小時未收到走廊端訊號，代表電池已耗盡。更換 3 顆新 4 號電池後將自動變回綠燈。 |


