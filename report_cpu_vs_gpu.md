# Báo cáo: So sánh CPU vs GPU — Lab 16 GCP

## Lý do sử dụng CPU thay GPU
Tài khoản GCP Free Trial mặc định khóa quota GPU (hạn mức = 0) cho Project mới.
Quá trình xin tăng quota NVIDIA T4 GPU bị từ chối do tài khoản chưa đủ lịch sử thanh toán.
Do đó, chuyển sang phương án dự phòng: CPU Instance (n1-standard-4) với LightGBM.

## Kết quả Benchmark trên CPU (n1-standard-4: 4 vCPU, 15GB RAM)
- **Dataset:** Credit Card Fraud Detection (284,807 rows, 31 features)
- **Training time:** 2.66 giây — rất nhanh nhờ LightGBM tối ưu cho CPU
- **AUC-ROC:** 0.924 — mức phân loại tốt cho bài toán fraud detection
- **Inference latency:** 2.19ms/row — đủ nhanh cho production real-time

## So sánh CPU vs GPU
- **GPU (T4 + vLLM/Gemma):** Phù hợp cho LLM inference, yêu cầu VRAM lớn, cold start 5-10 phút, chi phí ~$0.54/giờ.
- **CPU (n1-standard-4 + LightGBM):** Phù hợp cho ML truyền thống (tabular data), khởi động nhanh (<3 phút), chi phí ~$0.19/giờ — rẻ hơn 65%.
- **Bài học:** Không phải mọi workload AI đều cần GPU. Với dữ liệu dạng bảng, gradient boosting trên CPU cho kết quả tốt với chi phí thấp hơn đáng kể.
