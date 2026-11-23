# Sổ hợp đồng HCP

App web một trang để làm hợp đồng Speaker Agreement và file Yêu cầu thanh toán HCP.


## GM report (23/11/2026)
- Sự kiện qua ngày → trang sự kiện có khung **GM report**, Tổng quan nhắc "chưa có GM report" (60 ngày gần nhất, có nút Bỏ qua).
- GM with moderator: tự chọn 1 câu hỏi điều phối viên + 2 câu hỏi NVYT từ bộ câu hỏi theo sản phẩm (Adalat, Xarelto, Firialta), hợp chủ đề, tránh trùng 3 report gần nhất. Sửa tay, "Đổi bộ câu khác", hoặc "Tạo câu hỏi riêng bằng AI" (mở Claude với lời nhắc soạn sẵn) rồi "Dán kết quả AI".
- With / without presentation: điền thông tin + 3 dòng đánh giá.
- Xuất file `yymmdd - GM Report ….xlsx` từ mẫu Agenda GM, chỉ hiện sheet report; tìm ô theo nhãn cột A nên chạy được cả mẫu cũ và mẫu gọn. Xuất xong event được đánh dấu đã đóng.
- Bộ câu hỏi: `REPORT_BANK` trong index.html.

## Agenda & FMV (23/11/2026)
- Tạo sự kiện → phần **Agenda & FMV** trong form: chọn hình thức (GM with moderator · with presentation · without presentation), giờ bắt đầu, số NVYT, thành phần tham dự. Agenda tự tạo theo danh sách BS (khai mạc, từng bài, thảo luận, tổng kết), sửa tay được.
- Có BS nhận phí → xuất **Agenda & FMV Checklist V9.1 (.xlsm)**: điền agenda + FMV (requestor, brand, loại hoạt động RTM / Hospital meeting, BS, vai trò, phân loại, tier, số lần trong năm, phí). Giữ nguyên macro, công thức FMV, danh sách chọn.
- Không có phí / TDV báo bài / không trình bày → xuất **Agenda GM (.xlsx)**: chỉ hiện sheet agenda của hình thức đó.
- Thông tin không tự đoán được thì hỏi trước khi xuất. Gợi ý các giá trị đã dùng và nút "Điền nhanh giống sự kiện trước".
- File mẫu lưu ở Supabase (bảng `shared_config`, id `tpl-agenda`), chỉ người đăng nhập đọc được; admin tải lên ở Cài đặt. Không đưa file mẫu lên GitHub.

## Giao diện điện thoại (22/11/2026)
- Thanh tab dưới đáy (Tổng quan · Sự kiện · Thanh toán · Bác sĩ · Cài đặt), tự ẩn khi lướt xuống hoặc đang gõ; thanh trên cùng chỉ còn logo + tên trang.
- Bảng chuyển thành thẻ (nhãn cột bên trái), không phải vuốt ngang; danh sách BS chỉ hiện khoa – bệnh viện, thù lao năm, tình trạng hồ sơ.
- Hộp thoại dạng tấm trượt từ đáy, nền trang khoá khi mở, nút Lưu luôn ở đáy tấm.
- Ô nhập chữ 16px (iPhone không tự phóng to), nút cao tối thiểu 40px.
- Rời app / chuyển app: đẩy ngay thay đổi đang chờ đồng bộ.
- Báo "App có bản mới · Tải lại" khi đã deploy bản mới. **Khi deploy: sửa `APP_VER` trong index.html và `ver.json` cùng số** (số mới phải lớn hơn, dạng YYYY.MM.DD.n).

## Tính năng
- Danh bạ bác sĩ: lưu thông tin để điền hợp đồng và file thanh toán; nhập từ Excel YCTT cũ hoặc từ hợp đồng cũ.
- Nhập từ hợp đồng cũ: nhận Word (.docx, .doc), PDF, PDF scan và ảnh chụp (nhiều ảnh chọn cùng lúc = các trang của 1 hợp đồng). Mẫu nào cũng đọc: tự tìm tên BS, CCCD, địa chỉ, khoa – bệnh viện, ngày sự kiện, vai trò, thù lao, chủ đề, số tài khoản, ngân hàng; hiện lại để sửa trước khi nhập. Bản scan / ảnh được đọc chữ ngay trên máy, không gửi đi đâu.
- Sự kiện & hợp đồng: chọn sản phẩm/hoạt động (Adalat U, Xarelto, Firialta), chọn BS, điền chủ đề/phí, xuất hợp đồng .docx từ phôi (nhiều BS → 1 file .zip). Người ký Marketing tự đổi theo sản phẩm.
- Mã ngân hàng tự điền theo bảng mã Citi 31/03/2025.
- Thêm nhanh BS: dán tin nhắn Zalo/email hoặc ảnh chụp màn hình (đọc chữ tiếng Việt ngay trên máy bằng tesseract.js), app tự tách họ tên, CCCD, SĐT, email, STK, ngân hàng, khoa, bệnh viện.
- Theo dõi tiến độ: HĐ → Raise Veeva → Event → Gửi admin → Đã chi.
- Thanh toán: chọn nhiều BS, xuất file "Yêu cầu thanh toán hợp đồng HCP" đúng mẫu 20 cột; BU/UDA/SIO lấy theo sản phẩm của từng sự kiện.
- Tổng quan: việc cần làm, thù lao theo năm từng BS, nhắc sao lưu.

## Dữ liệu
- Code trong repo **không chứa dữ liệu bác sĩ và không chứa phôi hợp đồng**.
- Chưa đăng nhập: dữ liệu (kể cả CCCD, số tài khoản) và phôi chỉ lưu trong trình duyệt trên máy đang dùng.
- Đã đăng nhập (khi `SUPABASE_URL`/`SUPABASE_KEY` trong `index.html` được điền): dữ liệu và phôi lưu trên Supabase, mỗi tài khoản chỉ đọc/ghi được dữ liệu của mình (Row Level Security). Mở app trên máy khác, đăng nhập là thấy cùng dữ liệu.
- Hai thiết bị cùng sửa: app phát hiện và hỏi giữ bản nào.
- Đăng xuất sẽ xoá bản lưu trên máy đó (dữ liệu vẫn còn trên tài khoản).
- Chỉ điền **Project URL** và **publishable/anon key** vào code. Không bao giờ đưa secret/service_role key vào repo.

## Thiết lập Supabase
1. Tạo project (region Singapore).
2. SQL Editor → chạy:
```sql
create table public.app_state (
  user_id uuid primary key default auth.uid() references auth.users(id) on delete cascade,
  data jsonb not null default '{}'::jsonb,
  template jsonb,
  updated_at timestamptz not null default now()
);
alter table public.app_state enable row level security;
create policy "doc cua minh" on public.app_state for select to authenticated using (auth.uid() = user_id);
create policy "them cua minh" on public.app_state for insert to authenticated with check (auth.uid() = user_id);
create policy "sua cua minh" on public.app_state for update to authenticated using (auth.uid() = user_id) with check (auth.uid() = user_id);
```
3. Cấu hình chung (admin quản lý phôi hợp đồng và bảng sản phẩm cho mọi tài khoản) — chạy thêm:
```sql
create table public.app_admins (email text primary key);
insert into public.app_admins (email) values ('nguyenthanhkha1999@gmail.com');
alter table public.app_admins enable row level security;
create policy "doc danh sach admin" on public.app_admins for select to authenticated using (true);

create table public.shared_config (
  id text primary key,
  products jsonb,
  template jsonb,
  updated_at timestamptz not null default now(),
  updated_by text
);
alter table public.shared_config enable row level security;
create policy "ai dang nhap cung doc" on public.shared_config for select to authenticated using (true);
create policy "chi admin them" on public.shared_config for insert to authenticated
  with check (exists (select 1 from public.app_admins a where a.email = lower(auth.jwt() ->> 'email')));
create policy "chi admin sua" on public.shared_config for update to authenticated
  using (exists (select 1 from public.app_admins a where a.email = lower(auth.jwt() ->> 'email')))
  with check (exists (select 1 from public.app_admins a where a.email = lower(auth.jwt() ->> 'email')));
```
   Thêm admin: `insert into public.app_admins (email) values ('email@...');` và thêm email đó vào `ADMIN_EMAILS` trong `index.html`.
4. Authentication → URL Configuration → Site URL = link GitHub Pages của app.
5. Điền `SUPABASE_URL` và `SUPABASE_KEY` trong `index.html`.
- `.gitignore` chặn mọi file .docx/.xlsx/.json để không lỡ đẩy dữ liệu lên GitHub.

## Chạy
Mở `index.html` bằng trình duyệt, hoặc deploy thư mục này lên Vercel / Netlify / GitHub Pages (static site, không cần build).

Thư viện tải từ jsDelivr: pizzip 3.2.0, docxtemplater 3.67.0, exceljs 4.4.0, @supabase/supabase-js 2.117.2, tesseract.js 6.0.1, pdfjs-dist 4.10.38 (bản legacy, chạy với isEvalSupported:false).

## Trang phụ: Lịch khám Tâm Anh
Thư mục `lich-kham/` là trang tĩnh xem lịch khám BS Nội Thận, Nội tiết, Tim mạch (TTTM) và Nội tổng quát tại BVĐK Tâm Anh TP.HCM và PKĐK Tâm Anh Quận 7 (12–30/11/2026), độc lập với app hợp đồng: không dùng Supabase, không đụng dữ liệu HCP. Mở tại `<link GitHub Pages>/lich-kham/`. Cập nhật tháng mới: thay dữ liệu trong `lich-kham/index.html`.
