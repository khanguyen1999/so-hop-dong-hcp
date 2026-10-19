# Sổ hợp đồng HCP

App web một trang để làm hợp đồng Speaker Agreement và file Yêu cầu thanh toán HCP.

## Tính năng
- Danh bạ bác sĩ: lưu thông tin để điền hợp đồng và file thanh toán; nhập từ Excel YCTT cũ hoặc từ hợp đồng .docx cũ.
- Sự kiện & hợp đồng: chọn sản phẩm/hoạt động (Adalat U, Xarelto, Firialta), chọn BS, điền chủ đề/phí, xuất hợp đồng .docx từ phôi (nhiều BS → 1 file .zip). Người ký Marketing tự đổi theo sản phẩm.
- Mã ngân hàng tự điền theo bảng mã Citi 31/03/2025.
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
3. Authentication → URL Configuration → Site URL = link GitHub Pages của app.
4. Điền `SUPABASE_URL` và `SUPABASE_KEY` trong `index.html`.
- `.gitignore` chặn mọi file .docx/.xlsx/.json để không lỡ đẩy dữ liệu lên GitHub.

## Chạy
Mở `index.html` bằng trình duyệt, hoặc deploy thư mục này lên Vercel / Netlify / GitHub Pages (static site, không cần build).

Thư viện tải từ jsDelivr: pizzip 3.2.0, docxtemplater 3.67.0, exceljs 4.4.0, @supabase/supabase-js 2.117.2.
