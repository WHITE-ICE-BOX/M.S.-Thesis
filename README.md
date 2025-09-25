
---

# M.S. Thesis – RTL/FPGA Architecture Design of a Real-Time Multi-Strategy Switching AI Accelerator.

**M.S. Thesis (Co-advised)** — **Po-An Hsiung** (SoC, HW/SW Co-Design) & **Chun-Hsien Huang** (Reconfigurable SoC, Edge-AI)

> **即時多策略切換 AI 加速器之 RTL/FPGA 架構設計** 提出「多策略切換決策」：在 **純硬體AI硬體切換 / 部分硬體 RP AI切換/ 全AI應用切換** 之間依模型與輸入條件選擇，整體以 **AXI 介面** 串接，採 **Verilog/HLS** 實作並於 **KV260 / PYNQ-Z2** 實機驗證。

---

> ⚠️ 目前程式碼不公開（論文進行中）；此 Repo 僅提供設計說明與實驗方法摘要。

---

## Methodology (Flow)

1. **Model**：PyTorch 訓練 → Brevitas 量化 → ONNX。
2. **HW Gen**：FINN / HLS4ML 轉 HLS IP，加上自寫 **RTL 控制/緩衝 IP**。
3. **Integration**：Vitis HLS & Vivado 封裝 → AXI4-Lite 控制 × AXI4-Stream 資料 → DMA/DDR。
4. **Reconfiguration**：單/雙 PR 區與 Overlay 並存，策略機制決定配置與 Prefetch 排程。
5. **Validation**：板上量測 Latency / 頻寬 / 功耗，對比不同策略的效益。

---

## Platforms & Tools

* **Boards**：Xilinx KV260, PYNQ-Z2
* **EDA/HW**：Vitis HLS, Vivado, PYNQ Overlay
* **DL/Quant**：verilog, PyTorch, Brevitas, ONNX, FINN/HLS4ML
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
