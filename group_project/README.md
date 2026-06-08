# Báo Cáo Nhóm - RAG Pipeline System

## Thành viên và Phân công
| Thành viên | MSSV | Nhiệm vụ & Strategy đóng góp | Trạng thái |
|-----------|------|----------|------------|
| Lê Quang Thọ | 2A202600597 | **Trưởng nhóm.** Đóng góp **Retrieval Strategy** (Hybrid Search + RRF + PageIndex Fallback). Chịu trách nhiệm ghép nối (Integration) và Evaluation Pipeline. | Đã hoàn thành 100% |
| Nguyễn Văn A | 2A202600XXX | Đóng góp **Data Strategy** (Crawl báo chí tự động với Crawl4AI) và **Chunking Strategy** (RecursiveCharacter 500/50). | Đã hoàn thành 100% |
| Trần Thị B | 2A202600YYY | Đóng góp **Generation Strategy** (Reorder for LLM chống lost-in-the-middle, Multi-provider LLM) và thiết kế **UI Streamlit**. | Đã hoàn thành 100% |

## Kiến trúc hệ thống (Tích hợp từ các thành viên)
Hệ thống là sự kết hợp các chiến lược (strategies) tốt nhất từ bài tập cá nhân của 3 thành viên:

1. **Data Ingestion & Chunking (Strategy của bạn A):**
   - Text extraction từ PDF, DOCX và Crawler JSON tự động.
   - Sử dụng `RecursiveCharacterTextSplitter` (size 500, overlap 50) được chứng minh là giữ ngữ cảnh luật tốt nhất qua bài test cá nhân của A.

2. **Advanced Retrieval Pipeline (Strategy của Thọ):**
   - Đề xuất mô hình 3 lớp: Semantic Search (Dense) song song với Lexical Search (BM25 - Sparse).
   - Gộp kết quả bằng thuật toán `RRF` (Reciprocal Rank Fusion) và `Cross-Encoder Reranking`.
   - **Đột phá:** Đưa logic Fallback sang API `PageIndex` vào hoạt động thực tế khi query score quá thấp.

3. **Generation & UI (Strategy của bạn B):**
   - Kế thừa kỹ thuật `Reorder for LLM` từ bài cá nhân của B để đưa chunk quan trọng lên đầu/cuối prompt.
   - Hỗ trợ linh hoạt 7 nền tảng LLM (OpenAI, Gemini, Anthropic, LiteLLM, Ollama, Grok, Nvidia NIM) điều khiển qua `.env`.
   - Bọc toàn bộ pipeline vào giao diện Streamlit (`app.py`) thân thiện.

4. **Evaluation Framework (Nhiệm vụ chung):**
   - 15 cặp Q&A Golden Dataset trải đều lĩnh vực pháp luật và tin tức.
   - Sử dụng thư viện `DeepEval` để benchmark hệ thống trên 4 chỉ số: Faithfulness, Answer Relevancy, Contextual Relevancy, Contextual Precision.

## Hướng dẫn chạy

```bash
# Cài đặt thư viện
pip install -r requirements.txt
pip install streamlit deepeval

# Cấu hình API key trong file .env (VD: GOOGLE_API_KEY)

# Chạy giao diện Chatbot (Click đúp vào run_chatbot.bat trên Windows)
streamlit run app.py

# Chạy hệ thống đánh giá Evaluation
python group_project/evaluation/eval_pipeline.py
```
