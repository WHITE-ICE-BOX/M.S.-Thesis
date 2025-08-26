
---

# M.S. Thesis – Reconfigurable Deep Learning Accelerator for Digital IC Design: Multi-Strategy Switching and Low-Power Architecture.

**M.S. Thesis (Co-advised)** — **Po-An Hsiung** (SoC, HW/SW Co-Design) & **Chun-Hsien Huang** (Reconfigurable SoC, Edge-AI)

> 以「數位 IC 設計思維」打造可重組的深度學習加速器，聚焦 **PR 切換成本** 與 **低功耗**。提出「多策略切換決策」：在 **Overlay / 單 RP / 雙 RP + Prefetch** 之間依模型與輸入條件動態選擇，整體以 **AXI 介面** 串接，採 **Verilog/HLS** 實作並於 **KV260 / PYNQ-Z2** 實機驗證。

---

## Why this project fits Digital IC Design roles

* **RTL 規劃與模組化**：控制器、緩衝器、計算核心以 **Verilog/HLS IP** 分層實作，清楚的時序/介面規格。
* **AXI 系統整合**：AXI4-Lite 控制 + AXI4-Stream/HP 資料通道，對接 PS/PL 與外部記憶體。
* **功耗/效能取捨**：早停(early-exit)與選擇性推論(selective inference)結合 PR 決策，降低重組與計算能耗。
* **實機量測與驗證方法論**：建立 Throughput / Latency / Power 的量測流程（板上計時器、DMA 帶寬、Xilinx Power Estimator）。
* **可映射到 ASIC 能力**：管線化、記憶體階層、時序收斂與時脈域劃分等設計觀念，可延伸到 ASIC Flow（SYN/STA/PD）。

---

## Architecture at a Glance

```
         +------------------------------+
         |  Strategy Selector (FSM)     |  <-- 根據模型層/輸入條件選 Overlay / 單RP / 雙RP+Prefetch
         +------------------------------+
            |           |            |
   +--------+----+  +---+-------+  +--+--------------------+
   | Overlay IP |  | Single RP  |  | Dual RP + Prefetch   |
   |  (PYNQ)    |  | (PR Region)|  | (PR0/PR1 overlap)    |
   +-----+------+  +-----+------+  +-----------+----------+
         |               |                        |
  +------+---------------+------------------------+-------+
  |            AXI Interconnect / DMA / BRAM/DDR          |
  +-------------------------------------------------------+
```

---

## Key Contributions

1. **Multi-Strategy Switching**：在 **Overlay / 單 RP / 雙 RP + Prefetch** 之間做靜/動態切換，降低 **bitstream 下載/介面隔離** 的開銷。
2. **Early-Exit / Selective Inference**：針對輸入難度決定是否提前停止或跳過部分路徑，兼顧準確率與能耗。
3. **AXI-Compliant IPs**：計算核心、權重/特徵圖緩衝、控制暫存等皆為 **AXI Ready**，便於 SoC 整合與重用。
4. **實機驗證**：在 **KV260 / PYNQ-Z2** 完成端到端 Demo（訓練→量化→匯出→HLS/RTL→整合→量測）。

> ⚠️ 目前程式碼不公開（論文進行中）；此 Repo 僅提供設計說明與實驗方法摘要。

---

## Methodology (Flow)

1. **Model**：PyTorch 訓練 → Brevitas 量化 → ONNX。
2. **HW Gen**：FINN / HLS4ML 轉 HLS，加上自寫 **RTL 控制/緩衝 IP**。
3. **Integration**：Vitis HLS & Vivado 封裝 → AXI4-Lite 控制 × AXI4-Stream 資料 → DMA/DDR。
4. **Reconfiguration**：單/雙 PR 區與 Overlay 並存，策略機制決定配置與 Prefetch 排程。
5. **Validation**：板上量測 Latency / 帶寬 / 功耗，對比不同策略的效益。

---

## Platforms & Tools

* **Boards**：Xilinx KV260, PYNQ-Z2
* **EDA/HW**：Vitis HLS, Vivado, PYNQ Overlay
* **DL/Quant**：PyTorch, Brevitas, ONNX, FINN/HLS4ML
* **Bus/IF**：AXI4-Lite / AXI4-Stream / DMA

---

## Advisors（雙指導教授）

* **Prof. Po-An Hsiung** — SoC & HW/SW Co-Design
* **Prof. Chun-Hsien Huang** — Reconfigurable SoC & Edge-AI

> 本論文由兩位教授共同指導；研究同時兼顧 **可重組系統設計** 與 **可部署之實務工程**。

---

## Contact

Barkie (Po-Chun) Huang｜Zhubei, Hsinchu
Email: **[barkie.huang@gmail.com](mailto:barkie.huang@gmail.com)** ｜ GitHub: **WHITE-ICE-BOX**

---
