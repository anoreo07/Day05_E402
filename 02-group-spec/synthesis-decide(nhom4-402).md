# Toolkit — Từ Evidence Đến Build Slice 

> Nhóm: 4  
> Thành viên: Nguyễn Hải An · Lương Đình Bút · Đoàn Thị Thu Linh · Nguyễn Duy Đức · Lê Văn Quang  
> Ngày: Day 05 Lab

## 1. Gom evidence thành cụm

Gom theo **workflow/pain**, không gom theo tên feature.

| Cụm pain | Evidence gom vào |
|---|---|
| **"Không biết bắt đầu từ đâu" (cold-start)** | User không biết HN có gì nên không thể tự điền form; Google Maps không hỏi về preference |
| **"Thông tin quá nhiều, không lọc được"** | Foody trả hàng trăm kết quả; mỗi blog gợi ý list khác nhau; Vietnam Tourism DB không có filter theo sở thích |
| **"Lịch trình không khớp thực tế"** | Địa điểm đóng cửa thứ 2 nhưng app không cảnh báo; giờ mở cửa không chính xác |
| **"Muốn trải nghiệm local nhưng không biết cách"** | Muốn ăn bún chả, đi chợ, cà phê trứng theo thứ tự tự nhiên nhưng cần người dẫn đường |
| **"Output generic, không phù hợp tôi"** | ChatGPT/Claude biết HN nhưng không biết tôi — thiếu personalization theo lifestyle |

---

## 2. Viết insight

```
User lần đầu đến Hà Nội không chỉ cần danh sách địa điểm tham quan.
Họ thật ra cần một người dẫn đường hiểu lifestyle của họ
(ăn uống thế nào, muốn trải nghiệm local hay tourist, di chuyển bằng gì),
vì cold-start problem khiến họ không thể tự lọc thông tin từ hàng trăm nguồn khác nhau —
và failure mode nguy hiểm nhất là đến nơi thì địa điểm đóng cửa.
```

---

## 3. Viết opportunity

```
Cơ hội là dùng AI để hỏi 4–5 câu về habit/lifestyle của khách (thay vì bắt user tự biết HN),
tổng hợp từ data thực (Vietnam Tourism DB, Foody, giờ mở cửa)
để tạo lịch trình 1–2 ngày cá nhân hóa kèm ảnh/video minh họa,
trong khi vẫn kiểm soát failure mode "địa điểm đóng cửa đúng ngày khách đến"
bằng cách cảnh báo rõ và gợi ý thay thế.
```

---

## 4. Chọn build slice

**Build slice đã chọn:**

```
Cho khách du lịch lần đầu đến Hà Nội đang cần lập kế hoạch chuyến đi 1–2 ngày,
prototype chatbot hỏi 5 câu về habit (ăn uống, phong cách tham quan, chỗ ở, ngân sách, ngày đến),
sau đó dùng AI tổng hợp từ data thực (Vietnam Tourism DB + Foody)
để trả ra lịch trình theo khung giờ kèm ảnh minh họa,
và xử lý failure mode "địa điểm đóng cửa đúng ngày" bằng cảnh báo + gợi ý thay thế.
```

**Kiểm tra 5 câu hỏi:**

| Câu hỏi | Kết quả |
|---|---|
| User cụ thể chưa? | ✅ Khách lần đầu đến HN, đang lên kế hoạch chuyến đi 1–2 ngày |
| Task đủ hẹp chưa? | ✅ Tạo lịch trình 1 ngày — demo được trong 3–5 phút |
| AI decision rõ chưa? | ✅ AI hỏi habit → match địa điểm phù hợp → sắp xếp theo giờ + khoảng cách |
| Failure path rõ chưa? | ✅ User nhập ngày thứ 2 → AI detect địa điểm đóng cửa → cảnh báo + thay thế |
| Có evidence không? | ✅ Review TripAdvisor, Facebook group, self-use Vietnam Tourism DB + Foody |

---

## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống | Đánh giá | Quyết định |
|---|---|---|
| Evidence có đủ không? | Có: self-use + review thực + competitor analysis | **Giữ hướng** |
| Ý tưởng có quá rộng không? | Có thể rộng nếu build cả video TikTok | **Giảm scope**: ảnh tĩnh trước, video là backlog |
| AI có cần thiết không? | Cần: cold-start + personalization + matching theo habit không làm được bằng filter đơn giản | **Giữ AI** |
| Rủi ro cao không? | Thấp-trung: thông tin sai về giờ mở cửa — xử lý bằng disclaimer + real-time check | **Augmentation** |
| Demo được trong 1 ngày không? | Được nếu giới hạn: 1 flow, 5 câu hỏi, 10–15 địa điểm seed data | **Giữ scope đã cắt** |

---

## 6. Câu chốt cuối

```
Dựa trên evidence từ review TripAdvisor/Facebook, self-use Vietnam Tourism DB và Foody,
nhóm sẽ build chatbot hỏi 5 câu về habit để tạo lịch trình 1 ngày tại Hà Nội,
cho khách lần đầu đến HN chưa có baseline knowledge về thành phố,
để giải quyết cold-start problem và failure mode "đến nơi thì đóng cửa",
bằng cách AI match habit với địa điểm từ data thực → sắp xếp theo khung giờ → kèm ảnh,
và sẽ test failure path: user nhập ngày thứ 2, AI phải detect và cảnh báo Văn Miếu/Bảo tàng đóng cửa.
```

---

## 7. Backlog

Những thứ **không build trong Day 06**:

- Tích hợp video TikTok real-time (cần API, scope lớn)
- Gợi ý khách sạn/homestay (cần booking integration)
- Multi-day trip dài hơn 2 ngày
- Hỗ trợ đa ngôn ngữ (EN/JP/KR) đầy đủ
- Real-time pricing và discount tự động cập nhật
- Route optimization theo traffic thực tế (Google Maps API)
