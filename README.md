
---

# M.S. Thesis – RTL/FPGA Architecture Design of a Real-Time Multi-Strategy Switching AI Accelerator.

**M.S. Thesis (Co-advised)** — **Po-An Hsiung** (SoC, HW/SW Co-Design) & **Chun-Hsien Huang** (Reconfigurable SoC, Edge-AI)

> **支持即時多任務切換之 AI 加速器架構設計**

---

> ⚠️ 目前程式碼不公開（論文進行中）；此 Repo 僅提供設計說明與實驗方法摘要。

---

## Methodology (Flow)

1. **Model**：PyTorch 訓練 → Brevitas 量化 → ONNX。
2. **HW Gen**：**RTL ai網路架構 ip、記憶體 ip、fifo緩衝 ip、adapter ip **。
3. **Integration**：Vitis HLS & Vivado 封裝 → AXI4-Lite 控制 × AXI4-Stream 資料 → DMA/DDR。
4. **Validation**：fpga 板上量測 Latency / 頻寬 / 功耗 / 資源使用率，對比不同策略的效益。

---

## Platforms & Tools

* **Boards**：Xilinx KV260, PYNQ-Z2
* **EDA/HW**：Vitis HLS, Vivado, PYNQ Overlay
* **DL/Quant**：verilog, PyTorch, Brevitas, ONNX
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
