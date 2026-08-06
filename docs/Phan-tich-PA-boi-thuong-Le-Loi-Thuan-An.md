# Phân tích Phương án Bồi thường, Hỗ trợ Giải phóng Mặt bằng
### File nguồn: `PA LÊ LỢI THUẬN AN 12.5.2026 kèm theo QĐ.xlsx`
### Dự án: Đường Lê Lợi — Xã Thuận An, Tỉnh Lâm Đồng

---

## 0. Cấu trúc file

File gồm 3 sheet:

| Sheet | Nội dung | Kích thước |
|---|---|---|
| `28 HỘ BT TÀI SẢN` | Phương án bồi thường chi tiết cho 28 hộ dân, mỗi hộ 1 khối dữ liệu lặp lại theo cùng cấu trúc | A1:Z1079 |
| `DANH SÁCH` | Danh sách 28 hộ dân (STT, họ tên/CCCD, địa chỉ, ghi chú) | A1:D31 |
| `2 HỘ CÓ ĐẤT` | Phương án bồi thường cho 2 hộ khác (ông Lê Tấn Huy, ông Nguyễn Hữu Thanh) | A1:T94 |

Mỗi phương án của 1 hộ dân được trình bày theo 7 mục cố định (I → VII):

- **I.** Thông tin về người có đất bị thu hồi, chủ sở hữu tài sản
- **II.** Phương án bồi thường về đất
- **III.** Phương án bồi thường về cây trồng
- **IV.** Phương án bồi thường về tài sản, vật kiến trúc
- **V.** Chính sách hỗ trợ
- **VI.** Phương án tái định cư
- **VII.** Tổng tiền bồi thường, hỗ trợ, tái định cư

---

## 1. Mục I — Thông tin người có đất bị thu hồi / chủ sở hữu tài sản

**Các trường dữ liệu (input):**

- Họ tên, số CCCD, ngày cấp (của các đồng sở hữu nếu có)
- Địa chỉ thường trú, số điện thoại
- Tổng diện tích đất đang sử dụng (m²), tách đất ở / đất nông nghiệp
- Tổng diện tích thửa đất bị thu hồi (m²), tách đất ở / nông nghiệp
- Diện tích đất **thực tế** bị thu hồi (m²), tách đất ở / nông nghiệp
- Tỉ lệ thu hồi đất ở, tỉ lệ thu hồi đất nông nghiệp
- Có trực tiếp sản xuất nông nghiệp hay không
- Nguồn gốc thửa đất (nhận tặng cho, nhận chuyển nhượng, tự khai hoang...)
- Tình trạng sử dụng (ổn định / ranh giới có thay đổi)
- Tình trạng tranh chấp
- Hiện trạng (đang ở / đang canh tác trên đất)
- Số Giấy CNQSD đất, ngày cấp; ghi chú

**Công thức:**

| Cột | Ý nghĩa | Công thức |
|---|---|---|
| D | Tổng diện tích đang sử dụng | `D = E (đất ở) + F (đất NN)` |
| N | Tỉ lệ thu hồi đất nông nghiệp | `N = (L / F) × 100%` (diện tích NN thu hồi / tổng diện tích NN đang sử dụng) |

---

## 2. Mục II — Bồi thường về ĐẤT

**Loại đất xuất hiện trong file:** Đất ở tại nông thôn / Đất ở nông thôn, Đất trồng cây lâu năm (CLN), Đất sản xuất kinh doanh.

**Input cần có:**
- Mục đích sử dụng thửa đất bị thu hồi
- Vị trí thửa đất (số thửa, tờ bản đồ, mảnh trích đo, diện tích đo đạc chỉnh lý)
- Diện tích đủ điều kiện bồi thường: đã có GCNQSD / chưa có GCNQSD
- Diện tích không được bồi thường + nguyên nhân
- Tỉ lệ bồi thường (%), hệ số chiều sâu/lô góc
- **Đơn giá đất** theo Quyết định giá đất hiện hành (VD: QĐ số 2920/QĐ-UBND ngày 31/7/2024 của UBND huyện Đắk Mil)
- Khấu trừ nghĩa vụ tài chính (nếu có)

**Công thức:**

| Cột | Ý nghĩa | Công thức |
|---|---|---|
| K | Tổng khối lượng đất | `K = I (đã có GCN) + J (chưa có GCN)` |
| Q | Thành tiền | `Q = ROUND(P × N × K, -3)` (Đơn giá × Tỉ lệ BT% × Khối lượng, làm tròn nghìn) |
| R | Khấu trừ nghĩa vụ tài chính | `R = -ROUND(38.000 × J × 0,5%, -3)` |
| S | **Thành tiền bồi thường đất** | `S = Q + R` |

---

## 3. Mục III — Bồi thường về CÂY TRỒNG

**Danh mục cây trồng xuất hiện trong file:**
- Cây lâu năm/ăn quả: sầu riêng, mắc ca, chuối, mít, chanh, mận, lựu, đu đủ, thanh long, dừa (cao/lùn cảnh), bơ (ghép), mãng cầu, gòn, chè xanh...
- Cây công nghiệp: tiêu (trụ sống/bê tông/gỗ), chè
- Cây cảnh: mai vàng/cảnh, cau (vua/cảnh/lấy quả), sanh, lộc vừng, phát tài, đào cảnh, huyền điệp, quất, đinh lăng...
- Hoa màu: bắp

**Input cần có:**
- Hạng mục cây trồng, giai đoạn (kiến thiết cơ bản / kinh doanh, năm thứ mấy)
- Phân loại A/B/C theo biên bản xác định
- Nguồn gốc, năm trồng
- Khối lượng ảnh hưởng / khối lượng được bồi thường, đơn vị tính (cây/m²)
- Mật độ trồng chuẩn (m²/cây hoặc số cây/ha) theo quyết định
- **Đơn giá cây trồng** (theo QĐ 26/2024 hoặc QĐ đơn giá cây trồng hiện hành)
- Tỉ lệ bồi thường (%)

**Công thức:**

| Cột | Ý nghĩa | Công thức |
|---|---|---|
| J | Thời kỳ KTCB/kinh doanh (năm thứ) | `J = năm_hiện_tại − I(năm trồng) − offset` (offset = 3, 4 hoặc 5 tuỳ loại cây) |
| N | Mật độ (m²/cây) | `N = 10.000 / mật_độ_cây_trên_ha` (VD: `10000/1500`, `10000/1750`) |
| L | Khối lượng bồi thường | `L = K` (khối lượng ảnh hưởng, khi BT 100%) |
| Q | Thành tiền | `Q = ROUND(L × P, -3)` (Khối lượng BT × Đơn giá cây) |
| S | **Thành tiền BT cây** | `S = ROUND(Q × R, -3)` (R = tỉ lệ BT %) |

---

## 4. Mục IV — Bồi thường về TÀI SẢN, VẬT KIẾN TRÚC

**Danh mục tài sản/vật kiến trúc xuất hiện trong file:**
Nhà ở / Nhà tôn / Nhà (quán), Hàng rào / Tường rào (+ sắt thoáng), Trụ cổng, Cổng sắt (kín/thoáng), Mái (chữ A / vòm / che), Sân (xi măng/bê tông/gạch bát tràng), Bể nước (+ nắp đậy), Bờ kè, Tấm đan BTCT, Vách ngăn, Cống bi, Giếng đào, Lan can, Bó vỉa, Am thờ, Bảng hiệu, Bó vỉa, Cầu đổ BTCT...

**Input cần có:**
- Hạng mục + mô tả kết cấu chi tiết
- Đơn vị tính (m², m³, cái...)
- Năm xây dựng, thời gian trích khấu hao theo quy định, thời gian đã sử dụng
- Nguồn gốc tài sản (tự xây dựng...)
- Khối lượng/số lượng ảnh hưởng, khối lượng không bồi thường, khối lượng được bồi thường
- Hệ số khu vực
- **Đơn giá** theo bảng đơn giá xây dựng mới (VD: QĐ 32/2024)
- Tỉ lệ giảm trừ (%) do thiếu kết cấu so với kết cấu chuẩn trong đơn giá

**Công thức:**

| Cột | Ý nghĩa | Công thức |
|---|---|---|
| G | Thời gian đã sử dụng | `G = năm_kiểm_đếm − E(năm xây dựng)` |
| I | Khối lượng ảnh hưởng | Công thức hình học đo thực tế: `dài × rộng` (m²), `dài × rộng × cao` (m³), hình thang `0,5×(đáy lớn+đáy nhỏ)×cao`, hoặc tổng nhiều đoạn cộng lại — lấy từ số đo kiểm đếm thực địa |
| K | Khối lượng bồi thường | `K = I − J` (ảnh hưởng − không được BT) |
| O | Đơn giá sau giảm trừ thiếu kết cấu | `O = M × (hệ số thiếu kết cấu)` — VD: `M×80%×90%` (thiếu trụ BTCT 80%, thiếu giằng BTCT 90%), `M×90%`, `M×55%`, `M×90%×80%×70%`... |
| P | Nguyên giá | `P = ROUND(K × L × O × N, -3)` (Khối lượng BT × Hệ số khu vực × Đơn giá sau giảm trừ × Tỉ lệ%) |
| S | **Mức bồi thường** | thường `S = P`; một số trường hợp so sánh với giá trị hiện có × 30% theo khấu hao rồi lấy giá trị lớn hơn |

---

## 5. Mục V & VI — Hỗ trợ và Tái định cư

Trong file mẫu này: cả 2 mục đều ghi **"KHÔNG"** cho tất cả các hộ. Nếu có áp dụng, input cần thêm:
- Loại hỗ trợ (di chuyển, ổn định đời sống, đào tạo nghề, thưởng tiến độ...) và mức hỗ trợ theo quy định
- Vị trí, diện tích, hình thức tái định cư (nếu có)

---

## 6. Mục VII — Tổng hợp

**Theo từng hộ:**
```
U(hộ) = Tổng tiền đất + Tổng tiền cây trồng + Tổng tiền tài sản của hộ đó
```

**Tổng hợp toàn dự án** (cuối sheet `28 HỘ BT TÀI SẢN`, dòng ~1052–1060):
```
C1057 = Tổng tiền ĐẤT của toàn bộ 28 hộ (cộng dồn từng hộ)
C1058 = Tổng tiền TÀI SẢN của toàn bộ 28 hộ
C1054 = SUM(C1055:C1059)          → Tổng chi phí bồi thường, hỗ trợ
C1060 = ROUND(C1054 × 2,2%, -3)   → Chi phí dự phòng/tổ chức thực hiện (2,2%)
C1052 = C1054 + C1060             → TỔNG KINH PHÍ GPMB TOÀN DỰ ÁN
```

---

## 7. Tổng hợp bộ INPUT cần chuẩn bị để tự sinh ra phương án

| Nhóm | Input cụ thể |
|---|---|
| Hồ sơ nhân thân | CCCD, địa chỉ, số điện thoại của hộ dân |
| Hồ sơ đất đai | Giấy CNQSD đất, số thửa/tờ bản đồ, kết quả đo đạc/trích đo hiện trạng, nguồn gốc đất, tình trạng tranh chấp |
| Kiểm đếm đất | Diện tích đang sử dụng, diện tích thu hồi (tách đất ở/NN), tỉ lệ BT, đơn giá đất theo QĐ hiện hành |
| Kiểm đếm cây trồng | Loại cây, năm trồng, số lượng/diện tích, mật độ chuẩn theo QĐ, đơn giá cây theo QĐ |
| Kiểm đếm tài sản/vật kiến trúc | Loại kết cấu, kích thước đo đạc (dài/rộng/cao), năm xây dựng, đơn giá xây dựng mới theo QĐ, % kết cấu thiếu so với chuẩn |
| Chính sách | Chính sách hỗ trợ & tái định cư áp dụng cho dự án (nếu có) |
| Tham số chung | Năm tính khấu hao hiện tại, hệ số khu vực, % chi phí dự phòng (2,2%), đơn giá 38.000đ/m² tính nghĩa vụ tài chính |
| Thông tin dự án | Tên dự án, địa điểm, số quyết định/báo cáo phê duyệt kèm theo |

---

*Tài liệu này được tổng hợp từ phân tích cấu trúc và công thức thực tế trong file Excel `PA LÊ LỢI THUẬN AN 12.5.2026 kèm theo QĐ.xlsx`.*
