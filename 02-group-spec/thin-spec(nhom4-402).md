# Thin SPEC Cuối Day 05 

> Nhóm: 4  
> Thành viên: Nguyễn Hải An · Lương Đình Bút · Đoàn Thị Thu Linh · Nguyễn Duy Đức · Lê Văn Quang  
> Ngày: Day 05 Lab

---

## 1. Track, product/app và user

**Track:** Travel / Du lịch  
**Product/app thật:** Chatbot gợi ý lịch trình tham quan Hà Nội (dựa trên Vietnam Tourism DB + Foody)  
**User cụ thể:** Khách du lịch lần đầu đến Hà Nội — chưa biết thành phố, đang lên kế hoạch cho chuyến đi 1–2 ngày  
**Nhóm có phải user thật không?**  
Một phần. Thành viên đã từng đón người thân/bạn bè lần đầu đến HN và quan sát pain thực tế. Khác ở chỗ: nhóm biết HN nên không bị cold-start như user thật. Cần kiểm thử với người thật chưa đến HN bao giờ.

---

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| "Không biết chỗ ăn ngon thật sự của người địa phương" | Facebook group "Du lịch HN tự túc" | User muốn local experience nhưng thiếu insider lens để lọc | Build slice phải hỏi về preference local vs tourist, không chỉ "loại địa điểm" |
| Địa điểm đóng cửa thứ 2 nhưng app không cảnh báo | Review Google Maps Văn Miếu | Failure mode nghiêm trọng gây frustration cao — khách đến không vào được | Prototype bắt buộc xử lý path này; không để generic |
| "App du lịch hỏi bạn muốn đi đâu nhưng tôi không biết HN có gì" | Comment YouTube vlog HN | Cold-start: user không có đủ context để tự chọn | Chatbot phải hỏi về lifestyle/habit, không hỏi địa điểm cụ thể |
| Vietnam Tourism DB không có filter theo sở thích; Foody trả hàng trăm kết quả | Self-use | Data có nhưng UX thiếu lớp personalization | AI phải làm lớp matching habit → địa điểm, không chỉ list |
| Khách quốc tế mất 2 tiếng lên kế hoạch 1 ngày vì blog mâu thuẫn | TripAdvisor forum | Conflict thông tin + không biết giờ thực tế | Output phải có giờ mở cửa xác nhận từ data source thực |

---

## 3. Pain statement

```
Khách du lịch lần đầu đến Hà Nội đang gặp khó ở bước lên kế hoạch chuyến đi,
vì họ không có đủ context về thành phố để tự lọc hàng trăm gợi ý từ blog/app,
và không biết địa điểm nào phù hợp với lifestyle của họ (ăn gì, ở đâu, trải nghiệm gì).
Dẫn tới: mất nhiều giờ research, lịch trình generic không phù hợp, hoặc đến nơi thì địa điểm đóng cửa.
Bằng chứng chính:
- Review TripAdvisor: "spent 2 hours planning, every blog gives a different list"
- Google Maps review: Văn Miếu đóng thứ 2, khách không biết
- Facebook group: "không biết chỗ ăn ngon thật của người địa phương"
```

---

## 4. Build slice

```
Cho khách du lịch lần đầu đến Hà Nội đang lên kế hoạch chuyến đi 1–2 ngày,
prototype chatbot sẽ hỏi 5 câu về habit/preference
  (1. thích ăn uống thế nào — đường phố/nhà hàng/local,
   2. muốn tham quan văn hóa-lịch sử hay khu vui chơi-mua sắm,
   3. ưu tiên trải nghiệm local hay tiện nghi tourist,
   4. ngân sách/ngày ước tính,
   5. ngày và số ngày đến HN),
dùng AI để match habit với địa điểm từ Vietnam Tourism DB + Foody,
sắp xếp theo khung giờ thực tế (sáng/trưa/chiều/tối) kèm ảnh minh họa,
tạo ra lịch trình 1 ngày cá nhân hóa dạng text + ảnh có thể copy/share,
và xử lý failure mode "địa điểm đóng cửa đúng ngày khách đến"
bằng cảnh báo rõ ràng + gợi ý 1–2 địa điểm thay thế tương đương.
```

---

## 5. Auto/Aug decision

- [ ] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [x] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:** Với lịch trình du lịch, 80% case (rõ preference, địa điểm mở cửa) AI có thể tự tạo output ngay. Nhưng khi có ambiguity cao (preference mâu thuẫn, ngày lễ đặc biệt, câu hỏi về an toàn/khẩn cấp) cần hỏi lại hoặc chuyển sang thông tin chính thức.

**Human role:** Reviewer — user xem lịch trình và có thể yêu cầu điều chỉnh ("thêm 1 quán cà phê", "bỏ bảo tàng", "đổi buổi chiều")

---

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| **Happy** | User trả lời 5 câu rõ ràng → AI trả lịch trình 1 ngày đầy đủ (5–7 địa điểm), có khung giờ, có ảnh, địa điểm đang mở cửa đúng ngày → User hài lòng, copy lịch trình |
| **Low-confidence** | User trả lời mơ hồ (VD: "tôi ăn được hết" hoặc không có preference rõ) → AI hỏi thêm 1 câu làm rõ hoặc trả lịch trình "balanced" với disclaimer "dựa trên sở thích phổ biến nhất" |
| **Failure** | User nhập ngày thứ 2 → AI detect Văn Miếu/Bảo tàng HN đóng cửa → hiện cảnh báo rõ "⚠️ [Tên địa điểm] đóng cửa vào thứ 2 — đề xuất thay thế: [...]" |
| **Correction** | User sau khi xem lịch trình muốn điều chỉnh ("thêm quán ăn buổi tối", "đổi khu vực từ Hoàn Kiếm sang Tây Hồ") → AI update lịch trình theo yêu cầu mà không hỏi lại từ đầu |

---

## 7. Failure mode nguy hiểm nhất

```
Nếu user nhập ngày đến là thứ 2 (hoặc ngày nghỉ lễ đặc biệt)
và chatbot không kiểm tra lịch đóng cửa của địa điểm,
AI có thể tạo lịch trình có Văn Miếu Quốc Tử Giám, Bảo tàng Lịch sử Quốc gia,
hoặc các bảo tàng nhà nước thường đóng thứ 2,
hậu quả là khách đến tận nơi và không vào được — gây mất thời gian, tiền xe,
và mất niềm tin vào sản phẩm ngay lần dùng đầu tiên.

Prototype sẽ xử lý bằng:
1. Khi user nhập ngày đến → hệ thống cross-check ngày đó là thứ mấy
2. Với địa điểm có lịch đóng cửa cố định (thứ 2, ngày lễ) → tự động loại hoặc cảnh báo
3. Hiển thị badge "⚠️ Kiểm tra giờ mở cửa trước khi đến" với link nguồn chính thức

Owner kiểm thử path này: [Thành viên phụ trách Test/Failure path]
```

---

## 8. Owner plan cho sáng Day 06

| Thành viên | Vai trò | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|---|
| Thành viên 1 | Research lead | Scrape/chuẩn bị seed data từ Vietnam Tourism DB + Foody (10–15 địa điểm HN có giờ mở cửa, loại, ảnh) | `/data/hanoi_places.json` với ít nhất 15 địa điểm đầy đủ field |
| Thành viên 2 | SPEC + Prompt | Viết system prompt cho LLM (habit → lịch trình), thiết kế 5 câu hỏi chatbot, viết thin SPEC cuối | `/prompts/system_prompt.md`, thin-spec.md hoàn chỉnh |
| Thành viên 3 | Prototype dev | Build chatbot flow (Streamlit hoặc simple web), tích hợp LLM API, kết nối data JSON | `/app/` — chạy được locally, có UI hỏi 5 câu và trả lịch trình |
| Thành viên 4 | Test + Failure path | Viết test cases, kiểm thử path thứ 2 đóng cửa, path preference mơ hồ, path correction | `/tests/test_cases.md` — ít nhất 4 test cases với expected output |
| Thành viên 5 | Demo + Repo | Viết demo script 3–5 phút, chuẩn bị repo nộp bài đúng cấu trúc, README hướng dẫn chạy | `README.md`, demo script, repo đúng format Day06-MãHọcViên-HọVàTên |

---

## 9. Stack kỹ thuật dự kiến (để team thống nhất)

| Component | Lựa chọn | Lý do |
|---|---|---|
| LLM | Claude Sonnet / GPT-4o | Context window đủ rộng cho multi-turn, có tool use để check data |
| Data ingestion | Firecrawl scrape Vietnam Tourism DB + Foody | Data thực, không hallucinate địa điểm |
| Data store | JSON file / SQLite đơn giản | Đủ cho prototype 1 ngày, không cần DB phức tạp |
| UI | Streamlit | Nhanh nhất cho prototype chat UI trong 1 ngày |
| Ảnh minh họa | Scrape từ nguồn hoặc dùng URL trực tiếp từ Foody | Đủ để demo, không cần hosting ảnh |
| Failure detection | Logic check ngày trong tuần vs danh sách đóng cửa trong JSON | Đơn giản, reliable, không cần API ngoài |
