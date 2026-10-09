# OCR-Web — Nhận diện văn bản tiếng Việt

Website OCR chạy hoàn toàn trên máy bạn cho **văn bản in / đánh máy** từ ảnh (PNG, JPG, JPEG, WEBP)
Phát hiện vùng chữ bằng **PaddleOCR**, đọc chữ bằng **VietOCR**, có chấm độ tin cậy từng dòng và (tùy chọn) AI chỉnh sửa

## Chạy

```bat
pip install -r requirements.txt
python app.py          :: hoặc bấm run.bat trên Windows
```
Mở http://127.0.0.1:5000. Lần đầu khởi động cần tải model (vài phút), các lần sau chạy ngay.
`run.bat` tự tìm `venv`/`.venv` trong thư mục dự án (hoặc `D:\VietOCR\venv`) và chỉ mở trình duyệt khi máy chủ đã sẵn sàng.

## Tính năng

- **Ảnh chụp bị nghiêng/ngược/có bóng**: tự cắt tờ giấy, nắn phối cảnh, cân sáng, khử nhiễu, tự xoay đúng chiều, chỉnh nghiêng.
- **Nhận lại ký hiệu so sánh `<` `>` `=` `<=` `>=`**: VietOCR hay đọc sai các ký hiệu này (vd `(0 < n < 100)` thành `(0 Sn 5 100)`).
  Hệ thống nhận chúng bằng hình dạng (so khớp mẫu, ngưỡng chặt để không thay nhầm chữ/số), xóa khỏi ảnh dòng, đọc lại phần chữ rồi chèn
  ký hiệu đúng chỗ (`symbols.py`). Dòng nào được xử lý sẽ có ghi chú ở mục "Ảnh sau khi xử lý". Khi số từ model đọc được không khớp số cụm chữ (vd đọc dính `p n` thành `p.n`), hệ thống đọc riêng từng đoạn giữa các ký hiệu để ký hiệu không bị dồn sai chỗ. Chưa xử lý: ký hiệu dính liền chữ không có khoảng trắng
  (vd `0<n<100`), ký hiệu quá nhỏ/mờ (chữ cao dưới khoảng 20px), và `≤` `≥` `≠`.
- **Độ tin cậy từng dòng** (xanh/vàng/đỏ) + phát hiện từ không thể là tiếng Việt; dòng kém chắc chắn được đọc lại tự động.
- **Nút Dừng** để huỷ khi đang nhận diện; **dán ảnh bằng Ctrl+V**.
- Chỉnh sửa kết quả, khôi phục bản OCR gốc, xuất TXT / PDF (đủ dấu tiếng Việt; nếu thiếu `reportlab` hệ thống tự dùng Pillow để vẫn xuất được PDF).
- **Kết quả giữ đúng từng dòng như trong ảnh** (mặc định): mỗi dòng chữ trong ảnh là đúng một hàng trong ô kết quả, ô kết quả không tự bẻ dòng
  nên tiêu đề (Chương/Điều) và các khoản 1. 2. 3. không bị dính vào nhau. Có thể bật tuỳ chọn *Nối các dòng cùng một đoạn thành đoạn văn*
  nếu muốn văn xuôi liền mạch; khi đó tiêu đề và từng khoản vẫn luôn tách riêng. Chỉ đổi khoảng trắng nên CER/WER không đổi (`reflow.py`).
- **AI hiệu đính (tùy chọn)**: Gemini (hoặc Claude dự phòng). Chỉ gửi văn bản (không gửi ảnh); AI chỉ ĐỀ XUẤT, từng đề xuất có nút
  *Áp dụng chỉnh sửa / Giữ nguyên*, không tự ghi đè kết quả OCR gốc. Không có AI hoặc AI lỗi thì OCR vẫn chạy bình thường.

## Ba chỉ số khác nhau 

| Chỉ số | Là gì | Nguồn |
|---|---|---|
| **Quality Score** (🟢🟡🔴) | Ảnh đầu vào dễ hay khó đọc: độ nét, sáng, tương phản, độ phân giải, nhiễu, nền không đều/bóng, độ nghiêng, chữ quá nhỏ | `quality.py` + `input_advisor.py` (cục bộ, không AI ngoài) |
| **OCR Confidence** | Mức tự tin của model VietOCR (theo ký tự). **Không phải** độ chính xác thật | `ocr_engine.py` |
| **Accuracy** | Độ đúng thật = so với văn bản chuẩn (Ground Truth): CER, WER, Character Accuracy | `accuracy.py` |

Chưa có Ground Truth thì giao diện hiện **"Accuracy: Chưa đánh giá"**, không có con số nào.

## Đầu vào: cảnh báo, đề xuất, xử lý

Ảnh → Quality Score → cảnh báo xanh/vàng/đỏ + đề xuất ("Có thể tăng tương phản trước khi nhận diện") → preprocessing có sẵn
(khử nhiễu, cân sáng, CLAHE, làm nét, chỉnh nghiêng, phóng to, nhị phân hóa) → PaddleOCR → VietOCR. Giao diện cho xem **ảnh gốc** và
**ảnh sau xử lý** cùng danh sách những gì đã làm. Ảnh gốc không bao giờ bị thay.

## Đánh giá Accuracy bằng Ground Truth

**Trên giao diện**: sau khi OCR, mở mục *Accuracy*, dán văn bản đúng (hoặc mở file .txt) → *Tính CER / WER*.
Có bản gốc dạng **PDF số** (vd: xuất từ Word)? Bấm *Lấy từ PDF có sẵn chữ* để nạp chữ chính xác làm Ground Truth (mỗi dòng PDF một hàng; in/chụp lại thành ảnh rồi OCR ảnh đó để đo model).
**Ảnh và PDF scan bị từ chối** vì phải OCR mới có chữ, so OCR với chính OCR sẽ ra Accuracy giả. Với ảnh hãy tự gõ lại văn bản đúng.

**Trên cả bộ ảnh kiểm thử**:
```bat
python evaluate.py                       :: OCR toàn bộ eval_data/ rồi tính CER / WER / Accuracy theo từng loại ảnh
python evaluate.py --only-gt             :: chỉ ảnh đã có Ground Truth
python evaluate.py --ocr-outputs thu_muc :: so sánh file .txt OCR có sẵn (không cần model)
python tools/degrade.py anh_sach.png --gt anh_sach.txt   :: tạo biến thể mờ/tối/sáng/nhiễu/bóng/nghiêng/độ phân giải thấp từ 1 ảnh sạch
```
Thêm mẫu: ảnh vào `eval_data/images/<loai>/ten.png`, văn bản đúng (UTF-8) vào `eval_data/ground_truth/<loai>/ten.txt`. Chi tiết: `eval_data/README.md`.
Báo cáo: `eval_reports/<thời gian>/summary.md` (+ `results.csv`, `results.json`). Thư mục `eval_data` **cố ý để trống**: không có dữ liệu giả.

## Cấu hình (biến môi trường, đều tùy chọn)

| Biến | Ý nghĩa |
|---|---|
| `OCR_HOST`, `OCR_PORT` | Địa chỉ/cổng (mặc định `127.0.0.1:5000`; đặt `0.0.0.0` để mở cho máy khác trong LAN) |
| `OCR_DEVICE` | `cpu` hoặc `cuda` (mặc định tự dùng GPU NVIDIA nếu có) |
| `GEMINI_API_KEY`, `GEMINI_MODEL` | Bật AI hiệu đính bằng Gemini (không cần đặt model) |
| `ANTHROPIC_API_KEY`, `ANTHROPIC_MODEL` | Claude làm phương án dự phòng |
| `OCR_AI_DEBUG=1` | Ghi văn bản đã gửi AI + phản hồi thô vào `logs/ai_debug.jsonl` để kiểm tra (mặc định TẮT vì chứa văn bản của bạn) |

API key chỉ đọc từ biến môi trường, không có trong mã nguồn. Ảnh không bao giờ được gửi cho AI ngoài; chỉ văn bản sau OCR, và chỉ khi bạn bấm *Kiểm tra bằng AI* rồi xác nhận.

Chẩn đoán AI: http://127.0.0.1:5000/api/ai-check · Tình trạng: `/api/health`.

## Giới hạn

Ảnh ≤ 10 MB. Chỉ nhận diện văn bản **in**, không phải chữ viết tay.
Chưa tách cột cho bố cục nhiều cột (các dòng của hai cột cùng độ cao được đọc xen kẽ theo hàng — giữ nguyên để bảng/biểu mẫu đọc đúng).

## Kiểm thử

```bat
python -m unittest discover -s tests          :: 87 test, không cần model, không cần pytest
pip install playwright && playwright install chromium
python tests/e2e_browser.py                   :: kiểm thử giao diện thật bằng Chromium (engine OCR giả)
```

## Cấu trúc

| File | Vai trò |
|---|---|
| `app.py` | Flask: API, kiểm tra file tải lên, bộ nhớ đệm, xuất PDF, header bảo mật (CSP) |
| `ocr_engine.py` | Phát hiện dòng → tự xoay/chỉnh nghiêng → VietOCR theo lô + đọc lại dòng kém chắc chắn |
| `preprocess.py` / `quality.py` | Xử lý ảnh bằng OpenCV / chấm Quality Score (nét, sáng, tương phản, nhiễu, bóng, nghiêng, độ phân giải) |
| `reflow.py` | (Tùy chọn) nối các dòng cùng đoạn thành đoạn văn theo tọa độ chữ; luôn tách tiêu đề và các khoản 1. 2. 3. |
| `symbols.py` | Nhận lại ký hiệu so sánh `< > = <= >=` bằng hình dạng, xóa khỏi ảnh dòng và chèn lại sau khi đọc |
| `pdf_text.py` | Chỉ để lấy Ground Truth từ PDF số: trích chữ có sẵn, có kiểm tra độ tin cậy (từ chối PDF scan/font lỗi) |
| `pdf_export.py` | Xuất kết quả ra file PDF: reportlab, tự chuyển sang Pillow nếu reportlab lỗi/thiếu |
| `input_advisor.py` | Biến kết quả đo thành cảnh báo, đề xuất và kế hoạch xử lý đầu vào (cục bộ) |
| `accuracy.py` / `evaluate.py` | CER / WER / Accuracy so với Ground Truth; chạy đánh giá cả bộ ảnh |
| `eval_data/`, `tools/degrade.py` | Bộ kiểm thử (để trống, tự thêm) và công cụ tạo biến thể ảnh xấu |
| `vi_check.py` | Kiểm tra cấu trúc âm tiết tiếng Việt (offline) |
| `ai_review.py` | AI hiệu đính: chỉ đề xuất, chặn sửa quá mức, gắn cờ số liệu/tên/mã, có thử lại/đổi model tự động |
