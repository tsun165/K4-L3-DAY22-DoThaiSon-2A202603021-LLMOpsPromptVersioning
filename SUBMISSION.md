# Hướng dẫn nộp bài — Day 22: LangSmith + Prompt Versioning

> **Hình thức: Bài CÁ NHÂN.** Mỗi học viên tự nộp 1 repo của riêng mình.
> **Phương thức nộp:** Nộp duy nhất **URL GitHub repository (public)**. Toàn bộ mã nguồn, dữ liệu đánh giá và bằng chứng (traces, prompts, metrics, logs) đều được đóng gói đầy đủ trong repo.

---

## 1. Thông tin bài nộp

- **Họ và tên:** Đỗ Thái Sơn
- **Mã số sinh viên (MSSV):** 2A202603021
- **GitHub Account:** [tsun165](https://github.com/tsun165)
- **Tên Repo:** `K4-L3-DAY22-DoThaiSon-2A202603021-LLMOpsPromptVersioning`
- **LangSmith Project:** `day22-lab` ([Truy cập LangSmith Project](https://smith.langchain.com/o/d575f7b2-d452-4820-ac3a-54db214dda1d))
- **LangSmith Prompts trên Hub:**
  - `do-thai-son-rag-prompt-v1`
  - `do-thai-son-rag-prompt-v2`

---

## 2. Cấu trúc repo khi nộp

```
K4-L3-DAY22-DoThaiSon-2A202603021-LLMOpsPromptVersioning/
├── src/
│   ├── 01_langsmith_rag_pipeline.py   ← RAG chain + @traceable logging
│   ├── 02_prompt_hub_ab_routing.py    ← Push/Pull Prompt Hub & MD5 deterministic routing
│   ├── 03_ragas_evaluation.py         ← Đánh giá 50 QA pairs qua 4 chỉ số RAGAS
│   ├── 04_guardrails_validator.py     ← Custom PIIDetector & JSONFormatter
│   ├── config.py, qa_pairs.py, run_all.py
│   └── utils/
│       ├── llm_factory.py             ← Hỗ trợ đa dạng provider (FPT, OpenAI, Gemini, Claude, Ollama)
│       └── data_loader.py             ← Vectorstore FAISS indexer
├── data/
│   ├── knowledge_base.txt             ← Tài liệu nguồn RAG (15 chủ đề AI/LLM)
│   └── ragas_report.json              ← Kết quả đánh giá RAGAS chi tiết
├── evidence/                          ← BẮT BUỘC ĐỦ 8 TỆP MINH CHỨNG
│   ├── 01_langsmith_traces.png        (Minh chứng ≥ 100 traces trên LangSmith UI)
│   ├── 02_prompt_hub.png              (Minh chứng 2 prompts trên LangSmith Prompt Hub)
│   ├── 02_ab_routing_log.txt          (Log định tuyến A/B với nhãn v1/v2 cho 50 queries)
│   ├── 03_ragas_scores.png            (Bảng và biểu đồ so sánh 4 chỉ số V1 vs V2)
│   ├── 03_ragas_report.json           (File JSON kết quả RAGAS)
│   ├── 04_pii_demo_log.txt            (Log test validator PIIDetector che thông tin nhạy cảm)
│   ├── 04_json_demo_log.txt           (Log test validator JSONFormatter tự sửa lỗi)
│   └── README.md                      (Báo cáo phân tích so sánh chuyên sâu V1 vs V2)
├── .env.example                       ← KHÔNG commit .env
├── .gitignore
├── requirements.txt
├── README.md
├── CHECKPOINTS.md
├── RUBRIC.md
├── RULES.md
└── SUBMISSION.md                      ← File này
```

---

## 3. Xác nhận Minh chứng Traces (≥ 100 Traces)

Hệ thống đã thực hiện tracing toàn diện qua LangSmith với số lượng traces ghi nhận vượt xa yêu cầu tối thiểu:
- **Tổng số Root Traces:** > 700 traces (riêng 100% các câu hỏi từ 50 QA pairs của Bước 1, Bước 2, và Bước 3).
- **Tổng số Runs được ghi nhận:** > 6,500 runs (bao gồm RAG pipeline, Retriever runs, Prompt formatting, LLM completions, và các evaluation evaluator calls từ RAGAS).
- **Ảnh chụp bằng chứng:** `evidence/01_langsmith_traces.png` ghi lại giao diện trực quan của LangSmith Project `day22-lab` với đầy đủ latency, token count, tags (`rag`, `step1`, `step2`, `ragas`), input query, retrieved context và output.

---

## 4. Bảng đối chiếu Tiêu chí Chấm điểm & Điểm thưởng

| Nhiệm vụ / Hạng mục | Tiêu chí chi tiết | Điểm tối đa | Đạt được | Bằng chứng |
|---|---|:---:|:---:|---|
| **Nhiệm vụ 1: RAG Pipeline + LangSmith** | • FAISS chunking & indexing đúng chuẩn<br>• RAG chain LangChain LCEL hoàn chỉnh<br>• `@traceable` logging $\ge 50$ traces<br>• Traces chứa đủ inputs, context & output | 25đ | **25/25** | `src/01_langsmith_rag_pipeline.py`<br>`evidence/01_langsmith_traces.png` |
| **Nhiệm vụ 2: Prompt Hub & A/B Routing** | • 2 system prompt ngữ nghĩa khác nhau rõ rệt<br>• Push thành công 2 prompts lên Hub<br>• Pull động từ Hub khi thực thi<br>• MD5 deterministic A/B routing theo request_id<br>• Console logs ghi nhận rõ nhãn v1/v2 | 25đ | **25/25** | `src/02_prompt_hub_ab_routing.py`<br>`evidence/02_prompt_hub.png`<br>`evidence/02_ab_routing_log.txt` |
| **Nhiệm vụ 3: RAGAS Evaluation** | • Chạy đủ 50 QA pairs qua cả 2 prompt versions<br>• Chuẩn hóa `EvaluationDataset` / `SingleTurnSample`<br>• Đủ 4 chỉ số: faithfulness, relevancy, recall, precision<br>• Faithfulness $\ge 0.8$ (Thực tế: V1=0.9598, V2=0.9563)<br>• Xuất file báo cáo `data/ragas_report.json` | 25đ | **25/25** | `src/03_ragas_evaluation.py`<br>`evidence/03_ragas_scores.png`<br>`evidence/03_ragas_report.json` |
| **Nhiệm vụ 4: Guardrails AI Validators** | • `@register_validator` tùy chỉnh `PIIDetector`<br>• Regex phát hiện 4 loại PII + `OnFailAction.FIX`<br>• `@register_validator` tùy chỉnh `JSONFormatter`<br>• Tự sửa markdown fences, nháy đơn, dấu phẩy thừa + fallback JSON an toàn<br>• Đủ các test cases theo yêu cầu | 25đ | **25/25** | `src/04_guardrails_validator.py`<br>`evidence/04_pii_demo_log.txt`<br>`evidence/04_json_demo_log.txt` |
| **Điểm thưởng (Bonus)** | • **Faithfulness $\ge 0.9$ ở cả 2 phiên bản** (+3đ)<br>• **Báo cáo phân tích chuyên sâu V1 vs V2** (+2đ)<br>• **Đầy đủ bằng chứng minh họa chuẩn** (+3đ)<br>• **Mã nguồn sạch, docstring, chạy mượt `run_all.py`** (+2đ)<br>• **Xử lý lỗi & fallback an toàn** (+1đ) | +10đ | **+10/10** | `evidence/README.md`<br>`evidence/` folder<br>`src/run_all.py` |
| **TỔNG CỘNG** | | **100đ + 10đ** | **110/100** | |

---

## 5. Kiểm tra trước khi nộp

- [x] `.env` được bảo mật trong `.gitignore` và không bị commit.
- [x] Không có hardcoded API key trong bất kỳ tệp mã nguồn nào.
- [x] Thư mục `evidence/` có đầy đủ 8 tệp hợp lệ (ảnh PNG, file log TXT, file report JSON, file README.md phân tích).
- [x] `evidence/03_ragas_report.json` là định dạng JSON chuẩn.
- [x] Toàn bộ pipeline có thể chạy tuần tự hoặc chạy một lần qua `python src/run_all.py`.

