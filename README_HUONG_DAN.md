# VIETINBANK MOBILE SCAN — BẢN THỬ NGHIỆM

## 1. Những gì đã có
- `index.html`: Web App dùng trên điện thoại, chọn/chụp ảnh GPKD và CCCD (trước/sau), OCR tiếng Việt + tiếng Anh, rà soát thủ công, xuất `.xlsx` 1 doanh nghiệp/1 dòng.
- `manifest.webmanifest`, `sw.js`: thêm vào màn hình chính như ứng dụng web; trang chính được cache khi truy cập HTTPS.
- `MAU_EXCEL_MAIL_MERGE.xlsx`: tên cột tương thích file Excel app xuất.
- `MAU_WORD_GOC_VIETINBANK.docx`: bản sao nguyên mẫu Word đã cung cấp; **chưa chèn MERGEFIELD**, người dùng phải cấu hình Mail Merge trong Word lần đầu.

## 2. Cách đưa lên điện thoại (không cần Python)
1. Tạo repository GitHub **chỉ dành cho dữ liệu và mã nguồn demo** (không tải ảnh CCCD/GPKD thật lên repository).
2. Tải `index.html`, `manifest.webmanifest`, `sw.js` lên thư mục gốc repository.
3. GitHub > Settings > Pages > Deploy from branch > main / root > Save.
4. Mở địa chỉ HTTPS do GitHub Pages cung cấp trên điện thoại. iPhone: Safari > Share > Add to Home Screen; Android: Chrome > menu > Add to Home screen.
5. Lần đầu phải có Internet để tải Tesseract.js, dữ liệu OCR tiếng Việt và thư viện Excel từ CDN. OCR chạy trong trình duyệt, không có API upload tài liệu.

**Giới hạn:** Các thư viện OCR/Excel và gói ngôn ngữ tải qua CDN ngoài. Không nên dùng bản này cho giấy tờ thật nếu chưa được CNTT/ATTT phê duyệt. Không có xác thực, phân quyền, nhật ký kiểm toán, quản lý vòng đời dữ liệu. Trình duyệt hoặc tiện ích mở rộng có thể truy cập nội dung; không bảo đảm hoạt động hoàn toàn offline. Phiên bản này chỉ nhận ảnh JPG/PNG; PDF cần chuyển thành ảnh trước. OCR dùng quy tắc đơn giản, có thể bỏ sót hoặc đọc sai nhiều trường, nhất là CCCD mẫu mới và ảnh chất lượng thấp.

## 3. Cách Mail Merge Word từ Excel
1. Dùng Word trên máy tính mở `MAU_WORD_GOC_VIETINBANK.docx`; lưu một bản sao để không sửa mẫu gốc.
2. Mailings > Start Mail Merge > Letters.
3. Mailings > Select Recipients > Use an Existing List > chọn file Excel app xuất > chọn sheet `MAIL_MERGE`.
4. Đặt con trỏ ở vị trí cần điền trong mẫu Word, xóa bớt dấu chấm nếu cần, chọn Mailings > Insert Merge Field > chọn tên trường tương ứng. Ví dụ `TEN_DN`, `MST`, `HO_TEN_DD`, `SO_CCCD`, `NGAY_CAP_CCCD`.
5. Với các vị trí thông tin lặp lại, chèn lại cùng tên trường. **Không** điền vào ô chữ ký/mẫu dấu hoặc tự chọn FATCA/chủ sở hữu hưởng lợi.
6. Preview Results để kiểm tra, sau đó Finish & Merge > Edit Individual Documents; kiểm tra lại định dạng, số trang và dữ liệu trước khi in.
7. Lưu mẫu đã gắn MERGEFIELD để tái sử dụng với các file Excel sau này.

## 4. Ánh xạ trường dữ liệu quan trọng
- Tên tiếng Việt: `TEN_DN`; tiếng Anh: `TEN_TA`; viết tắt: `TEN_VT`.
- Số giấy tờ thành lập / mã số doanh nghiệp: `MST` (cần đối chiếu quy trình nội bộ trước khi sử dụng cùng giá trị cho hai mục).
- Ngày đăng ký lần đầu: `NGAY_DK`; trụ sở chính: `DIA_CHI_DN`; điện thoại: `SDT_DN`; email: `EMAIL_DN`; vốn điều lệ: `VON_DL`.
- Người đại diện: `HO_TEN_DD`; giới tính: `GIOI_TINH_DD`; chức vụ: `CHUC_VU_DD`; ngày sinh: `NGAY_SINH_DD`; quốc tịch: `QUOC_TICH_DD`.
- Giấy tờ tùy thân: `SO_CCCD`; ngày cấp: `NGAY_CAP_CCCD`; nơi cấp: `NOI_CAP_CCCD`; nơi ở hiện tại: `DIA_CHI_HIEN_TAI`; thường trú: `DIA_CHI_THUONG_TRU`.

## 5. Kiểm tra bản demo
Nhấn `Nạp dữ liệu mẫu giả lập` > xác nhận kiểm tra > `Xuất Excel (.xlsx)` > mở Excel kiểm tra tên cột và dữ liệu. Tiếp đó thử OCR với giấy tờ **giả lập**. Không dùng OCR như cơ sở duy nhất để chấp nhận hồ sơ.
