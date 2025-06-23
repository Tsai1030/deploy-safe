<!--
title: RAG QA System for Air Pollution (DuckDNS + Nginx Version)
description: A LangChain + Chroma + Ollama powered RAG chatbot system deployed with DuckDNS and Nginx, focused on air pollution and public health.
keywords: RAG, LangChain, Ollama, DuckDNS, Nginx, chatbot, air pollution, QA system, react, fastapi, chromadb
-->

# 🌱 RAG 空氣污染問答系統（DuckDNS + Nginx 部署版本）

本專案為一套結合語意檢索（Retrieval-Augmented Generation, RAG）與生成式 AI 的智慧型問答系統，針對空氣污染政策、健康影響與環境法規等主題，提供即時、精準且具可查證性的回應。

本版本為原始專案的強化版，**新增整合 DuckDNS 免費網域與 Nginx 靜態頁面 + 反向代理部署機制**，可於區網外部透過自訂網址（如 `http://kmu-rag.duckdns.org`）進行存取。

---

## 🔗 線上測試網址

👉 [http://kmu-rag.duckdns.org/](http://kmu-rag.duckdns.org/)

---

## 🔧 本地端快速啟動說明

### 📦 建立 Conda 環境

```bash
conda env create -f environment.yml
conda activate test2
```

---

### 🧩 前端啟動（React + Vite）

```bash
cd frontend
npm install
npm run dev
```

如為部署模式，請執行：

```bash
npm run build
```

並確認 `dist/` 資料夾內容已更新。

---

### ⚙️ 後端啟動（FastAPI + LangChain + Ollama）

```bash
cd backend
conda activate test2
uvicorn main:app --reload
```

後端 Swagger 測試網址：  
[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

---

### 🌐 外部部署（Nginx + DuckDNS）

- 將前端打包後產生的 `dist` 目錄掛載至 Nginx `root` 設定目錄
- Nginx 設定中 `location /api` 需反向代理至後端 FastAPI (`http://127.0.0.1:8000`)
- 執行 `duckdns_update.bat` 以自動更新 DuckDNS 的 IP
- 確保防火牆開放 80 及 FastAPI 對應 port

---

### 前端每次更新 要做的步驟
```bash
npm run build

```
```bash
cd C:\nginx
nginx -s reload
```

### 停止再啟動
```bash
nginx -s stop
start nginx
```

## 📁 專案結構（新版本）

```
deploy-safe/
├── backend/                 # FastAPI 後端
├── frontend/                # React + Vite 前端原始碼
├── dist/                    # 打包後的靜態前端檔案（供 Nginx 部署）
├── duckdns_update.bat       # DuckDNS 網域 IP 更新批次檔
├── environment.yml          # Conda 環境設定
├── nginx.conf               # Nginx 配置檔（若有備份）
├── package.json             # 前端專案設定
├── README.md                # 本說明文件
```

---

## 🧠 技術架構總覽

| 元件             | 說明                                                                 |
|------------------|----------------------------------------------------------------------|
| **RAG 架構**      | 使用 LangChain 整合 Chroma 向量資料庫與本地 LLM 模型（如 Gemma、Qwen）     |
| **語意檢索**       | 採用 BGE-m3 或 all-mpnet-base-v2 向量模型進行文件嵌入與查詢                        |
| **回答生成**       | 結合查詢結果與自訂 Prompt，透過本地部署 LLM 生成答案                               |
| **後端 API**      | 使用 FastAPI 撰寫 `/api/predict` 等推論服務，支援 JSON 格式資料交換                  |
| **靜態網頁與反向代理** | 使用 Nginx 提供前端 `dist` 靜態檔案並轉發 `/api` 請求至後端 FastAPI API                  |
| **前端介面**       | 使用 Vite 建立的 React 前端，支援提問、回覆呈現與格式化 Markdown                        |
| **模型部署**       | 搭配 Ollama 進行本地 LLM 部署，可支援 Gemma 3B、Qwen 14B 等模型                       |
| **免費網域串接**     | 使用 DuckDNS 建立自訂網域（如 `kmu-rag.duckdns.org`），供外部使用者連線                   |
| **評估機制**       | 可搭配 RAGAS 框架進行忠實度、語意相關性等指標的自動評估                                   |
| **版本控管**       | 使用 Git 控制前後端與部署環境變更，搭配 `.gitignore` 排除暫存與模型檔案                     |

---

## 🧾 資料處理方式說明

- **長文資料**：以標題為單位切割段落，再以語意斷句處理，每段設計 chunk_size 與 overlap 切片後進行向量化。
- **QA 文件**：完整保留問答對內容，不進行切段，確保回答語境與邏輯一致性。
- 所有文件轉換為結構化 JSON 格式，欄位包含 `title`、`content`、`doc_id`、`source` 等欄位。

> 🔧 設定上，QA 的文本不進行切割，僅針對長文本進行 chunk 處理。

---

## 🖼️ 系統展示畫面

![RAG 系統展示圖](https://github.com/Tsai1030/rag-air-pollution/blob/main/frontend/public/images/%E5%B1%95%E7%A4%BA%E7%85%A7%E7%89%87.png?raw=true)

---

## 📬 聯絡方式

**蔡承紘 Cheng-Hung, Tsai**  
📧 Email : pijh102511@gmail.com  
📍 高雄醫學大學 醫療資訊研究所（AI組）碩士生

---

> 若你覺得此專案對你有幫助，歡迎 Star 🌟 或提出 Issue / Pull Request 改進 🙌
