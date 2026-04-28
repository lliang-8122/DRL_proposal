# Multi-Objective DRL Smart Traffic Signal Control

本專案是一份以 Deep Reinforcement Learning (DRL) 為核心的智慧交通號誌控制 proposal。研究目標是使用 SUMO / SUMO-RL 建立可重現的交通數位孿生環境，並設計一個兼顧效率、公平性與穩定性的多目標自適應紅綠燈控制系統。

## Project Overview

傳統固定時制號誌無法即時回應尖峰、離峰、單向壅塞與混合車流等動態情境。一般 DRL 號誌控制方法雖然能降低平均等待時間或排隊長度，但若只追求單一效率目標，可能造成小流量方向長時間等待、燈號過度頻繁切換，或控制策略不符合人類駕駛者預期。

因此，本 proposal 將題目聚焦在：

- 使用 SUMO-RL 建立交通號誌控制環境。
- 以 DRL agent 學習動態調整紅綠燈相位。
- 將 reward 從單一效率目標擴充為多目標設計。
- 比較 fixed-time baseline 與 DRL 控制策略的表現。

## Core Idea

本研究提出「多目標自適應號誌控制」作為主要貢獻。Agent 不只降低壅塞，也需要避免特定方向被犧牲，並抑制過度頻繁的相位切換。

多目標 reward 可表示為：

```text
r_t = -alpha * Q_t - beta * F_t - gamma * S_t - delta * C_t
```

其中：

- `Q_t`：壅塞程度，例如總排隊長度或平均等待時間。
- `F_t`：公平性懲罰，例如各方向 queue length 的標準差，用來降低 starvation。
- `S_t`：系統穩定性懲罰，用來避免策略震盪。
- `C_t`：切換成本，用來抑制過度頻繁切燈造成的時間損失。

## Proposed System

預計系統流程如下：

1. 建立單一路口 SUMO traffic scenario。
2. 使用 SUMO-RL 封裝成 Gymnasium-compatible RL environment。
3. 定義 state、action、reward 與 episode termination。
4. 建立 fixed-time traffic light baseline。
5. 使用 Stable-Baselines3 訓練 DRL agent，例如 A2C、PPO 或 DQN。
6. 針對不同車流情境進行評估，例如均衡車流、尖峰車流與單向壅塞。
7. 輸出訓練曲線、模擬畫面與量化比較圖表。

## Evaluation Metrics

預計使用以下指標評估控制策略：

- Average waiting time
- Queue length
- Travel time
- Throughput
- Number of stops
- Fairness between directions
- Signal switching frequency

## Repository Contents

- `Multi-Objective_Smart_Traffic_Control.pdf`：智慧交通號誌控制 proposal 相關文件。
- `多目標強化學習智慧交通號誌控制系統(PRD).pdf`：多目標 DRL 號誌控制系統 PRD。
- `chat_1.md`：題目發想、工具選型與 SUMO-RL 實作方向討論紀錄。
- `chat_2.md`：proposal 架構、研究動機、相關研究與方法設計討論紀錄。

## Planned Implementation Structure

```text
traffic_light_project/
├─ envs/
├─ sumo_files/
├─ train.py
├─ test.py
├─ baseline_fixed.py
├─ evaluate.py
├─ models/
└─ results/
```

## Research Positioning

本專案定位為課堂 DRL project 與研究 proposal 的結合：需要能實際訓練、執行與展示，也需要有清楚的問題價值與方法貢獻。相較於常見的遊戲型 DRL 題目，智慧交通號誌控制更容易以客觀交通指標說明社會價值與技術貢獻。

目前 repository 主要保存 proposal 與設計文件；後續可依照上述架構加入 SUMO-RL 環境、訓練程式、baseline 與實驗結果。
