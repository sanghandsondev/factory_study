# 📘 Ôn tập Kiến thức Điện – Điện tử cho Phỏng vấn Nhà máy EMS (Luxshare-ICT / Foxconn)

> Dành cho người có nền **Cơ điện tử (BK Hà Nội)** đã chuyển sang **C++ Automotive (App/Middleware)**, cần ôn lại kiến thức điện tử thời sinh viên + bổ sung kiến thức sản xuất.
>
> Cách dùng: đọc lướt toàn bộ 1 lần để nhớ khung → focus phần ⭐ (hay hỏi nhất) → tự trả lời to thành lời như đang phỏng vấn.

---

## 🎯 0. Chiến lược ôn tập (đọc trước khi học)

**Luxshare & Foxconn là EMS/ODM** — sản xuất theo hợp đồng. Trọng tâm phỏng vấn thường KHÔNG phải thiết kế mạch từ đầu, mà là:
- Hiểu **linh kiện & mạch cơ bản** (để đọc hiểu, debug lỗi sản xuất).
- Hiểu **quy trình sản xuất SMT/PCBA** và **các công đoạn test**.
- **Tư duy chất lượng**: yield, defect, root-cause, 8D, DFM.

👉 **Lợi thế của bạn**: nền C++ + Automotive + Linux/embedded rất mạnh cho các vị trí **Test Engineer, Process/Equipment Engineer, Automation, Software/Firmware, MES/AOI programming**. Hãy "kể chuyện" xoay quanh: *"Tôi hiểu cả phần cứng lẫn phần mềm, có thể viết tool test / phân tích log / tự động hóa."*

| Nhóm vị trí | Họ kỳ vọng gì | Bạn nên nhấn mạnh |
|---|---|---|
| Test / Quality Engineer | ICT, FCT, AOI, phân tích lỗi | C++ để viết test script, đọc log, thống kê |
| Process / Equipment | Hiểu SMT, máy móc, thông số | Tư duy hệ thống, debug, tối ưu |
| Automation / SW / MES | Lập trình, DB, tích hợp | C#, C++, Python, SQL, Linux |
| Firmware / Embedded | Vi điều khiển, giao tiếp | Nền embedded/Yocto/glibc của bạn |

---

## ⭐ 1. Điện học cơ bản (nền tảng bắt buộc)

### 1.1. Định luật Ohm & Công suất
- **Định luật Ohm**: `U = I × R` (Điện áp = Dòng × Trở kháng).
- **Công suất**: `P = U × I = I²R = U²/R` (đơn vị W).
- Nhớ nhanh tam giác: che đại lượng cần tìm ⇒ ra công thức.

### 1.2. Mắc nối tiếp vs song song
| | Nối tiếp (Series) | Song song (Parallel) |
|---|---|---|
| Trở kháng R | `R = R1 + R2 + ...` | `1/R = 1/R1 + 1/R2 + ...` |
| Dòng điện I | Bằng nhau qua mọi phần tử | Chia theo nhánh |
| Điện áp U | Chia theo từng phần tử | Bằng nhau trên mọi nhánh |

- **Tụ điện** thì NGƯỢC lại với điện trở: song song thì cộng `C = C1+C2`, nối tiếp thì nghịch đảo.

### 1.3. Định luật Kirchhoff (KCL & KVL)
- **KCL (dòng)**: Tổng dòng vào một nút = tổng dòng ra (bảo toàn điện tích).
- **KVL (áp)**: Tổng điện áp quanh một vòng kín = 0 (bảo toàn năng lượng).
- Đây là bộ đôi để giải mọi mạch → hay được hỏi để test tư duy.

### 1.4. AC vs DC
- **DC (một chiều)**: dòng/áp không đổi chiều (pin, nguồn cấp board).
- **AC (xoay chiều)**: đổi chiều tuần hoàn (điện lưới 220V/50Hz VN).
- **RMS (giá trị hiệu dụng)**: `Vrms = Vpeak / √2` cho sóng sin. "220V" chính là RMS.

### 1.5. ⭐ Điện 1 pha vs 3 pha (hay hỏi ở nhà máy)
- **Điện 3 pha** = 3 dòng AC cùng tần số, cùng biên độ, lệch pha nhau **120°** → truyền tải công suất lớn ổn định, ít hao hụt hơn, chạy được động cơ/máy công suất cao.

| | Điện 1 pha | Điện 3 pha |
|---|---|---|
| Số dây | 2 (1 nóng + 1 lạnh) | 4 (3 nóng + 1 lạnh) |
| Điện áp (VN) | 220V | 380V |
| Mục đích | Thiết bị sinh hoạt (đèn, quạt, TV, tủ lạnh) | Sản xuất công nghiệp, nhà xưởng, động cơ lớn |
| Truyền tải | Công suất nhỏ, dễ sụt áp khi kéo xa/tải lớn | Công suất lớn, đi xa ổn định & hiệu quả |

- Đấu nối 3 pha thường theo kiểu **hình sao (Y)** hoặc **tam giác (Δ)**; hộ kinh doanh/nhà xưởng muốn dùng phải xin cấp điện 3 pha từ điện lực.

---

## ⭐ 2. Linh kiện điện tử cơ bản (hay hỏi định nghĩa + ứng dụng)

### 2.1. Linh kiện thụ động (Passive)
- **Điện trở (R)**: hạn dòng, chia áp, pull-up/pull-down. Đọc theo **vạch màu** hoặc mã số (SMD: 103 = 10×10³ = 10kΩ).
- **Tụ điện (C)**: tích trữ điện tích, lọc nguồn (decoupling), chặn DC cho AC qua. `Q = C×V`. Trở kháng `Xc = 1/(2πfC)` → tần số cao thì tụ "dẫn" tốt.
- **Cuộn cảm (L)**: chống lại thay đổi dòng, lọc, dùng trong nguồn switching. `Xl = 2πfL` → tần số cao thì cản mạnh.

### 2.2. Linh kiện bán dẫn (Active)
- **Diode**: cho dòng đi 1 chiều. Ứng dụng: **chỉnh lưu** (AC→DC), bảo vệ ngược cực, LED, Zener (ổn áp).
- **Cầu chỉnh lưu (Bridge rectifier)**: 4 diode → chỉnh lưu 2 nửa chu kỳ.
- **Transistor (BJT / MOSFET)**: 2 vai trò chính:
  - **Khuếch đại** (analog).
  - **Đóng/ngắt – switch** (digital, điều khiển tải). ⭐ Nhà máy quan tâm vai trò switch.
  - BJT điều khiển bằng **dòng** (base), MOSFET điều khiển bằng **áp** (gate) → MOSFET tiết kiệm năng lượng, dùng nhiều trong nguồn.
- **IC (Integrated Circuit)**: op-amp, vi điều khiển, ADC/DAC, nguồn (LDO, buck/boost)...

### 2.3. Câu hỏi bẫy hay gặp
- "Tụ khác pin thế nào?" → Tụ tích **điện trường**, xả rất nhanh; pin tích **hóa năng**, xả chậm & lâu.
- "Pull-up resistor để làm gì?" → Giữ mức logic xác định (HIGH) khi chân thả nổi, tránh nhiễu.
- "Op-amp lý tưởng?" → Trở kháng vào ∞, trở kháng ra 0, độ lợi ∞; 2 quy tắc vàng: không có dòng vào ngõ vào, hai ngõ vào bằng nhau (khi có hồi tiếp âm).

---

## ⭐ 3. Analog vs Digital (rất hay hỏi)

| | Analog | Digital |
|---|---|---|
| Tín hiệu | Liên tục (∞ mức) | Rời rạc (0/1) |
| Ví dụ | Âm thanh, nhiệt độ, cảm biến | CPU, bộ nhớ, logic |
| Ưu điểm | Chi tiết, tự nhiên | Chống nhiễu, dễ lưu/xử lý |
| Nhược | Dễ nhiễu, khó lưu | Cần lấy mẫu, mất chi tiết |

- **ADC** (Analog→Digital) & **DAC** (Digital→Analog): cầu nối 2 thế giới. Cảm biến (analog) → ADC → vi xử lý (digital).
- **Định lý lấy mẫu Nyquist**: tần số lấy mẫu ≥ 2× tần số cao nhất của tín hiệu.
- **Cổng logic cơ bản**: AND, OR, NOT, NAND, NOR, XOR. NAND/NOR là "cổng vạn năng" (làm được mọi hàm).
- **Hệ đếm**: nhị phân, hex. Biết đổi qua lại (VD: 1010₂ = A₁₆ = 10₁₀).

---

## ⭐ 4. PCB & Quy trình sản xuất SMT (điểm cộng LỚN cho EMS)

> Đây là "sân nhà" của Luxshare/Foxconn. Hiểu phần này = ghi điểm mạnh dù bạn học phần mềm.

### 4.1. PCB là gì?
- **Printed Circuit Board** – bảng mạch in, "khung xương" gắn & kết nối linh kiện bằng đường đồng (trace).
- Cấu tạo: lõi **FR-4** (sợi thủy tinh + epoxy), lớp đồng, solder mask (lớp xanh), silkscreen (chữ in).
- Loại: 1 lớp, 2 lớp, đa lớp (multilayer), HDI, mềm (flex/FPC).

### 4.2. SMT vs THT
- **SMT (Surface Mount Technology)**: linh kiện dán trực tiếp lên bề mặt → nhỏ, nhanh, tự động hóa cao. Chủ lực hiện đại.
- **THT (Through-Hole)**: chân linh kiện xuyên lỗ → chắc chắn cơ khí (connector lớn, linh kiện công suất).
- Đa số board hiện đại = **kết hợp cả hai** (SMT chính + THT cho phần chịu lực).

### 4.3. ⭐ Quy trình dây chuyền SMT (thuộc lòng thứ tự!)
Foxconn mô tả 3 bước lõi: **In kem hàn → Gắp đặt → Hàn reflow**. Chi tiết đầy đủ 8 bước:

1. **Solder Paste Printing** – In kem hàn qua khuôn thép (stencil) lên pad.
2. **SPI (Solder Paste Inspection)** – Kiểm tra lượng/vị trí kem hàn.
3. **Pick & Place** – Máy gắp đặt linh kiện (tốc độ hàng chục nghìn linh kiện/giờ).
4. **Reflow Soldering** – Lò nung theo profile nhiệt (VD SAC305 ~217°C) làm chảy kem hàn → mối hàn.
5. **AOI (Automated Optical Inspection)** – Camera kiểm tra lỗi đặt/hàn.
6. **Manual Inspection** – Người kiểm tra lỗi máy bỏ sót.
7. **Functional Testing** – Kiểm tra chức năng điện.
8. **Final QC & Packaging** – Kiểm tra cuối, đóng gói.

*(Nguồn quy trình: NextPCB, JLCPCB, Foxconn.)*

### 4.4. ⭐ Các lỗi hàn SMT kinh điển (hay hỏi!)
| Lỗi | Mô tả | Nguyên nhân thường gặp |
|---|---|---|
| **Tombstoning** (bia mộ) | Linh kiện 2 chân dựng đứng 1 đầu | Lực căng bề mặt lệch, nhiệt không đều |
| **Bridging** (cầu chì) | Chì nối tắt 2 pad | Thừa kem hàn, stencil lỗi |
| **Insufficient solder** | Thiếu chì | Thiếu kem, pad bẩn |
| **Voiding** | Bọt khí trong mối hàn | Khí không thoát, profile sai |
| **Skew / shift** | Linh kiện lệch | Đặt sai, kem lệch |
| **Cold joint** | Mối hàn nguội, xỉn | Nhiệt reflow chưa đủ |

### 4.5. ⭐ Kỹ năng hàn (Soldering) – rất dễ bị hỏi tay nghề thực tế

> Nếu bị yêu cầu "hàn thử" hoặc hỏi lý thuyết hàn, đây là phần cần thuộc.

**a) Phân loại hàn trong nhà máy**

| Loại hàn | Khi nào dùng | Đặc điểm |
|---|---|---|
| **Hàn tay (Manual/Hand soldering)** | Rework/sửa lỗi sau AOI-ICT, gắn linh kiện to (connector, jack) không qua wave được, làm mẫu (prototype), gắn dây jump wire | Linh hoạt, cần tay nghề, năng suất thấp |
| **Hàn sóng (Wave soldering)** | Hàn hàng loạt chân THT (xuyên lỗ) | Nhúng board qua sóng thiếc nóng chảy |
| **Hàn reflow** | Hàn hàng loạt linh kiện SMT | Lò nhiệt theo profile (xem mục 4.3) |
| **Hàn chọn lọc (Selective soldering)** | Board vừa có SMT vừa có THT | Chỉ hàn đúng điểm cần, tránh ảnh hưởng nhiệt vùng SMT đã hàn trước |
| **Hàn khò/hot air rework** | Tháo/gắn lại linh kiện SMD, BGA bị lỗi | Dùng súng khò khí nóng + kem hàn, cần trạm rework chuyên dụng |

**b) Dụng cụ hàn tay cơ bản**
- **Mỏ hàn (soldering iron)**: có điều chỉnh nhiệt độ, đầu mỏ (tip) tiếp đất chống ESD.
- **Thiếc hàn (solder wire)**: loại có chì (Sn63/Pb37, chảy ~183°C) hoặc không chì/lead-free (SAC305, chảy ~217°C) theo chuẩn RoHS.
- **Nhựa thông/Flux**: giúp làm sạch oxit, thiếc bám đều hơn.
- **Dụng cụ hỗ trợ**: hút thiếc (solder sucker/pump), dây bện hút thiếc (solder wick/braid) để tháo mối hàn cũ, kẹp gắp linh kiện, kính lúp/kính hiển vi cho linh kiện nhỏ.

**c) Các bước hàn tay cơ bản (5 bước)**
1. Chuẩn bị: làm sạch chân linh kiện & pad, cố định linh kiện đúng vị trí.
2. Làm nóng mối nối: áp đầu mỏ hàn vào **cả chân linh kiện và pad** cùng lúc (không áp vào thiếc).
3. Đưa thiếc vào: chạm dây thiếc vào điểm tiếp xúc (không đưa vào đầu mỏ hàn) để thiếc chảy đều.
4. Rút thiếc ra trước, sau đó rút mỏ hàn ra sau khi đủ lượng.
5. Giữ yên linh kiện vài giây để mối hàn nguội tự nhiên, không thổi hay lay động.

**d) Tiêu chuẩn mối hàn tốt (theo IPC-A-610)**
- Hình **nón/phễu (fillet)** sáng bóng, thiếc bao đều quanh chân, không quá nhiều/quá ít.
- Không có: gai nhọn (icicle), rỗ khí (void), mối hàn nguội xỉn màu (cold joint), cầu chì dính 2 chân (bridging).
- Chân linh kiện vẫn nhận diện được hình dạng qua lớp thiếc (không bị "chôn" hoàn toàn).

**e) Lưu ý an toàn & chất lượng**
- Luôn đeo **vòng tay chống tĩnh điện (ESD)**, làm việc trên bàn/thảm ESD để tránh phóng tĩnh điện hỏng IC.
- Hàn không chì cần nhiệt độ cao hơn hàn có chì (~350–370°C so với ~330–350°C).
- Thông gió tốt, tránh hít khói hàn; không chạm tay trực tiếp vào đầu mỏ hàn.

### 4.6. ⭐ Đọc bản vẽ & tài liệu sản xuất SMT (Data Package)

> Đây là câu hỏi rất hay gặp cho vị trí Process/NPI/Test/SMT Engineer: *"Bạn có biết đọc bản vẽ SMT không? Kể tên các file trong bộ tài liệu sản xuất PCBA?"*

Một bộ tài liệu (Data Package) chuẩn giao cho nhà máy EMS gồm:

| File / Tài liệu | Nội dung | Ai dùng |
|---|---|---|
| **Schematic** (sơ đồ nguyên lý) | Kết nối logic giữa các linh kiện; ký hiệu chuẩn (R1, C2, U3…) + net name (VCC, GND, D+, D-…) | Test/Debug/NPI engineer |
| **PCB Layout / Board file** | Bản vẽ vật lý: vị trí linh kiện, trace, via, các lớp đồng | Layout/NPI |
| **Gerber files** | Bộ file chuẩn công nghiệp mô tả từng lớp PCB để fab sản xuất board trần | Fab / PCB shop |
| **Drill file (Excellon)** | Tọa độ & đường kính lỗ khoan (via, mounting hole) | Fab |
| **BOM** (Bill of Materials) | Danh sách linh kiện: designator, giá trị, part number (MPN), package, số lượng, alternate part | Purchasing / SMT / IQC |
| **Pick & Place / Centroid / CPL** | CSV: designator, X-Y, góc xoay, layer (Top/Bottom) → máy P&P đọc để gắp đặt | SMT programming |
| **Assembly Drawing** | Ghi chú lắp ráp: hướng linh kiện phân cực (diode/IC/tụ hóa), DNP, hướng cắm connector | Line operator, IPQC |
| **Stencil file** | File chế tạo khuôn thép in kem hàn (thường lấy từ paste layer Gerber) | Stencil shop |
| **Fab Notes / Fab Drawing** | Số lớp, độ dày, vật liệu (FR-4 Tg170), impedance control, surface finish (HASL/ENIG/OSP), màu solder mask | Fab |

**Ý nghĩa các lớp Gerber phổ biến** (đuôi file có thể khác tùy tool):

| Lớp | Ký hiệu | Ý nghĩa |
|---|---|---|
| Top Copper | GTL | Đường đồng mặt trên |
| Bottom Copper | GBL | Đường đồng mặt dưới |
| Top / Bottom Solder Mask | GTS / GBS | Lớp phủ xanh chống hàn dính ngoài pad |
| Top / Bottom Silkscreen | GTO / GBO | Chữ trắng in ký hiệu linh kiện |
| Top / Bottom Paste | GTP / GBP | Vùng in kem hàn → dùng làm stencil |
| Board Outline | GKO / GML | Đường viền cắt board |
| Drill | TXT / NC / XLN | Tọa độ lỗ khoan |

**Cách đọc Schematic nhanh (mẹo phỏng vấn)**:
1. Tìm **power rails** trước (VCC, 3V3, 1V8, GND) — thường trên/dưới cùng.
2. Tìm **IC chính** (U1/U2 — MCU, SoC, PMIC) — trái tim của board.
3. Đi theo tín hiệu I/O: **connector (J) → protection (TVS/ferrite) → IC → tải**.
4. Chú ý **net name** (nhãn) — schematic ít khi vẽ hết dây, dùng label thay thế.
5. Chú ý ghi chú **DNP** (Do Not Populate) — linh kiện có in trên board nhưng KHÔNG lắp.

**Cách tìm 1 linh kiện thực tế trên board từ bản vẽ**:
- Có designator (VD: `R42`) → tra tọa độ X-Y trong **Pick & Place file** → nhìn silkscreen trên board (nhớ mirror khi lật sang Bottom).
- **Test Point (TP1, TP2…)** là điểm được cấp riêng cho việc đo/kích tín hiệu — kỹ sư test hay dùng.

### 4.7. ⭐ Reference Designator (ký hiệu linh kiện) — thuộc lòng

Theo chuẩn IEEE 315 / IPC — nhìn schematic hay BOM đều gặp:

| Ký hiệu | Linh kiện | Ghi chú |
|---|---|---|
| **R** | Resistor | Điện trở |
| **RN** | Resistor Network/Array | Nhiều điện trở trong 1 gói |
| **C** | Capacitor | Tụ (nhớ nhìn cực cho tụ hóa/tantalum) |
| **L** | Inductor | Cuộn cảm |
| **FB** | Ferrite Bead | Chặn nhiễu HF |
| **D** | Diode | Bao gồm LED, Zener, TVS |
| **Q** | Transistor | BJT hoặc MOSFET |
| **U** | IC (Integrated Circuit) | MCU, op-amp, PMIC, memory… |
| **Y** hoặc **X** | Crystal / Oscillator | Thạch anh, dao động |
| **J** | Connector / Jack | Cắm dây/cable |
| **P** | Plug | Đầu cắm rời |
| **SW** hoặc **S** | Switch | Nút/công tắc |
| **F** | Fuse | Cầu chì |
| **T** | Transformer | Biến áp |
| **K** | Relay | Rơ-le |
| **BT** | Battery | Pin |
| **TP** | Test Point | Điểm đo/kích |
| **MH** | Mounting Hole | Lỗ bắt vít |

Mẹo nhớ: **R**esistor, **C**ap, **L**inductor, **U**nit (IC), **Q** = transistor, **J**ack (connector), **TP** = test point, **FB** = ferrite bead.

### 4.8. ⭐ Package linh kiện SMT (kiểu chân) — hay hỏi kích thước & phương pháp kiểm tra

**a) Điện trở / tụ chip 2 chân** — mã inch = kích thước (LxW):
- **0201** = 0.6 × 0.3 mm (rất nhỏ — smartphone hi-end, đòi hỏi máy P&P chuẩn)
- **0402** = 1.0 × 0.5 mm (phổ biến trong board dày đặc)
- **0603** = 1.6 × 0.8 mm (phổ biến board dân dụng/công nghiệp)
- **0805**, **1206** — lớn hơn, chịu công suất/áp cao hơn

**b) Diode / Transistor rời**:
- **SOD-123**, **SOD-323**: diode nhỏ
- **SOT-23** (3 chân), **SOT-89**, **SOT-223**: transistor rời / regulator nhỏ
- **DPAK / TO-252**, **D2PAK / TO-263**: MOSFET / regulator công suất

**c) IC nhỏ – vừa** (chân lộ ra ngoài — AOI kiểm tra được):
- **SOIC** (Small Outline IC) — 2 hàng chân gull-wing
- **TSSOP / SSOP** — thinner/shrink SOIC
- **QFP / LQFP / TQFP** (Quad Flat Package) — 4 cạnh, chân gull-wing
- **QFN** (Quad Flat No-lead) — không chân, pad dưới bụng + pad quanh mép → **kiểm tra biên vẫn thấy**
- **DFN** — như QFN nhưng chỉ 2 cạnh

**d) IC lớn / bộ nhớ / SoC** (chân/bi ẩn dưới bụng):
- **BGA** (Ball Grid Array) — mảng bi hàn dưới bụng chip → **AOI KHÔNG nhìn được** → phải dùng **AXI (X-Ray)** để kiểm tra mối hàn.
- **LGA** (Land Grid Array) — pad phẳng dưới bụng
- **CSP / WLCSP** (Wafer-Level Chip-Scale Package) — kích thước gần bằng die silicon, cực nhỏ

**Câu hỏi bẫy hay gặp**:
- *"Lỗi hàn dưới BGA phát hiện bằng phương pháp nào?"* → **AXI / X-Ray** (AOI không nhìn xuyên được).
- *"0402 khác 0603 ở đâu?"* → Kích thước & độ dày đặc; 0402 nhỏ hơn ⇒ khó hàn tay, cần máy chuẩn, tiết kiệm diện tích board.
- *"Vì sao QFN/BGA cần thermal pad nối GND?"* → Tản nhiệt cho chip + giảm inductance ground.

---

## ⭐ 5. Kiểm tra & Test trong sản xuất (cực kỳ hay hỏi ở EMS)

| Phương pháp | Kiểm tra gì | Đặc điểm |
|---|---|---|
| **AOI** (Optical) | Lỗi nhìn thấy: thiếu linh kiện, lệch, cầu chì, sai cực | Nhanh, sau reflow; KHÔNG thấy mối hàn ẩn (BGA) |
| **AXI / X-Ray** | Mối hàn ẩn dưới linh kiện (BGA) | Nhìn xuyên, đắt hơn |
| **ICT** (In-Circuit Test) | Từng linh kiện & net: hở, chập, sai giá trị, sai cực | Dùng **bed-of-nails** (kim chích test point); phát hiện lỗi lắp ráp sớm |
| **FCT** (Functional Test) | Cả board hoạt động đúng như sản phẩm thật | Cấp nguồn, chạy thật, so với spec |
| **Burn-in Test** | Chạy ở nhiệt độ/tải cao thời gian dài | Loại lỗi "chết sớm" (infant mortality) |

**Câu chốt để nhớ**: *ICT hỏi "board có được lắp đúng không?", FCT hỏi "board có chạy đúng không?"*

**Điểm nhấn cho bạn**: FCT thường cần **viết phần mềm test** (C++/C#/Python) để cấp kích thích, đọc output, so sánh spec, log kết quả → đúng thế mạnh của bạn.

### 5.6. ⭐ Tư duy logic debug lỗi trên dây chuyền (câu hỏi tình huống hay gặp)

> Đây là câu quyết định điểm phỏng vấn cho vị trí Test/Process/Quality: nhà tuyển dụng muốn nghe **quy trình tư duy** chứ không phải câu trả lời may rủi.

**Quy trình chuẩn khi 1 board fail ở FCT/ICT** (nhớ theo thứ tự):

1. **Reproduce (tái hiện)** — chạy lại test 2–3 lần. Lỗi *ổn định* hay *ngẫu nhiên (intermittent)*? Ghi lại điều kiện (nhiệt độ, thời gian, thứ tự bước).
2. **Isolate (khoanh vùng)** —
   - Đọc log/error code: module nào fail (nguồn / RF / USB / storage / display)?
   - Nếu **cả 1 lô** cùng fail 1 loại lỗi → nghi ngờ **process** hoặc **lot vật tư**.
   - Nếu chỉ **1–2 board** → nghi ngờ **lỗi lắp ráp cá biệt** (hàn, đặt sai).
3. **Compare good vs bad** — đặt 1 board GOOD song song, đo cùng điểm ⇒ điện áp / tần số / dòng khác ở đâu? Đây là kỹ thuật tìm ra 80% lỗi trong 15 phút.
4. **Half-split (chia đôi)** — signal chain có N khối → đo ngay điểm GIỮA để chia đôi vùng nghi, lặp lại. Thuật toán O(log N) thay vì O(N).
5. **Top-down power-up** — thứ tự bắt buộc khi board "không lên":
   1. Đo VBUS/VIN vào
   2. Fuse (F1) còn thông không?
   3. Output của LDO/DC-DC (3V3, 1V8…) đủ không?
   4. Enable pin của chip nguồn có đúng mức?
   5. Clock (thạch anh) có dao động không?
   6. Reset chân RST đã nhả cao chưa?
   7. Strap/boot pin có đúng cấu hình?
6. **Đọc ngược từ hiện tượng** — VD: LED không sáng → kiểm nguồn LED → kiểm tín hiệu điều khiển (base/gate transistor) → kiểm firmware set GPIO.
7. **Root cause** — dùng **5-Why** hoặc **Fishbone (Ishikawa) 6M**: **M**an / **M**achine / **M**aterial / **M**ethod / **M**easurement / **E**nvironment (Mother nature).
8. **Verify & document (8D)** — xác nhận fix trên nhiều board, cập nhật SOP/traveler/AOI recipe, đóng vòng CAPA (Corrective/Preventive Action).

**Câu chốt phỏng vấn** (nói to lên khi trả lời):
> *"Tôi luôn tách 3 giả thuyết đầu tiên: **lỗi vật tư — lỗi lắp ráp — lỗi thiết kế**. So sánh 1 board tốt vs board xấu ở cùng điểm đo thường cho câu trả lời trong 15 phút, rồi mới quyết định có cần escalate lên R&D không."*

**Sai lầm nhà tuyển dụng hay test bạn**:
- Sửa mà không hiểu nguyên nhân → lỗi quay lại (không đóng được vòng 8D).
- Không **containment** lô đang chạy → lỗi lan rộng, thiệt hại nhân lên.
- Đổ lỗi cho công nhân trước khi kiểm jig/SOP/vật tư.
- Chỉ nhìn board fail, không so với board pass.
- Không log lại — lần sau gặp lại phải làm từ đầu.

---

## 🔧 6. Điện công nghiệp & Tự động hóa nhà máy (hỏi nếu vị trí liên quan)

- **Relay / Contactor**: đóng ngắt mạch bằng nam châm điện (contactor = relay công suất lớn cho động cơ).
- **PLC (Programmable Logic Controller)**: "bộ não" điều khiển dây chuyền. Lập trình bằng **Ladder Logic** (LD), hoặc ST/FBD. Điều khiển băng tải, robot, cảm biến, van...
- **Biến tần (Inverter/VFD)**: điều khiển tốc độ động cơ AC bằng cách thay đổi tần số.
- **Cảm biến (Sensor)**: quang (photoelectric), tiệm cận (proximity), nhiệt, áp suất → đầu vào cho PLC/hệ thống AOI.
- **Nguyên tắc an toàn điện**: cách ly nguồn (LOTO – Lock Out Tag Out), nối đất, thiết bị bảo hộ, tránh chạm 2 tay vào 2 cực.

---

## 🖥️ 7. Cầu nối Phần cứng ↔ Phần mềm (LỢI THẾ của bạn)

Nhấn mạnh những thứ này để khác biệt với ứng viên thuần điện tử:

- **MES (Manufacturing Execution System)**: hệ thống điều hành sản xuất — theo dõi lot, truy xuất nguồn gốc (traceability), thu thập dữ liệu máy. Thường code bằng C#/.NET, DB SQL Server → đúng hướng bạn đang học.
- **AOI programming**: cấu hình thư viện linh kiện, ngưỡng, xử lý ảnh → có thể liên quan Python/C++.
- **Giao tiếp thiết bị**: UART/RS-232, SPI, I²C, CAN (bạn đã quen từ Automotive!), Ethernet/TCP-IP, Modbus (công nghiệp).
- **Tự động hóa test**: viết script điều khiển thiết bị đo (SCPI qua GPIB/USB/LAN), thu log, phân tích thống kê (Cp/Cpk).
- **Firmware/Embedded**: nền Yocto/glibc/ARM của bạn rất giá trị cho vị trí embedded.

### 7.5. ⭐ Hiểu phần cứng — kiến thức "phải biết" cho Test/NPI/Debug

> Đây là phần thể hiện bạn KHÔNG chỉ code mà thực sự **hiểu board đang chạy như thế nào** — cực kỳ ghi điểm.

**a) Power rails & phân cấp nguồn**
- Một board điện tử thường có **nhiều mức điện áp**: VBUS 5V (USB) → **3V3** (I/O logic) → **1V8** (IO chip mới) → **1V2 / 1V0** (core CPU) — được tạo bởi **LDO** hoặc **DC-DC buck**.
- **LDO** (Low-Dropout Regulator): đơn giản, ít nhiễu, nhưng hiệu suất thấp (sinh nhiệt) khi sụt áp lớn.
- **Buck converter** (DC-DC): hiệu suất cao (>90%), phức tạp hơn, có nhiễu switching.
- **Nguyên tắc vàng của Test engineer**: khi board bất thường, **luôn đo rails trước** bằng đồng hồ hoặc scope.

**b) Decoupling capacitor ("tụ lọc nguồn cục bộ")**
- Mỗi chân VCC của IC thường có **1 tụ 100 nF** đặt SÁT chân + **1 tụ lớn 10 μF** dùng chung cho khối.
- Vai trò: giữ áp ổn định khi IC chuyển mạch nhanh, giảm nhiễu HF, giảm impedance đường nguồn ở tần số cao.
- **Câu hỏi bẫy**: *"Tại sao mỗi IC lại có nhiều tụ 100 nF chứ không dùng 1 tụ lớn?"* → Tụ nhỏ có ESL/ESR thấp ở tần số cao, tụ lớn hiệu quả ở tần số thấp — kết hợp để phủ dải rộng.
- **Vị trí đặt tụ**: càng SÁT chân IC càng tốt (giảm inductance đường dẫn).

**c) Ground plane & Signal Integrity cơ bản**
- **Ground plane** = 1 lớp đồng phủ kín làm đường về dòng nhiễu → giảm EMI, giảm crosstalk, ổn định impedance.
- Với tín hiệu tốc độ cao (**USB, HDMI, DDR, MIPI, Ethernet**):
  - **Impedance control**: single-ended ~50 Ω, differential ~90–100 Ω.
  - **Length matching**: chiều dài D+ và D− phải bằng nhau (thường ±5 mil) → nên có đoạn zig-zag ("serpentine") để bù chiều dài.
  - **Reference plane liên tục**: không được cắt ground bên dưới đường tín hiệu cao tốc.

**d) Clock, Reset, Boot flow** (rất hay hỏi cho vị trí embedded/debug)
- Mỗi MCU/SoC cần: **clock** (thạch anh Y1 + 2 tụ tải, hoặc oscillator nội) + **reset** (nút RST + IC supervisor / POR nội).
- **Boot flow chuẩn**:
  1. Reset nhả cao → 2. Clock ổn định → 3. Đọc **strap/boot pins** để chọn nguồn boot (ROM/Flash/SD/USB) → 4. Nạp bootloader → 5. Nạp OS/firmware.
- **Khi board "không boot"**: kiểm 4 bước đầu — VCC → clock → reset → strap pins. Đây là mẫu câu trả lời chuẩn.

**e) Cổng debug trên board — biết để tận dụng**
- **JTAG** (5 chân TCK/TMS/TDI/TDO/TRST) — chuẩn cũ, mạnh, dùng cho chain nhiều chip.
- **SWD** (2 chân SWDIO + SWCLK) — chuẩn ARM Cortex, gọn, dùng với ST-Link/J-Link.
- **UART console** (3 chân TX/RX/GND, thường 115200-8N1) — hầu hết embedded Linux board có, in log boot.
- Các chân này thường lộ ra ở **Test Point (TP)** hoặc header nhỏ → kỹ sư test/firmware hay câu để debug.

**f) Bảo vệ mạch — linh kiện "vệ sĩ"**
- **TVS diode** (Transient Voltage Suppressor): chống xung ESD/surge trên đường I/O (USB, HDMI…).
- **Ferrite bead (FB)**: chặn nhiễu HF trên đường nguồn (thường ở lối vào VCC).
- **Fuse (F1)** hoặc **PTC resettable (polyfuse)**: cắt dòng khi ngắn mạch/quá tải.
- **Reverse polarity protection**: diode nối tiếp hoặc MOSFET P kênh cắm ngược.
- **Câu hỏi bẫy**: *"Ferrite bead khác cuộn cảm ở đâu?"* → Bead có Q thấp, biểu hiện như trở kháng ở HF ("lossy"), tiêu tán năng lượng nhiễu thành nhiệt; cuộn cảm tích trữ năng lượng.

**g) Câu hỏi phỏng vấn phần cứng thường gặp**
- *"Board không lên nguồn, checklist đo?"* → Đo VBUS vào → fuse thông → LDO/buck output → enable pin → feedback pin.
- *"Board đôi khi reset ngẫu nhiên?"* → Thiếu tụ decoupling → sụt áp brown-out; hoặc watchdog kick; nhiệt độ cao; ESD; nhiễu switching.
- *"Vì sao cặp trace USB đi song song và uốn zig-zag?"* → Differential pair + length matching để D+/D− đồng bộ pha.
- *"Vì sao BGA phải kiểm bằng X-Ray?"* → Bi hàn nằm dưới bụng chip, AOI không nhìn xuyên được.
- *"Sự khác nhau giữa LDO và Buck?"* → LDO tuyến tính, ít nhiễu, nóng khi sụt áp lớn; Buck switching, hiệu suất cao, có ripple.
- *"Vì sao MCU cần thạch anh ngoài khi có clock nội?"* → Cần độ chính xác cao cho UART/USB/RTC; clock nội (RC) trôi theo nhiệt & nguồn.

---

## 📐 8. Khái niệm chất lượng & sản xuất (thể hiện tư duy chuyên nghiệp)

- **Yield (tỷ lệ đạt)**: % sản phẩm tốt. **FPY** (First Pass Yield) = đạt ngay lần đầu, không rework.
- **Defect / DPMO**: số lỗi trên triệu cơ hội.
- **DFM / DFA**: Design for Manufacturability/Assembly — thiết kế sao cho dễ sản xuất, ít lỗi.
- **8D / 5-Why / Fishbone (Ishikawa)**: công cụ phân tích nguyên nhân gốc (root cause).
- **SPC (Statistical Process Control)**: kiểm soát quá trình bằng thống kê (control chart, Cp/Cpk).
- **Six Sigma / Lean / Kaizen / 5S**: triết lý cải tiến & giảm lãng phí.
- **RoHS**: tuân thủ không dùng chất độc hại (chì...) → nhớ "lead-free soldering".
- **ESD (Electrostatic Discharge)**: chống tĩnh điện — đeo vòng tay tiếp đất, sàn/bàn ESD; tĩnh điện phá hỏng IC.

---

## 💬 9. Câu hỏi phỏng vấn thường gặp + gợi ý trả lời

### Nhóm kỹ thuật cơ bản
1. **Định luật Ohm? Tính công suất?** → U=IR, P=UI.
2. **Analog vs Digital khác nhau?** → (xem mục 3).
3. **PCB là gì? Vai trò?** → Khung mạch kết nối & đỡ linh kiện.
4. **SMT là gì? Kể quy trình?** → 3 bước lõi + 8 bước chi tiết (mục 4.3).
5. **ICT vs FCT?** → "Lắp đúng chưa" vs "Chạy đúng chưa".
6. **Kể vài lỗi hàn SMT?** → Tombstoning, bridging, cold joint... (mục 4.4).
7. **Transistor dùng làm gì?** → Khuếch đại & đóng/ngắt (switch).
8. **Điện 1 pha vs 3 pha?** → 3 pha lệch 120°, cho tải công suất lớn.
9. **Bạn biết hàn không? Mô tả các bước hàn 1 linh kiện.** → 5 bước ở mục 4.5c: làm sạch → làm nóng chân+pad → đưa thiếc → rút thiếc → rút mỏ hàn, giữ yên cho nguội.
10. **Sự khác biệt giữa hàn có chì và không chì?** → Không chì (lead-free, SAC305) chảy ở nhiệt cao hơn (~217°C) so với có chì (Sn63/Pb37, ~183°C); không chì tuân chuẩn RoHS.
11. **Làm sao biết mối hàn tốt hay xấu?** → Theo IPC-A-610: mối hàn hình phễu sáng bóng, đều, không cầu chì/rỗ khí/nguội (mục 4.5d).
12. **Hàn tay khác hàn máy (reflow/wave) ở điểm nào? Khi nào dùng hàn tay?** → Hàn tay dùng cho rework, linh kiện đặc biệt, mẫu thử; hàn máy dùng cho sản xuất hàng loạt (mục 4.5a).

### Nhóm đọc bản vẽ & phần cứng (rất hay hỏi cho Test/NPI/Process)
13. **Bộ tài liệu sản xuất PCBA gồm những gì?** → Schematic, PCB layout, Gerber, Drill, BOM, Pick&Place, Assembly drawing, Stencil, Fab notes (mục 4.6).
14. **File Pick & Place chứa thông tin gì? Máy dùng làm gì?** → Designator, X-Y, góc xoay, layer Top/Bottom → máy P&P đọc để gắp đặt linh kiện đúng vị trí.
15. **Kể vài reference designator hay gặp trên schematic?** → R (điện trở), C (tụ), L (cuộn cảm), U (IC), Q (transistor), D (diode/LED), J (connector), Y/X (thạch anh), TP (test point), FB (ferrite bead).
16. **DNP trên assembly drawing nghĩa là gì?** → Do Not Populate — linh kiện có in trên board nhưng KHÔNG lắp (cấu hình option, hoặc rework backup).
17. **0402 là gì? Khác 0603 thế nào?** → Kích thước tụ/trở chip theo inch: 0402 = 1.0×0.5 mm, 0603 = 1.6×0.8 mm; 0402 nhỏ hơn, dùng cho board mật độ cao.
18. **BGA phát hiện lỗi hàn bằng gì?** → AXI / X-Ray, vì AOI không thấy bi hàn ẩn dưới bụng chip.
19. **Board không lên nguồn, bạn kiểm tra theo thứ tự nào?** → Top-down: VBUS → Fuse → LDO/Buck out → Enable pin → Clock → Reset → Strap pins (mục 5.6 & 7.5g).
20. **Vì sao mỗi chân VCC của IC cần tụ 100 nF sát chân?** → Decoupling: giữ áp ổn định khi IC chuyển mạch, giảm impedance đường nguồn ở tần số cao, giảm nhiễu.
21. **Cách bạn khoanh vùng lỗi khi 1 board FCT fail?** → Reproduce → Isolate (log + so sánh 1 lô hay lẻ) → **Compare good vs bad** → Half-split → Root cause 5-Why → Verify & 8D (mục 5.6).
22. **Ferrite bead khác cuộn cảm ở đâu?** → Bead "lossy" ở HF, tiêu tán nhiễu thành nhiệt; cuộn cảm tích trữ năng lượng, ít mất mát.

### Nhóm tình huống (STAR: Situation–Task–Action–Result)
9. **"Phát hiện lỗi hàng loạt trên PCB trước khi xuất, xử lý sao?"**
   → Dừng dây chuyền → cách ly lô hàng (containment) → phân tích root-cause (5-Why/Fishbone) → sửa quy trình → kiểm tra lại → báo cáo 8D.
10. **"Giải quyết vấn đề mạch phức tạp?"** → Kể 1 ví dụ thật (kể cả từ đồ án/dự án C++), nhấn tư duy phân tích có hệ thống.

### Nhóm giới thiệu bản thân (chuẩn bị sẵn 60 giây)
- Học Cơ điện tử BK HN → làm C++ Automotive App/Middleware → hiểu **cả phần cứng lẫn phần mềm** → muốn đóng góp cho sản xuất/test/automation ở môi trường EMS quy mô lớn.
- **Biến "quên điện tử" thành lợi thế**: "Tôi có nền điện tử vững thời sinh viên, cộng kinh nghiệm phần mềm & embedded thực chiến — tôi học lại rất nhanh và kết nối được 2 mảng."

### Nhóm về công ty (bắt buộc tìm hiểu trước)
11. **"Vì sao chọn công ty chúng tôi?"** → Biết họ là EMS lớn, làm connector/acoustic/lắp ráp Apple/automotive, có nhà máy tại VN (Bắc Giang, Nghệ An...). Gắn với định hướng của bạn.

---

## ✅ 10. Checklist ôn tập nhanh (tick trước khi phỏng vấn)

- [ ] Ohm, KCL, KVL, công suất
- [ ] R-C-L: chức năng & mắc nối tiếp/song song
- [ ] Diode, transistor (switch), op-amp cơ bản
- [ ] Analog vs Digital, ADC/DAC, cổng logic, nhị phân/hex
- [ ] PCB: cấu tạo, SMT vs THT
- [ ] **Quy trình SMT 8 bước** (thuộc lòng)
- [ ] Lỗi hàn: tombstoning, bridging, cold joint, voiding
- [ ] Kỹ năng hàn tay: 5 bước hàn, chuẩn IPC-A-610, hàn tay vs hàn máy, hàn chì vs không chì
- [ ] **Bộ tài liệu sản xuất SMT**: Schematic, PCB layout, Gerber (GTL/GBL/GTS/GTO/GTP), Drill, BOM, Pick&Place, Assembly, Stencil, Fab notes
- [ ] **Reference designator**: R, C, L, U, Q, D, J, Y, TP, FB, RN — hiểu ý nghĩa
- [ ] **Package SMT**: 0201/0402/0603/0805, SOT-23, SOIC, QFP, QFN, BGA — biết BGA phải X-Ray
- [ ] **Cách đọc schematic**: power rails → IC chính → I/O chain → net name → DNP
- [ ] **Quy trình debug FCT**: Reproduce → Isolate → Compare good/bad → Half-split → Top-down power-up → 5-Why → 8D
- [ ] **Hiểu phần cứng**: power rails (LDO/Buck), decoupling cap 100nF, ground plane, impedance/length matching cho HS signals
- [ ] **Boot flow embedded**: VCC → Clock → Reset → Strap pins → bootloader
- [ ] **Cổng debug**: JTAG, SWD, UART console — biết dùng Test Point
- [ ] **Bảo vệ mạch**: TVS, Ferrite bead, Fuse/PTC, reverse polarity
- [ ] AOI / X-Ray / ICT / FCT / Burn-in (phân biệt rõ)
- [ ] PLC, relay, contactor, biến tần, cảm biến
- [ ] ESD, RoHS, Yield/FPY, DFM, 8D/5-Why, SPC/Cpk
- [ ] Chuẩn bị 1 câu chuyện STAR + giới thiệu bản thân 60s
- [ ] Tìm hiểu sản phẩm & nhà máy VN của công ty ứng tuyển

---

### 📌 Ghi chú cuối
- Nội dung điện tử cơ bản là kiến thức nền ổn định; phần quy trình SMT/test tổng hợp từ tài liệu ngành (Foxconn, NextPCB, JLCPCB, PCBELEC) và các bộ câu hỏi phỏng vấn điện tử VN.
- **Chiến lược thắng**: không cần giỏi sâu điện tử bằng dân hardware thuần — hãy chứng minh bạn **hiểu đủ điện tử + mạnh phần mềm/embedded**, đó là hồ sơ hiếm và giá trị cho EMS đang tự động hóa mạnh.
