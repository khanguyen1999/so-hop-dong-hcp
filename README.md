# Sổ hợp đồng HCP

App web một trang để làm hợp đồng Speaker Agreement và file Yêu cầu thanh toán HCP.

## Tính năng
- Danh bạ bác sĩ: lưu thông tin để điền hợp đồng và file thanh toán; nhập từ Excel YCTT cũ hoặc từ hợp đồng .docx cũ.
- Sự kiện & hợp đồng: chọn BS, điền chủ đề/phí, xuất hợp đồng .docx từ phôi (nhiều BS → 1 file .zip).
- Theo dõi tiến độ: HĐ → Raise Veeva → Event → Gửi admin → Đã chi.
- Thanh toán: chọn nhiều BS, xuất file "Yêu cầu thanh toán hợp đồng HCP" đúng mẫu 20 cột.
- Tổng quan: việc cần làm, thù lao theo năm từng BS, nhắc sao lưu.

## Dữ liệu
- Code trong repo **không chứa dữ liệu bác sĩ và không chứa phôi hợp đồng**.
- Dữ liệu (kể cả CCCD, số tài khoản) chỉ lưu trong trình duyệt (localStorage) trên máy đang dùng.
- Phôi hợp đồng (.docx) được tải lên trong app và cũng chỉ lưu trong trình duyệt.
- Chuyển máy: Cài đặt → Sao lưu (.json) → mở app ở máy mới → Khôi phục.
- `.gitignore` chặn mọi file .docx/.xlsx/.json để không lỡ đẩy dữ liệu lên GitHub.

## Chạy
Mở `index.html` bằng trình duyệt, hoặc deploy thư mục này lên Vercel / Netlify / GitHub Pages (static site, không cần build).

Thư viện tải từ jsDelivr: pizzip 3.2.0, docxtemplater 3.67.0, exceljs 4.4.0.
