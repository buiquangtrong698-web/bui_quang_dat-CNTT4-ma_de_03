# BÀI LÀM PHA 1 – HACKATHON 02

- **Mã đề:** 03
- **Họ tên:** bùi quang đạt
- **Lớp:**CNTT4
- **Link repository Pha1:**

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
là 1 chuyên viên tư vấn . hãy soạn 1 email thông báo xác nhận khóa học cho học viên theo mẫu trong thẻ <data>
<data>
Học viên [ten_hoc_vien], số điện thoại [SO_DIEN_THOAI], số CCCD [CCCD], email [EMAIL], đăng ký khoá Luyện thi IELTS 5.5 (mã IE55) khai giảng ngày 14/12/2026 cho 1 học viên lớn và 1 học viên nhỏ 13 tuổi. Khách đã chuyển khoản đặt cọc 2.000.000 đồng. Khách yêu cầu học lớp buổi tối.
<\data>

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
- bn hãy trả lời câu hỏi trên dựa theo file đính kèm tôi dưa và không tự bịa câu trả lời nếu không hiểu có thể hỏi lại tôi để xác nhận rồi mới trả lời
- khi bn trả lời thì ghi rõ cho tôi bn lấy từ mục , điều khoản nào cảu file đính kèm  
```

## 4.2. Trích xuất tin nhắn đăng ký khoá học ra JSON

**PROMPT**

```text
c - context : bn là 1 chuyên viên sử lý thông tin sinh viên đăng ký khóa học 
r - request : bn hãy chuẩn hóa thông tin sinh viên đăng ký khóa học theo 6 trường như sau : ma_khoa ,ngay_khai_giang ,hoc_vien_lon ,hoc_vien_nho ,hinh_thuc ,offline ,ghi_chu ,và dựa vào thông tin khóa học ở thẻ <bang> để chuẩn hóa thông tin khóa học cho phù hợp 
e - explanation : bn lấy thông tin tin nhắn của sinh viên từ thẻ <TN> sau đó bn chuẩn hóa thông tin từ tin nhắn sinh viên theo trong vd trong thẻ <mau>
a - action : bn chuẩn hóa thông tin của sinh viên theo đúng quy tắc như sau :
QUY TẮC CHUẨN HOÁ (5 quy tắc)
1. ma_khoa lấy theo bảng mã khoá học.
2. ngay_khai_giang dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc học viên nhỏ tuổi thì hoc_vien_nho là 0 và mọi khách là học viên lớn.
4. hinh_thuc chỉ nhận "online", "offline" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về sức khoẻ, đi lại, điều kiện học tập; không có thì null.

bỏ qua lời chào hỏi xã giao , không giải thích và trả về như tôi yêu cầu 
t - tone : bn sử dung tone chuyên nghiệp 
e - extras : trả về cho tôi dưới dạng join không lời chào hỏi xã giao , không giải thích

<TN>
TN1: Anh đăng ký khoá Ngữ pháp nền tảng khai giảng 08/12 nhé, 2 người lớn và 1 bé 13 tuổi, học offline.

TN2: Cho 3 người học IELTS 5.5, khai giảng 06/01/2027 ạ. Một bạn bị khiếm thính nhẹ.

TN3: Lớp Giao tiếp tháng 12 còn chỗ không ạ? Nhóm em 4 người lớn, một bạn đi nạng, muốn học offline.
<\TN>

<mau>
Tin nhắn mẫu 1:
Cho chị đăng ký khoá Giao tiếp cơ bản khai giảng 10/11/2026, chị với con gái 12 tuổi, học offline. Con chị bị cận nặng nên cần ngồi bàn đầu.
Kết quả đúng:
{"ma_khoa": "GT01", "ngay_khai_giang": "2026-11-10", "hoc_vien_lon": 1, "hoc_vien_nho": 1, "hinh_thuc": "offline", "ghi_chu": "con bị cận nặng, cần ngồi bàn đầu"}

Tin nhắn mẫu 2:
IELTS 5.5 còn lớp online không ạ, em học 1 mình, lớp nào khai giảng cũng được.
Kết quả đúng:
{"ma_khoa": "IE55", "ngay_khai_giang": null, "hoc_vien_lon": 1, "hoc_vien_nho": 0, "hinh_thuc": "online", "ghi_chu": null}
<\mau>

<bang>
BẢNG MÃ KHOÁ HỌC
GT01 – Giao tiếp cơ bản, 24 buổi
IE55 – Luyện thi IELTS 5.5, 30 buổi
NP12 – Ngữ pháp nền tảng, 12 buổi
<\bang>
```

## 4.3. Báo giá bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
bn là 1 quản trị viên thu ngân chuyên nghiệp . bn hãy tính tổng tiền cho yêu cầu báo giá trong thẻ <bao> dựa vào bảng giá trong thẻ <gia> và quy tắc trong thẻ <qt> . bn hãy suy luận theo từng bước thứ tự như sau : tiền học viên lớn -> tiền học viên nhỏ tuổi -> tiền học phí -> giảm giá nhóm -> phí giáo trình -> tổng tiền mỗi bước ghi phép tính cho tôi dòng cuối ghi có dạng như sau : [RESULT: <tổng tiền> đồng] không lời chào hỏi xã giao , không giải thích 


<gia>
BẢNG HỌC PHÍ (đồng/học viên lớn)
GT01 – Giao tiếp cơ bản, 24 buổi: 4.800.000
IE55 – Luyện thi IELTS 5.5, 30 buổi: 7.500.000
NP12 – Ngữ pháp nền tảng, 12 buổi: 2.400.000
<\gia>

<qt>
QUY TẮC TÍNH GIÁ
1. Học viên nhỏ tuổi (từ 10 đến 15 tuổi) tính 60% học phí học viên lớn. Trẻ dưới 10 tuổi học dự thính cùng phụ huynh miễn phí.
2. Nhóm từ 4 học viên lớn trở lên được giảm 10% trên tiền học phí (học viên lớn + học viên nhỏ tuổi).
3. Phí giáo trình bản in: 250.000 đồng/bộ. Phí giáo trình không được giảm giá.
4. Tổng tiền = tiền học phí sau giảm + phí giáo trình.
<\qt>

<bao>
YÊU CẦU BÁO GIÁ
Nhóm đăng ký khoá NP12 gồm 4 học viên lớn, 2 học viên nhỏ tuổi (11 tuổi và 14 tuổi) và 1 bé 8 tuổi học dự thính. Nhóm mua 3 bộ giáo trình bản in.
<\bao>
```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
bn là 1 chuyên viên chuyên phụ trách xử lý email khiếu nại . bn hãy sử lý email trong thẻ <email> theo chính sách của công ty trong thẻ <cs> và trả về định dạng theo đúng như vd trong thẻ <vd> bỏ qua lời chào hỏi xã giao và trả về dạng jion

<email>
EMAIL KHIẾU NẠI CỦA KHÁCH

Tôi đăng ký khoá Giao tiếp cơ bản GT01 cho 2 học viên (tôi và em trai), học offline từ 03/11/2026. Hai tuần qua có 4 buổi học bị huỷ mà trung tâm không báo trước, chúng tôi đến lớp mới biết. Cô giáo Hà dạy rất dễ hiểu, tôi không có gì phàn nàn về giáo viên. Tôi đề nghị trung tâm giải quyết thỏa đáng.
</email>

<cs>
======================================================
CHÍNH SÁCH ĐỀN BÙ CỦA SKYENGLISH
======================================================

- huy_buoi: buổi học bị huỷ không báo trước: hoàn 250.000 đồng/học viên cho mỗi buổi bị huỷ.
- hoc_lieu: thiếu hoặc sai giáo trình: tặng giáo trình mới trị giá 250.000 đồng/học viên.
- giao_vien: khiếu nại về thái độ, chất lượng giáo viên: tặng voucher 300.000 đồng/học viên.
- khac: chuyển bộ phận Chăm sóc học viên xem xét, chưa đề xuất mức đền bù.
- Tổng mức đền bù trên 1.500.000 đồng phải được Giám đốc trung tâm duyệt trước khi gửi khách.

<\cs>

<vd>
======================================================
ĐỊNH DẠNG ĐẦU RA CỦA TRẠM 1 (sinh viên viết prompt)
======================================================

Trạm 1 – đọc email, trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "huy_buoi" | "hoc_lieu" | "giao_vien" | "khac", "so_hoc_vien": số nguyên, "so_buoi_bi_huy": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}
<\vd>
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. phải là giạng jion bỏ qua lời chào hỏi xã giao và trả về dạng jion

2.yêu cầu phải có trong chính sách nếu không có thì phải hỏi lại và không bịa câu trả lời

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
bn hãy viết tiếp cho tôi , bn đang hị ngắt "..skyenglish xin hoàn lại quý khách học viên"
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý SkyEnglish", nhân viên tư vấn trên website của trung tâm tiếng Anh SkyEnglish, giúp khách chọn và đăng ký một trong 3 khoá: GT01 (Giao tiếp cơ bản), IE55 (Luyện thi IELTS 5.5), NP12 (Ngữ pháp nền tảng).

[Quy tắc ràng buộc]
1.chỉ tư vấn 3 khoá học của công ty;
2. không hứa giảm giá ngoài bảng giá; 
3. không hỏi xin số CCCD, số thẻ ngân hàng.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Học phí trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: trả lời thẳng là câu hỏi ngoài phạm vi xử lý của tôi
- Khách cung cấp thiếu thông tin: hỏi và yêu cầu thêm thông tin thiếu
```

