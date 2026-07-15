# Slide seminar MLLM-MSR

Deck Beamer tiếng Việt cho paper **Harnessing Multimodal Large Language Models for Multimodal Sequential Recommendation** (AAAI 2025).

## Biên dịch

Trong thư mục này chạy:

```bash
xelatex -interaction=nonstopmode main.tex
bibtex main
xelatex -interaction=nonstopmode main.tex
xelatex -interaction=nonstopmode main.tex
```

## Hình ảnh

Các hình trong `img/` được crop cục bộ từ PDF paper AAAI, trang 4–7 của bản PDF tải từ [AAAI proceedings](https://ojs.aaai.org/index.php/AAAI/article/download/33426/35581). Không dùng URL ảnh từ xa trong `.tex`.

## Nguồn

- Paper: https://arxiv.org/abs/2408.09698
- Proceedings: https://ojs.aaai.org/index.php/AAAI/article/view/33426
- Code: https://github.com/YuyangYe/MLLM-MSR

