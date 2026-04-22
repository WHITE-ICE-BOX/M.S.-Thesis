# M.S. Thesis — 超低位元參數高效率遷移式學習之 AI 硬體加速器架構設計與實作

**Master's Thesis of Po-Chun Huang (黃柏鈞)**  
National Chung Cheng University, Dept. of Computer Science and Information Engineering  
Co-advised by **Prof. Pao-Ann Hsiung** (SoC / HW-SW Co-Design) and **Prof. Chun-Hsian Huang** (Reconfigurable SoC / Edge-AI)

> 🇹🇼 **超低位元參數高效率遷移式學習之 AI 硬體加速器架構設計與實作**  
> 🇬🇧 **Hardware Architecture Design and Implementation of Parameter-Efficient Transfer Learning for Ultra-Low-Bit AI Accelerators**

> ⚠️ Source code is currently private (thesis work in progress).  
> This repository provides only the design overview and experimental methodology.

---

## Motivation

Edge FPGAs 現行多任務 AI 推論方案三大類 — Partial Reconfiguration (PCAP 97–114 ms, ZyCAP 5.2 ms)、FixyNN 固定特徵萃取器、NeuroAdaptiveNet 片上變體 — 皆無法同時滿足 **次毫秒切換** 與 **特徵萃取器層級任務適應**。本論文於 **1W1A (1-bit weight / 1-bit activation)** 超低位元 CNN 主幹上，透過 PEFT (Parameter-Efficient Transfer Learning) + 客製 RTL Adapter IP 同時滿足上述兩項需求。

---

## Key Contributions

1. **超低位元遷移準確率突破**：Multi-Adapter Scaling (m2–m4) + Residual Correction (RC) 零硬體代價補償  
   ‧ SVHN：m1 72.12% → m3 78.97% (+6.85 pp)  
   ‧ FashionMNIST：m1 78.76% → m3 81.13% (+2.37 pp)

2. **RTL 級低功耗 Adapter IP**：純 distributed RAM + 組合邏輯，**Zero DSP**；5 層異質 PE/SIMD/HIDDEN 參數化 adapter (MVAU1–MVAU5, Conv2–Conv6) 合計 **25,593 LUT (~48% XC7Z020)**、**zero BRAM overhead** (down/up-projection 全部 distributed RAM ROM 打包)。

3. **Adapter–MVAU 整合 (Super Wrapper)**：Decoupled Thresholding、預計算 Contribution LUT、Simple FIFO cycle-accurate 同步；Stream Adder + Threshold 以 Q8 定點比較融合 adapter 貢獻，支援 γ>0 / γ<0 BatchNorm 雙極性 (MVAU5 bit-reflection trick)。

4. **Runtime 多任務切換基礎設施**：自研 AXI-Lite Configuration Hub，runtime 重新編程閾值、分類器權重、adapter 控制暫存器，**單一 bitstream、sub-1 ms task switching、無 FPGA fabric reconfiguration**。

5. **PYNQ-Z2 實作與三層功能驗證**：  
   ‧ Per-MVAU RTL Sim：**43,520 筆測試向量、100% pass**  
   ‧ Top-level Streaming Dataflow Sim：對齊 PyTorch golden  
   ‧ 板上 End-to-End：**26,000 張 SVHN 71.81% 準確率 (vs. PyTorch 72.12%)、1,928 img/s 吞吐、0.52 ms/img 延遲**

6. **End-to-End 設計方法論**：PyTorch / Brevitas QAT → FINN backbone IP → 自訂 RTL 整合 → IP packaging → Block Design stitch → Bitstream → 板上部署；並以 `rebuild_bitstream.tcl` 解決 Vivado OOC synthesis cache 陷阱。

7. **自動化 Adapter 架構搜尋**：$2^5 = 32$ 組 ablation 架構自動窮舉，透過歸零 scaling factor 免重訓即找最優子集。

---

## Methodology

| Stage | Description |
|-------|-------------|
| **Model** | PyTorch QAT → Brevitas 1W1A 量化 → ONNX export |
| **Backbone** | FINN dataflow compile → MVAU per-layer RTL |
| **Custom HW** | 自研 RTL：Adapter IP (Down/Up-Projection)、Stream Adder + Threshold、Simple FIFO、AXI-Lite cfg_hub |
| **Integration** | Super Wrapper 封裝 MVAU + Adapter → Vivado Block Design stitch → Bitstream |
| **Validation** | Per-MVAU RTL Sim + Top-level Sim + PYNQ-Z2 板上端到端量測 |

---

## Platforms & Tools

- **Target Board:** PYNQ-Z2 (Xilinx Zynq-7020 xc7z020clg400-1)
- **EDA:** Vivado 2022.2, Vitis HLS, PYNQ Overlay, xsim
- **DL / Quantization:** PyTorch, Brevitas, ONNX, FINN
- **HDL:** Verilog RTL
- **Bus / Interface:** AXI4-Lite (control), AXI4-Stream (data), DMA / DDR

---

## Keywords

Parameter-Efficient Transfer Learning (PEFT) · Conv-Adapter · Ultra-Low-Bit Quantization ·  
FINN · MVAU · FPGA · Multi-Adapter · Residual Correction · Streaming Dataflow · PYNQ-Z2

---

## Advisors

- **Prof. Pao-Ann Hsiung (熊博安)** — National Chung Cheng University CSIE  
  Research: SoC & HW-SW Co-Design, SoC Verification, Real-Time Embedded Systems

- **Prof. Chun-Hsian Huang (黃駿賢)** — National Changhua University of Education EE  
  Research: Reconfigurable Hardware Architecture, Hardware Accelerator IP Design, High-Level Synthesis (HLS)

> 本論文由兩位教授共同指導，同時兼顧 **可重組系統設計** 與 **可部署之實務工程**。

---

## Contact

**Po-Chun Huang (黃柏鈞 / Barkie)** — Zhubei, Hsinchu, Taiwan  
📧 [barkie.huang@gmail.com](mailto:barkie.huang@gmail.com)  
🔗 GitHub: [WHITE-ICE-BOX](https://github.com/WHITE-ICE-BOX)
