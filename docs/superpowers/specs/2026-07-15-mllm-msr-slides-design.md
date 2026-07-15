# Thiết kế slide seminar MLLM-MSR

## Mục tiêu

Tạo một thư mục slide Beamer mới trong repo `MLLM-MSR`, dùng format của
`/home/pearspringmind/NCKH/autoregressive-models-beyond-language/slides` làm
template. Slide phục vụ một người trình bày với giảng viên hướng dẫn trong
30–40 phút, bằng tiếng Việt, tập trung vào hiểu sâu paper “Harnessing
Multimodal Large Language Models for Multimodal Sequential Recommendation”.

## Phạm vi và nguyên tắc

- Giữ layout 16:9, preamble, typography, màu sắc và cách tổ chức file theo
  template mẫu nếu không có lý do kỹ thuật để thay đổi.
- Bỏ toàn bộ danh sách sinh viên/giảng viên trên trang bìa; chỉ giữ tiêu đề,
  thông tin paper, venue và ngày trình bày.
- Không đưa các chi tiết chưa được paper hoặc repo xác nhận thành kết luận.
  Các quan sát từ code sẽ được ghi rõ là “implementation trong repo”.
- Nội dung paper là nguồn chính; code repo dùng để minh họa khả năng tái hiện
  pipeline và các lựa chọn triển khai.

## Cấu trúc slide dự kiến

1. Bìa và thông tin trích dẫn paper.
2. Mục lục.
3. Bối cảnh: sequential recommendation, multimodal recommendation và giới hạn
   của cách biến lịch sử thành text prompt.
4. Bài toán và ký hiệu: user sequence, item text/image, next-item prediction.
5. Câu hỏi nghiên cứu và ba thách thức: multimodal sequence dài, temporal
   preference dynamics, và khả năng recommendation của MLLM.
6. Ý tưởng tổng quan MLLM-MSR.
7. Stage 1 — item summarizer: tóm tắt text/image và fusion thành mô tả item.
8. Stage 2 — recurrent user preference summarizer: cập nhật preference theo
   từng chunk của lịch sử.
9. Ví dụ prompt và một vòng cập nhật preference.
10. MLLM-based recommender: input/output và formulation yes/no.
11. SFT objective, LoRA và các lựa chọn fine-tuning.
12. Dataset, preprocessing và cách chia train/validation/test.
13. Baseline, metric và protocol đánh giá.
14. Kết quả chính trên các dataset.
15. Phân tích đóng góp của multimodal information.
16. Ablation: item summarization, recurrent summarization và fine-tuning.
17. Qualitative case study: lịch sử, preference summary và dự đoán.
18. Đối chiếu paper với implementation trong repo.
19. Chi phí, giới hạn và rủi ro triển khai.
20. Takeaways, câu hỏi thảo luận và tài liệu tham khảo.

Nếu bảng kết quả trong PDF có nhiều dataset/metric, các slide 14–16 sẽ được
chia nhỏ để tránh chữ quá nhỏ; tổng số slide có thể tăng nhưng vẫn giữ thời
lượng 30–40 phút.

## Tài sản hình ảnh

- Crop hình framework, sơ đồ phương pháp, bảng/qualitative example cần thiết
  từ PDF paper và lưu bản cục bộ trong thư mục `img/` của slide mới.
- Tên file phải mô tả nguồn và nội dung, ví dụ `paper_framework.png` và
  `paper_qualitative_case_01.png`.
- Các hình crop sẽ được kiểm tra độ phân giải và khả năng đọc khi render PDF.
- Không phụ thuộc vào URL ảnh bên ngoài trong file `.tex`; mọi ảnh trình bày
  phải import từ thư mục cục bộ.
- Nếu hình paper khó đọc khi crop, sẽ vẽ lại sơ đồ pipeline đơn giản bằng
  TikZ/Beamer và ghi chú rõ đây là sơ đồ diễn giải, không phải hình gốc.

## Tổ chức thư mục đầu ra

Tạo thư mục `slides/` ở root repo hiện tại với cấu trúc tương tự template:

```text
slides/
├── main.tex
├── preamble/
├── content/
├── img/
└── ref/
```

Các file content được chia theo mạch trình bày, không dồn toàn bộ nội dung vào
một file; `main.tex` chỉ chịu trách nhiệm ghép các phần.

## Kiểm thử và tiêu chí hoàn thành

- Biên dịch được `slides/main.tex` thành PDF bằng toolchain LaTeX có sẵn.
- Không còn placeholder, TODO, tên người trình bày cũ hoặc citation key không
  tồn tại.
- Render PDF và kiểm tra trực quan tất cả trang, đặc biệt bảng, công thức,
  crop hình và slide có nhiều cột.
- Nội dung phải thể hiện rõ: đóng góp chính, luồng dữ liệu hai stage, objective
  recommendation, experimental protocol, kết quả/ablation và giới hạn.
- README của thư mục slide ghi cách biên dịch và nguồn paper/ảnh.

## Nguồn chính

- Paper: https://arxiv.org/abs/2408.09698
- Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/33426
- Code: https://github.com/YuyangYe/MLLM-MSR
