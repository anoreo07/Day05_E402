# Evidence Pack 

## 1. Nhóm và track

> Nhóm: 4  
> Thành viên: Nguyễn Hải An · Lương Đình Bút · Đoàn Thị Thu Linh · Nguyễn Duy Đức · Lê Văn Quang  
> Ngày: Day 05 Lab


**Track:** Travel / Du lịch  
**Product/app đã chọn:** Gợi ý lịch trình tham quan Hà Nội  
**Build slice đang nghĩ:** Chatbot hỏi 4–5 câu về thói quen/sở thích của khách, sau đó trả ra lịch trình 1–2 ngày gồm địa điểm tham quan, quán ăn, phương tiện di chuyển phù hợp — kèm ảnh/video minh họa từ dữ liệu thực.

---

## 2. Self-use evidence

Nhóm tự dùng các trang hiện có (Vietnam Tourism, Foody, Google Maps) và ghi lại điểm gãy.

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| Truy cập csdl.vietnamtourism.gov.vn/dest/?province=01 — danh sách địa điểm hiển thị dạng bảng thuần, không có filter theo sở thích, không biết nên đi đâu trước | [Vietnam Tourism DB](https://csdl.vietnamtourism.gov.vn/dest/?province=01) | Failure | User cần bước "lọc theo nhu cầu" trước khi xem danh sách; trang hiện tại không có |
| Trên Foody Hà Nội, tìm "quán bún bò" trả về hàng trăm kết quả, không có gợi ý "phù hợp với người lần đầu đến" hay "gần Hồ Gươm" | [Foody HN](https://www.foody.vn/ha-noi) | Low-confidence | Filter địa lý + context "lần đầu đến" là nhu cầu thật, chưa được đáp ứng |
| Mục bar/pub trên Foody có thông tin giờ mở cửa nhưng không ghi rõ có phù hợp khách nước ngoài không, không có tag "local vibe" vs "tourist-friendly" | [Foody Bar](https://www.foody.vn/ha-noi/bar-pub) | Low-confidence | Khách nước ngoài cần thêm context "vibe" và "local vs tourist" mà data hiện tại thiếu |
| Google Maps gợi ý lịch trình nhưng không hỏi về thói quen ăn uống, mức chi tiêu, hay preference ở homestay vs khách sạn — output generic | Self-use Google Maps | Failure | Personalization dựa trên habit là khoảng trống lớn |

---

## 3. User / review / social evidence

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Mình đến Hà Nội 3 ngày mà cứ quanh quẩn mấy chỗ du lịch đông, không biết chỗ ăn ngon thật sự của người địa phương" | Group Facebook "Du lịch Hà Nội tự túc" | Khách nội địa lần đầu đến HN | Không khám phá được hidden gem; chỉ đến điểm nổi tiếng |
| "I spent 2 hours trying to plan a 1-day Hanoi itinerary but every blog gives a different list and I don't know what's open on weekdays" | TripAdvisor review forum Hanoi | Khách quốc tế, chuẩn bị chuyến đi | Conflict thông tin, không biết giờ mở cửa thực tế |
| "App du lịch nào cũng hỏi 'bạn muốn đi đâu' nhưng mình không biết Hà Nội có gì để trả lời" | Comment YouTube vlog du lịch HN | Khách lần đầu, chưa có baseline knowledge | Cold-start: user không có đủ thông tin để tự chọn |
| Nhiều review trên Google Maps cho thấy điểm tham quan đóng cửa thứ 2 nhưng app lịch trình không cảnh báo, khách đến không vào được | Review Google Maps Văn Miếu, Bảo tàng HN | Khách có kế hoạch sẵn | Failure mode nghiêm trọng: sai thông tin giờ mở cửa |
| "Muốn trải nghiệm sống như người Hà Nội 1 ngày — ăn bún chả, đi chợ, uống cà phê trứng — nhưng không biết đi theo thứ tự nào và ở khu nào" | Reddit r/solotravel | Khách quốc tế, muốn local experience | Cần curation theo "theme/lifestyle", không chỉ danh sách địa điểm |

---

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| Google Maps "Explore" tab | Gợi ý địa điểm dựa trên vị trí, có filter theo loại (ăn uống, tham quan) nhưng không hỏi về habit/preference | Filter theo category — nhưng thiếu personalization và sequencing | Một phần — dùng làm data source |
| TripAdvisor Itinerary Builder | User tự chọn địa điểm, app sắp xếp theo ngày — nhưng không gợi ý dựa trên lifestyle | Sequencing logic theo thời gian + map — đáng học | Có thể học cấu trúc output |
| ChatGPT / Claude (hỏi tự do) | Trả lời được nhưng không có data thực về giờ mở cửa, discount, không có ảnh/video thực | LLM + structured data = stronger; pure LLM thiếu ground truth | Là điểm differentiation của prototype này |
| Klook / GetYourGuide | Gợi ý tour có sẵn, có ảnh, có review — nhưng chỉ bán tour, không tạo lịch trình tự do | Social proof (ảnh, review) giúp user tin tưởng | Dùng pattern ảnh + review để augment output |

---

## 5. Evidence → Insight

```
Evidence nổi bật nhất:
- Khách (cả nội địa lẫn quốc tế) không thiếu thông tin về HN — họ thiếu cách lọc thông tin
  đó qua lens "tôi là ai và tôi muốn trải nghiệm gì".
- Cold-start problem: user không biết HN nên không thể tự fill form "bạn muốn đi đâu".
- Failure mode thực tế: địa điểm đóng cửa ngày nhất định mà app không cảnh báo.

Insight:
Khách du lịch đến Hà Nội không chỉ cần danh sách địa điểm.
Thật ra họ cần một người dẫn đường hiểu họ là ai — ăn uống thế nào,
muốn sống kiểu local hay tourist, budget bao nhiêu — rồi tạo ra lịch trình
đã được sắp xếp theo thời gian thực tế (giờ mở cửa, khoảng cách).

Opportunity:
AI có thể giúp bằng cách hỏi 4–5 câu về habit/preference (thay vì bắt user tự biết HN),
sau đó tổng hợp từ data thực (Vietnam Tourism DB + Foody + giờ mở cửa)
để tạo lịch trình cá nhân hóa kèm visual (ảnh/video).
```

---

## 6. Evidence đổi SPEC như thế nào?

- [x] Đổi user chính — ban đầu nghĩ "khách du lịch nói chung", sau evidence chốt thành **khách lần đầu đến Hà Nội (cold-start user)** vì đây là nhóm có pain lớn nhất.
- [x] Đổi pain statement — surface pain là "không biết đi đâu", deeper pain là **không có context để tự lọc thông tin**.
- [x] Đổi build slice — thêm **failure path xử lý địa điểm đóng cửa** vào prototype vì đây là failure mode nguy hiểm nhất từ evidence.
- [ ] Đổi Auto/Aug decision.
- [x] Đổi 4 paths — thêm path "user thay đổi preference giữa chừng" (muốn thêm địa điểm ăn uống).
- [x] Đổi failure mode — chốt failure mode chính là **AI gợi ý địa điểm đóng cửa đúng ngày khách đến**.

```
Trước evidence, nhóm định build chatbot hỏi "bạn muốn tham quan loại gì" và trả list.
Sau evidence, nhóm đổi thành chatbot hỏi theo habit/lifestyle (không yêu cầu user biết HN trước),
và prototype phải xử lý được failure path "địa điểm đóng cửa".
Lý do: evidence từ review cho thấy đây là pain thực tế gây frustration cao nhất.
```
