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

---

## 🛠️ Bill of Materials & Cost Estimation / 硬體元件清單與成本估算

以下為整套系統的完整採購清單與預估成本（以台灣電子材料行與網購常見單價估算，不含運費）：

### 1. Core Electronics / 核心電子元件（雙機共用與控制端）
* **ESP32-C3 SuperMini 開發板（附排針） × 2 片**
  * 單價約 NT$ 65 ～ 85 / 片，共約 **NT$ 130 ～ 170**
  * *備註：兩片規格完全相同，一片作為走廊偵測端、一片作為電腦後方接收端。*
* **單芯漆包線 或 細單芯銅線（約 0.4mm ～ 0.6mm） × 1 小捲或剪裁段**
  * 單價約 **NT$ 10 ～ 25**
  * *備註：剪兩段各 **3.1 公分（$\lambda/4$）** 焊接於陶瓷天線末端作外接天線，若有舊網路線或單芯線可直接剝皮取用。*

### 2. Corridor Sensor Unit / 走廊偵測端專用零件
* **AM312 迷你人體紅外線感測器模組 × 1 顆**
  * 單價約 **NT$ 25 ～ 40**
  * *備註：務必選購 AM312 迷你版（待機電流極低且抗干擾佳），勿買體積較大的 HC-SR501。*
* **3 節 4 號（AAA）薄型電池盒（附開關與紅黑導線） × 1 個**
  * 單價約 **NT$ 15 ～ 25**
  * *備註：串聯輸出 4.5V，務必選「3 節版」，切勿買 4 節版（電壓超過 6V 會燒毀板載穩壓晶片）。*
* **4 號（AAA）鹼性電池 × 3 顆**
  * 單價約 **NT$ 45 ～ 60**（建議使用大品牌鹼性電池，防止長期使用過放漏液）。
* **小型塑膠外殼盒（收納偵測端主板與電池盒用，約 6×4×2.5 cm） × 1 個**
  * 單價約 **NT$ 15 ～ 30**（亦可使用現成小藥盒或塑膠收納盒替代）。

### 3. Desk Partition Indicator Unit / 辦公桌接收端專用零件
* **5mm 霧面（散光型）共陰極紅綠雙色 LED（3 支腳位） × 1 顆**
  * 單價約 **NT$ 3 ～ 5**（建議多買 2 顆備用，合計約 **NT$ 10**；務必指定「霧面散光」與「共陰極」）。
* **220Ω（歐姆）1/4W 色碼電阻 × 2 顆**
  * 單價約 NT$ 1 / 顆，買一小包（10 顆裝）約 **NT$ 5 ～ 10**。
* **迷你軟木塞玻璃瓶（瓶身直徑約 2～2.5 cm、高約 4～5 cm） × 1 個**
  * 單價約 **NT$ 10 ～ 20**（手作材料行、生活百貨或網拍皆有售）。
* **母對母杜邦線（長度建議 60cm ～ 100cm，或 3 芯細軟線） × 3 條**
  * 單價約 **NT$ 15 ～ 30**（用於從屏風頂端玻璃瓶垂降連接至電腦後方的主機板）。
* **USB Type-A 轉 Type-C 電源傳輸線（約 1 公尺） × 1 條**
  * 單價約 **NT$ 40 ～ 80**（必須具備「數據傳輸功能」，燒錄韌體時才能直接插電腦使用）。

### 4. Consumables & Mounting / 耗材與固定配件
* **熱縮套管（直徑約 2mm ～ 3mm，包裹 LED 焊點與杜邦線接頭防短路） × 1 小包**
  * 單價約 **NT$ 10 ～ 20**
* **3M 無痕雙面膠 或 泡棉膠（固定屏風玻璃瓶底座、螢幕後方主板與走廊感應盒） × 1 捲**
  * 單價約 **NT$ 35 ～ 50**

### 💰 Total Estimated Cost / 總成本估算
* **All-New Purchase (全部全新採購，含電池、線材、外殼與耗材):** 約 **NT$ 365 ～ 565**
* **Core Components Only (自備部分現成線材與耗材):** 純電子核心材料費僅約 **NT$ 245 ～ 345**
