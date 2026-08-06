---
title: "Addendum: Hệ thống Quản lý & Tính toán Bồi thường, Hỗ trợ GPMB"
status: draft
created: 2026-08-06
updated: 2026-08-06
---

# Addendum

Nội dung bổ sung không đưa vào brief chính (quá chi tiết cho executive brief) nhưng cần giữ lại cho PRD/Architecture sau này.

## Nguồn dữ liệu gốc

- `docs/PA LÊ LỢI THUẬN AN 12.5.2026 kèm theo QĐ.xlsx` — bảng tính chi tiết 30 hộ (28+2), 3 sheet (`28 HỘ BT TÀI SẢN`, `DANH SÁCH`, `2 HỘ CÓ ĐẤT`), mỗi hộ có cấu trúc 7 mục I→VII với công thức Excel cụ thể. Đã được phân tích đầy đủ tại `docs/Phan-tich-PA-boi-thuong-Le-Loi-Thuan-An.md` — tài liệu này là nguồn tham chiếu chính cho đặc tả engine tính toán khi làm PRD/Architecture (danh sách input field, công thức từng cột, ví dụ số).
- `docs/PA-đường liên xã Đức Minh Thuận An_cuối.doc` — văn bản pháp lý chính thức "Phương án bồi thường, hỗ trợ, tái định cư" cho dự án Đường liên xã Đức Minh - Thuận An, 19 hộ, tổng kinh phí 2.031.116.000đ. File .doc định dạng Word cũ (binary), đã trích xuất bằng `antiword` (không đọc trực tiếp được bằng công cụ đọc file text thông thường — cần lưu ý này nếu về sau hệ thống cần parse các file .doc tương tự).
  - Cấu trúc văn bản: Phần I (căn cứ pháp lý — trích dẫn Luật Đất đai 2024, hàng loạt Nghị định/Quyết định của UBND tỉnh Lâm Đồng/huyện Đắk Mil/xã Thuận An), Phần II (nội dung phương án: đất, cây trồng, vật kiến trúc, chính sách hỗ trợ, tổng hợp, nguồn kinh phí).
  - Đây là bằng chứng cho việc phần mềm cần tách biệt rõ: (a) số liệu tính toán (MVP) vs (b) văn bản hành chính hoàn chỉnh có căn cứ pháp lý (ngoài MVP, vì căn cứ pháp lý thay đổi theo tỉnh/huyện/thời điểm ban hành và đòi hỏi cập nhật liên tục — rủi ro cao nếu tự động hoá non-MVP).
  - Cơ cấu tổng hợp chi phí tham khảo được ở đây: Đất + Cây trồng + Vật kiến trúc + Chính sách hỗ trợ + Kinh phí thực hiện công tác GPMB = Tổng kinh phí GPMB. Có sự khác biệt nhỏ so với công thức tổng hợp trong file Excel (dự án Lê Lợi không có mục "hỗ trợ" phát sinh, dự án Đức Minh - Thuận An có) — xác nhận: **mỗi dự án có thể có tổ hợp hạng mục chi phí khác nhau, engine cần linh hoạt theo dự án, không hard-code một công thức tổng hợp cố định.**

## Rủi ro kiến trúc: Umbraco cho lõi nghiệp vụ tính toán

Lý do chọn Umbraco (theo user): tận dụng nền tảng có sẵn để quản lý content/hình ảnh/label, đội ngũ đã quen dùng, giúp phát triển nhanh hơn. Đây là lựa chọn kỹ thuật hợp lý cho tốc độ triển khai với đội ngũ 1–2 người — không phải yêu cầu bắt buộc từ khách hàng.

Điểm cần lưu ý cho Architecture workflow: lõi giá trị của hệ thống (dữ liệu hộ dân, đất/cây trồng/tài sản, engine công thức tính toán, RBAC theo dự án) là bài toán nghiệp vụ/giao dịch — không phải sở trường mặc định của một CMS như Umbraco (vốn tối ưu cho quản lý nội dung/trang/media). Hướng kiến trúc khả thi cần Architecture workflow xác nhận cụ thể:
- Umbraco quản lý phần content/trang landing (nếu có) + tận dụng backoffice có sẵn cho quản lý label/hình ảnh/tài liệu đính kèm.
- Lõi tính toán và quản lý dữ liệu hộ dân/dự án xây bằng custom Umbraco backoffice sections (Umbraco 13+ hỗ trợ custom sections/dashboards) hoặc tách hẳn thành API + data layer riêng, Umbraco chỉ đóng vai trò identity/CMS layer.
- Cần đánh giá: Umbraco Forms/Custom Tables có đủ cho mô hình dữ liệu quan hệ phức tạp (Dự án → Hộ dân → [Đất, Cây trồng, Tài sản] → Công thức) hay cần Umbraco Content-as-a-Service (headless) + database/API riêng cho phần transaction-heavy.

## Cơ sở tính ước lượng thời gian

Giả định dùng để tính bảng ước lượng trong brief chính (team 1 người / 2 người):

- Setup Umbraco + auth/RBAC theo dự án: 1–1.5 tuần
- Data model đa dự án (Dự án, Hộ dân, Đất, Cây trồng, Tài sản/vật kiến trúc): 1.5–2 tuần
- Engine tính toán (công thức đất/cây trồng/tài sản theo `Phan-tich-PA-boi-thuong-Le-Loi-Thuan-An.md`, tổng hợp linh hoạt theo dự án — xem lưu ý ở trên): 2.5–3.5 tuần
- UI quản lý đa dự án: 1–1.5 tuần
- UI nhập liệu kiểm đếm theo hộ (responsive, tối ưu thực địa): 2–2.5 tuần
- Xuất báo cáo Excel/PDF: 1–1.5 tuần
- Với 2 người: một số hạng mục chạy song song (VD: một người làm engine tính toán + data model, người kia làm Umbraco setup + UI) — hệ số tăng tốc ước tính ~30% chứ không tuyến tính 2x, vì engine tính toán và data model có phụ thuộc lẫn nhau.

Đây là ước lượng ở mức brief (chưa qua breakdown epic/story), sai số hợp lý ±20-30%. Khi có Architecture + Epics/Stories, dùng `bmad-sprint-planning` để ra số liệu theo effort thực từng story — sẽ chính xác hơn nhiều để đưa vào hồ sơ chào thầu chính thức.

## Câu hỏi/giả định còn mở cho PRD

- Chưa xác định: có cần vai trò "Admin" quản lý user/gán quyền dự án (khác với Cán bộ và Lãnh đạo) không, hay việc này làm thủ công/qua Umbraco backoffice mặc định?
- Chưa xác định: định dạng xuất báo cáo Excel có cần khớp chính xác layout với file Excel gốc của khách hàng không (để họ dùng quen), hay chỉ cần đúng số liệu với layout mới?
- Chưa xác định: hệ thống có cần lưu lịch sử thay đổi (audit trail) khi cán bộ sửa số liệu đã nhập không — liên quan đến tính toàn vẹn dữ liệu cho một hồ sơ dùng để cấp tiền bồi thường thực tế.
