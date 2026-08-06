---
title: "Product Brief: Hệ thống Quản lý & Tính toán Bồi thường, Hỗ trợ GPMB"
status: draft
created: 2026-08-06
updated: 2026-08-06
---

# Product Brief: Hệ thống Quản lý & Tính toán Bồi thường, Hỗ trợ GPMB

*(Tên làm việc — có thể đổi khi đặt tên thương mại cho hồ sơ chào thầu)*

## Executive Summary

Ban Quản lý dự án đầu tư xây dựng hiện tính toán phương án bồi thường, hỗ trợ, giải phóng mặt bằng (GPMB) bằng file Excel thủ công — mỗi hộ dân một khối dữ liệu lặp lại qua 7 mục (nhân thân, đất, cây trồng, tài sản/vật kiến trúc, chính sách hỗ trợ, tái định cư, tổng hợp), với hàng chục công thức tính theo đơn giá, hệ số, tỉ lệ bồi thường quy định tại nhiều Quyết định của UBND cấp tỉnh/huyện. Cách làm này chậm, dễ sai sót khi copy công thức qua hàng trăm dòng, và khó kiểm soát khi Ban phải chạy song song nhiều dự án (nhiều tuyến đường) trong cùng thời điểm.

Chúng tôi đề xuất xây dựng một hệ thống web nội bộ, chạy trên nền **Umbraco CMS + API tuỳ biến**, cho phép cán bộ nhập dữ liệu kiểm đếm (đất, cây trồng, tài sản) và hệ thống tự động tính toán số tiền bồi thường theo đúng công thức nghiệp vụ đã được kiểm chứng từ hồ sơ thực tế, xuất ra bảng tổng hợp/Excel/PDF làm căn cứ soạn văn bản phương án chính thức. Hệ thống quản lý được nhiều dự án song song, mỗi dự án có bộ đơn giá/quyết định riêng, và phân quyền theo dự án giữa cán bộ nhập liệu và lãnh đạo theo dõi.

Giá trị cốt lõi: **giảm thời gian tính toán từ hàng ngày xuống còn vài giờ, loại bỏ sai sót công thức thủ công, và cho cán bộ một công cụ dùng hàng ngày thay vì file Excel hàng nghìn dòng khó bảo trì.**

## The Problem

- **Tính tay trên Excel khổng lồ, dễ sai:** một file thực tế đã lên tới ~1080 dòng cho 30 hộ, mỗi hộ lặp lại cấu trúc 7 mục với công thức riêng cho từng loại đất/cây trồng/tài sản. Sai một ô công thức copy nhầm dòng có thể làm sai lệch số tiền bồi thường của cả hộ.
- **Đơn giá và hệ số thay đổi liên tục theo quyết định pháp lý** (đơn giá đất theo quyết định giá đất huyện, đơn giá cây trồng theo QĐ tỉnh, đơn giá xây dựng mới theo QĐ khác) — quản lý thủ công dễ dùng nhầm đơn giá cũ hoặc sai đối tượng áp dụng.
- **Không quản lý được nhiều dự án song song một cách có hệ thống:** Ban QLDA thực tế đang chạy nhiều tuyến đường cùng lúc (ví dụ dự án Lê Lợi và dự án Đức Minh - Thuận An), mỗi dự án một file Excel riêng, không có nơi tổng hợp/theo dõi tập trung.
- **Không có kiểm soát truy cập:** file Excel chia sẻ qua email/USB, không phân biệt được ai được sửa, ai chỉ cần xem — rủi ro với dữ liệu nhạy cảm (CCCD, tài sản cá nhân của hộ dân).
- **Chi phí cơ hội:** thời gian cán bộ dành cho việc dò công thức, đối chiếu số liệu, sửa lỗi Excel lẽ ra nên dành cho kiểm đếm thực địa và làm việc với hộ dân.

## The Solution

Một web app quản lý bồi thường GPMB, xây trên Umbraco CMS + API tuỳ biến:

1. **Quản lý dự án:** tạo/quản lý nhiều dự án GPMB song song, mỗi dự án có bộ đơn giá đất/cây trồng/xây dựng riêng, danh sách hộ dân riêng.
2. **Nhập liệu kiểm đếm theo hộ:** form nhập dữ liệu theo đúng 7 mục nghiệp vụ (I. Nhân thân → VII. Tổng hợp), tối ưu cho nhập nhanh trên di động/tablet ngoài thực địa.
3. **Engine tính toán tự động:** áp dụng đúng công thức đã kiểm chứng từ hồ sơ thực tế cho từng loại — bồi thường đất (theo tỉ lệ, đơn giá, khấu trừ nghĩa vụ tài chính), cây trồng (theo mật độ, giai đoạn kiến thiết cơ bản/kinh doanh, đơn giá), tài sản/vật kiến trúc (theo khối lượng đo đạc, khấu hao, hệ số thiếu kết cấu) — và tổng hợp toàn dự án kèm chi phí dự phòng.
4. **Xuất báo cáo:** bảng tổng hợp theo hộ và toàn dự án, xuất Excel/PDF làm căn cứ để cán bộ soạn văn bản phương án chính thức (soạn thảo văn bản pháp lý vẫn thực hiện thủ công ở giai đoạn này — xem Scope).
5. **Phân quyền theo dự án:** cán bộ được gán quyền xem+sửa trên (các) dự án cụ thể; lãnh đạo được gán quyền chỉ xem để theo dõi tiến độ và số liệu tổng hợp.

## What Makes This Different

- **Công thức được rút ra từ dữ liệu thực tế, không phải suy đoán:** toàn bộ logic tính toán (đất, cây trồng, tài sản) đã được phân tích chi tiết từ hồ sơ Excel và văn bản phương án đã phê duyệt thực tế của khách hàng — giảm rủi ro sai lệch nghiệp vụ so với xây dựng từ đầu theo văn bản luật thuần túy.
- **Tốc độ triển khai nhờ tận dụng nền tảng có sẵn:** chọn Umbraco vì đội ngũ đã quen dùng, tận dụng backoffice có sẵn cho quản lý nội dung/hình ảnh/label thay vì xây from-scratch — mục tiêu là giao được sản phẩm nhanh với đội ngũ nhỏ (1–2 người).
- **Đây không phải là một lợi thế công nghệ độc quyền** — đây là lợi thế thực thi (execution speed) và độ chính xác nghiệp vụ đã kiểm chứng. Cần nói thẳng: lõi giá trị của hệ thống (tính toán, quản lý hộ dân) là bài toán nghiệp vụ/giao dịch, không phải sở trường mặc định của một CMS — xem rủi ro kỹ thuật trong addendum.

## Who This Serves

**Cán bộ lập phương án (vai trò chính, MVP)**
Người trực tiếp kiểm đếm đất/cây trồng/tài sản ngoài thực địa và nhập liệu vào hệ thống. Cần: nhập nhanh, đúng công thức tự động tính ra số tiền, không phải tự dò Excel; làm việc được trên di động/tablet ngoài hiện trường. Thành công với họ = nhập một lần, ra số đúng ngay, không phải kiểm tra lại công thức thủ công.

**Lãnh đạo (vai trò chính, MVP)**
Theo dõi tiến độ và số liệu tổng hợp của (các) dự án được phân quyền xem. Không nhập/sửa dữ liệu. Thành công với họ = có bức tranh tổng hợp chính xác, tức thời, không phải chờ cán bộ tổng hợp Excel gửi qua.

**Hộ dân (ngoài phạm vi MVP)**
Tra cứu tiến độ/số tiền bồi thường của mình — để ở giai đoạn sau.

## Success Criteria

- **Độ chính xác:** số liệu hệ thống tính ra khớp 100% với cách tính tay/Excel hiện tại trên dữ liệu thực tế của 2 dự án đã có (Lê Lợi, Đức Minh - Thuận An) — đây là tiêu chí nghiệm thu định lượng, kiểm bằng đối chiếu trực tiếp.
- **Tốc độ:** thời gian từ nhập liệu kiểm đếm đến có bảng tổng hợp hoàn chỉnh giảm rõ rệt so với quy trình Excel thủ công hiện tại.
- **Mức độ tin dùng hàng ngày:** cán bộ dùng hệ thống này thay vì quay lại Excel — tín hiệu thành công thực chất là hệ thống trở thành công cụ làm việc mặc định, không phải chỉ dùng để đối chiếu.
- **Thắng thầu:** hồ sơ chào thầu đủ sức thuyết phục Ban QLDA về tính khả thi, độ chính xác nghiệp vụ và thời gian triển khai.

## Scope

**Trong phạm vi MVP:**
- Quản lý nhiều dự án GPMB song song, mỗi dự án có bộ đơn giá đất/cây trồng/xây dựng và danh sách hộ dân riêng.
- Nhập liệu kiểm đếm theo hộ dân, đầy đủ 7 mục (I–VII) theo cấu trúc đã phân tích.
- Engine tính toán tự động: bồi thường đất, bồi thường cây trồng, bồi thường tài sản/vật kiến trúc, tổng hợp toàn hộ và toàn dự án (bao gồm chi phí dự phòng/tổ chức thực hiện).
- Xuất bảng tổng hợp dạng Excel/PDF theo hộ và toàn dự án.
- Phân quyền theo dự án: Cán bộ (xem+sửa dự án được gán), Lãnh đạo (chỉ xem dự án được gán).
- Giao diện web responsive: dùng tốt trên di động, tablet, desktop (đặc biệt quan trọng cho nhập liệu ngoài thực địa).

**Ngoài phạm vi MVP (rõ ràng loại trừ):**
- Tự động soạn thảo văn bản Phương án bồi thường hoàn chỉnh (đúng thể thức hành chính, kèm căn cứ pháp lý cập nhật theo luật/nghị định/quyết định) — cán bộ vẫn soạn thủ công, dùng số liệu xuất ra từ hệ thống.
- Workflow phê duyệt nhiều cấp (submit → duyệt → phản hồi).
- Cổng tra cứu cho hộ dân.
- Bảo mật/mã hoá nâng cao cho dữ liệu nhạy cảm (CCCD, tài sản cá nhân) — xem mục "Nâng cao" bên dưới.

**Nâng cao (optional, tính vào ước lượng nhưng không chặn MVP):**
- Tăng cường bảo mật dữ liệu cá nhân nhạy cảm (mã hoá, kiểm soát truy cập chi tiết hơn, nhật ký truy cập).

## Ước lượng Thời gian & Nguồn lực (sơ bộ)

*Ước lượng ở mức brief — mang tính định hướng cho hồ sơ chào thầu, sẽ chính xác hơn sau khi có Architecture và Epics/Stories chi tiết (qua workflow Sprint Planning của BMad). Giả định đội ngũ 1–2 người, có tận dụng Umbraco để tăng tốc phần content/backoffice.*

| Giai đoạn | 1 người | 2 người |
|---|---|---|
| Phân tích yêu cầu + tài liệu (PRD, UX, Architecture) | 2.5–3 tuần | 2–2.5 tuần |
| Thi công (setup Umbraco/RBAC, data model đa dự án, engine tính toán đất/cây/tài sản, UI nhập liệu responsive, xuất báo cáo) | 10–13 tuần | 7–9 tuần |
| Kiểm thử (đối chiếu công thức với Excel gốc, UAT với dữ liệu thật của 2 dự án đã có) | 1.5–2 tuần | 1.5–2 tuần |
| Triển khai (deploy, đào tạo cán bộ) | 0.5–1 tuần | 0.5–1 tuần |
| **Tổng** | **~16–19 tuần (~4–4.5 tháng)** | **~11–14 tuần (~2.5–3.5 tháng)** |

Phần chiếm nhiều thời gian nhất là **engine tính toán** (~2.5–3.5 tuần riêng phần này) do có nhiều công thức khác nhau theo loại đất/cây trồng/tài sản, mỗi loại có hệ số/tỉ lệ riêng — đây cũng là phần quyết định tiêu chí thành công "độ chính xác 100%" nên không nên rút ngắn ẩu.

## Vision

Nếu thành công, hệ thống trở thành công cụ vận hành chuẩn của Ban QLDA cho mọi dự án GPMB, không chỉ 2 dự án khởi điểm. Hướng mở rộng tự nhiên theo thời gian: tự động soạn thảo văn bản phương án có căn cứ pháp lý cập nhật, workflow phê duyệt nhiều cấp đúng quy trình hành chính, cổng tra cứu cho hộ dân, tích hợp bản đồ địa chính/GIS để hiển thị trực quan thửa đất bị thu hồi, và bảo mật dữ liệu cá nhân đạt chuẩn cho dữ liệu nhạy cảm của công dân.
