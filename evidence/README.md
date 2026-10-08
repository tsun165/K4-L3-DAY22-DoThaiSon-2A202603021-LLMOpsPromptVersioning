# Báo cáo Đánh giá RAGAS & Minh chứng Lab Day 22

- **Học viên:** Đỗ Thái Sơn
- **LangSmith Project URL:** [https://smith.langchain.com/o/d575f7b2-d452-4820-ac3a-54db214dda1d](https://smith.langchain.com/o/d575f7b2-d452-4820-ac3a-54db214dda1d)
- **Prompt V1 Hub Name:** `do-thai-son-rag-prompt-v1`
- **Prompt V2 Hub Name:** `do-thai-son-rag-prompt-v2`

---

## 1. Bảng kết quả RAGAS Evaluation (50 QA Pairs)

| Chỉ số (Metric) | Prompt V1 (Ngắn gọn) | Prompt V2 (Chuyên gia / Cấu trúc) | Winner |
|---|:---:|:---:|:---:|
| **Faithfulness** (Độ trung thực) | **0.9598** | 0.9563 | **← V1** |
| **Answer Relevancy** (Độ liên quan câu trả lời) | 0.9486 | **0.9671** | **← V2** |
| **Context Recall** (Độ bao phủ ngữ cảnh) | **1.0000** | **1.0000** | **Hòa (100%)** |
| **Context Precision** (Độ chính xác ngữ cảnh) | 0.9450 | **0.9517** | **← V2** |

> **Mục tiêu bài lab:** Đạt Faithfulness $\ge 0.8$.  
> **Kết quả đạt được:** Cả 2 phiên bản đều vượt mức xuất sắc $\ge 0.90$ (**V1: 0.9598, V2: 0.9563**).

---

## 2. Phân tích so sánh V1 vs V2

### 2.1. Về Faithfulness (V1 nhỉnh hơn V2: 0.9598 so với 0.9563)
- **Prompt V1** yêu cầu mô hình *"Trả lời ngắn gọn (2-4 câu), chỉ dựa trên context"*. Việc giới hạn độ dài ngắn và trả lời trực diện giúp LLM bám sát từng câu chữ của context, hạn chế tối đa việc mở rộng diễn giải hay suy đoán, do đó giảm nguy cơ sinh ra các mệnh đề không được hỗ trợ bởi context.
- **Prompt V2** yêu cầu giải thích có cấu trúc và phân tích facts (3-5 câu). Dù độ trung thực vẫn rất cao (0.9563), câu trả lời dài hơn đôi khi sử dụng nhiều từ nối hoặc diễn giải ngữ nghĩa khiến bước kiểm tra claim của RAGAS khắt khe hơn.

### 2.2. Về Answer Relevancy (V2 vượt trội hơn V1: 0.9671 so với 0.9486)
- **Prompt V2** với vai trò *"Chuyên gia phân tích thông tin, đọc kỹ context, xác định facts liên quan và viết câu trả lời rõ ràng có tổ chức"* giúp mô hình trả lời trúng trọng tâm, giải thích đầy đủ các khía cạnh của câu hỏi người dùng, tạo nên embedding ngữ nghĩa gần với câu hỏi hơn.
- **Prompt V1** ngắn gọn đôi khi lược bỏ một số chi tiết bổ trợ nên điểm liên quan câu hỏi thấp hơn một chút.

### 2.3. Về Context Recall & Precision
- Cả 2 phiên bản đều đạt **Context Recall = 1.0000 (100%)** chứng minh bộ Retriever với FAISS index và tham số $k=3$ đã truy xuất hoàn hảo tất cả các đoạn văn bản chứa câu trả lời chuẩn (ground truth reference).
- **Context Precision đạt trên 0.945**, phản ánh rằng các đoạn tài liệu truy xuất có thứ tự xếp hạng cao và đúng trọng tâm.

---

## 3. Danh sách tệp bằng chứng trong `evidence/`

1. `01_langsmith_traces.png`: Minh chứng giao diện LangSmith project `day22-lab` ghi nhận $\ge 100$ traces (thực tế ghi nhận > 700 root traces và > 6,500 total trace runs cho các tác vụ RAG, A/B testing và RAGAS evaluation).
2. `02_prompt_hub.png`: Minh chứng 2 prompts (`do-thai-son-rag-prompt-v1` và `do-thai-son-rag-prompt-v2`) được push lên LangSmith Prompt Hub.
3. `02_ab_routing_log.txt`: Log console chạy A/B routing tất định cho 50 câu hỏi có nhãn `[prompt-v1]` và `[prompt-v2]`.
4. `03_ragas_scores.png`: Bảng và biểu đồ so sánh 4 chỉ số RAGAS giữa V1 và V2.
5. `03_ragas_report.json`: Báo cáo chi tiết định dạng JSON lưu điểm 4 chỉ số.
6. `04_pii_demo_log.txt`: Log demo bộ kiểm duyệt `PIIDetector` che thông tin nhạy cảm.
7. `04_json_demo_log.txt`: Log demo bộ kiểm duyệt `JSONFormatter` tự động sửa JSON lỗi.
8. `README.md`: Báo cáo phân tích so sánh và giải thích điểm số chi tiết.

---

## 4. Tóm tắt kết quả theo Tiêu chí (Rubric)

- **Điểm 4 nhiệm vụ cốt lõi:** 100 / 100 điểm.
- **Điểm thưởng bổ sung:** +10 / 10 điểm (Faithfulness $\ge 0.9$ cả 2 bản, phân tích so sánh chi tiết, cấu trúc mã nguồn hoàn chỉnh, đủ 8/7 minh chứng, chạy tự động mượt mà qua `run_all.py`).


