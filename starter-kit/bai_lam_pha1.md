# BÀI LÀM PHA 1 – HACKATHON 02

- **Mã đề:** 04
- **Họ tên:** Đoàn Bảo Duy
- **Lớp:** HCM-KS26CNTT!
- **Link repository Pha1:**

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khách hàng [TEN_KHACH], số điện thoại [SO_DIEN_THOAI], số CCCD [SO_CCCD], email tung [EMAIL], đặt 2 vé người lớn và 1 vé trẻ em 6 tuổi tuyến Sài Gòn – Nha Trang (mã SGNT) ngày 20/12/2026. Khách đã chuyển khoản đặt cọc 500.000 đồng. Khách yêu cầu giường tầng dưới.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
câu 1: chỉ được sử dụng thông tin có trong file chính sách kèm để trả lời, tuyẹt đối không tự suy diễn; nếu tài liệu không đề cặp đếm trường hợp này 
câu 2: trong câu trả lời bắt buộc trích dẫn rõ ràng tên mục, số điều kiện hoặc trang chứa quy định trong file đính kèm
```

## 4.2. Trích xuất tin nhắn đặt vé xe ra JSON

**PROMPT**


```text
Bạn là trợ lý trích xuất dữ liệu đặt vé cho giải Cúp Sao Vàng.

[C - Context]
Bối cảnh: Bạn là trợ lý trích xuất dữ liệu đặt vé cho nhà xe. Bạn nhận các tin nhắn đặt vé bằng tiếng Việt. Mỗi tin nhắn có thể nêu mã tuyến, ngày đi, số người lớn, số trẻ em, loại vé và yêu cầu đặc biệt.

[R - Request]
Chuyển ba tin nhắn trong thẻ <tin_nhan> thành một mảng JSON, đúng thứ tự TN1, TN2, TN3. Mỗi phần tử chỉ có đủ 6 trường: ma_tuyen, ngay_di, nguoi_lon, tre_em, loai_ve, ghi_chu.

[E - Explanation]
Quy tắc chuẩn hoá:
1. ma_tuyen lấy theo bảng mã tuyến xe.
2. ngay_di dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì ve_tre_em là 0 và mọi khách là người lớn.
4. loai_cho chỉ nhận "giuong", "ghe" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về sức khoẻ, đi lại, mang thai; không có thì null.

Tin nhắn 1
Hai tin nhắn mẫu đã xử lý :
Đặt giúp em 2 vé người lớn 1 vé trẻ em đi Nha Trang ngày 18/11/2026, giường nằm nha. Bé hay say xe.
Kết quả đúng:
{"ma_tuyen": "SGNT", "ngay_di": "2026-11-18", "ve_nguoi_lon": 2, "ve_tre_em": 1, "loai_cho": "giuong", "ghi_chu": "bé hay say xe"}

Tin nhắn 2
Xe đi Cần Thơ còn ghế ngồi không anh, em đi 1 mình, ngày nào cũng được.
Kết quả đúng:
{"ma_tuyen": "SGCT", "ngay_di": null, "ve_nguoi_lon": 1, "ve_tre_em": 0, "loai_cho": "ghe", "ghi_chu": null}

[A - Action]
Đọc và trích xuất ba tin nhắn sau:

<tin_nhan>

TN1: Cho mình đặt xe Sài Gòn đi Đà Lạt ngày 22/12 nha, 2 người lớn với 1 bé 8 tuổi, đi giường nằm.

TN2: Đặt 3 vé đi Nha Trang ngày 02/01/2027 ạ. Một người đang mang thai 6 tháng.

TN3: Tuyến Cần Thơ dịp Tết còn vé không ạ? Nhà em 4 người lớn, bà nội đi xe lăn, đi ghế ngồi.

</tin_nhan>

[T - Tone]
Trả dữ liệu trung tính, chính xác, không thêm lời dẫn hay bình luận.

[E - Extras]
Chỉ trả về mảng JSON hợp lệ, không chào hỏi, không giải thích, không dùng markdown code fence.
```

## 4.3. Báo giá bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
 Bạn là một chuyên gia tính toán giá vé, nhiệm vụ của bạn là tổng số tiền khách hàng đã đặt vé theo đúng dữ liệu sau:

BẢNG GIÁ VÉ (đồng/vé người lớn)

SGDL – Sài Gòn – Đà Lạt: 320.000
SGNT – Sài Gòn – Nha Trang: 380.000
SGCT – Sài Gòn – Cần Thơ: 180.000

QUY TẮC TÍNH GIÁ
1. Trẻ em từ 5 đến 10 tuổi tính 75% giá vé người lớn. Trẻ dưới 5 tuổi ngồi cùng người lớn miễn phí.
2. Đoàn từ 6 người lớn trở lên được giảm 5% trên tiền vé (tiền vé người lớn + tiền vé trẻ em).
3. Phụ thu hành lý cồng kềnh: 100.000 đồng/kiện. Phụ thu không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + phụ thu.

Suy luận tính giá từng bươc theo thứ tự:
Bước 1: Tính tiền vé người lớn 
Bước 2: Tính tiền vé trẻ em
Bước 3: Tính tiền vé
Bước 4: Tính tiền vé giảm giá đoàn 
Bước 5 Tính tiền phụ thu
Bước 6: Tính tổng tiền, mỗi bước ghi lại phép tính; dòng cuối có dạng [RESULT:<tổng tiền>đồng]
```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**


```text
Bạn là Trạm 1 trong quy trình xử lý khiếu nại của nhà xe PHƯƠNG NAM EXPRESS .

Nhiệm vụ: đọc email trong thẻ <email> và trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "tre_chuyen" | "sai_loai_xe" | "tai_xe" | "khac", "so_khach": số nguyên, "so_chang_bi_doi": số nguyên hoặc null, "tom_tat"

<email>
Tôi đi tuyến Sài Gòn – Đà Lạt SGDL ngày 01/11/2026 và về ngày 04/11/2026, đi 3 người. Tôi đặt và trả tiền giường nằm, nhưng cả chặng đi và chặng về nhà xe đều đổi sang xe ghế ngồi. Tài xế lái xe cẩn thận, tôi không có gì phàn nàn về tài xế. Tôi đề nghị nhà xe giải quyết thỏa đáng.

</email>

Quy trình trích xuất :
- tre_chuyen: xe khởi hành trễ trên 1 giờ do nhà xe: tặng voucher 50.000 đồng/khách.
- sai_loai_xe: đổi sang loại xe khác loại khách đã đặt: hoàn 120.000 đồng/khách cho mỗi chặng bị đổi.
- tai_xe: khiếu nại về thái độ tài xế, phụ xe: tặng voucher 100.000 đồng/khách.
- khac: chuyển bộ phận Chăm sóc khách hàng xem xét, chưa đề xuất mức đền bù.
- Tổng mức đền bù trên 500.000 đồng phải được Trưởng phòng điều hành duyệt trước khi gửi khách.

Chỉ trả về đúng một đối tượng JSON hợp lệ. Không chào hỏi, không giải thích, không dùng markdown code fence.
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. Kiểm tra đủ  trường bắt buộc và kiểu dữ liệu của từng cái : Chỉ dùng chính sách trong thẻ <chinh_sach> để tính tổng mức đền bù. Trả về đúng một đối tượng JSON, không giải thích:
{"loai_su_co": ..., "muc_den_bu": tổng số tiền (đồng) hoặc null, "can_duyet": true nếu muc_den_bu trên 500.000, ngược lại false}

2. Kiểm tra giá trị phân loại khớp email: đây là sai_loai_xe, số khách là 3; không chọn nhãn liên quan đến nhân viên.

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Viết tiếp từ đúng vị trí "Nhà xe xin hoàn lại Quý khách khoản" trong đoạn chưa hoàn chỉnh sau đây. Không lặp lại hoặc viết lại phần đã có, chỉ thêm phần tiếp theo đến khi kết thúc email:

"…Nhà xe xin hoàn lại Quý khách khoản"

Khi viết tiếp, dùng đúng mức đền bù trong JSON đầu ra của Trạm 2, không tự thêm mức khác.
```



## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
Bạn là giám khảo chấm bài tư vấn cho nhân viên tư vấn tập sự của nhà xe. hãy chấm bài trong thẻ <bai_can_cham> dựa trên thông tin chuẩn và rubic dưới đây:

THÔNG TIN CHUẨN CỦA CÔNG TY
- Giá vé SGNT: 380.000 đồng/vé người lớn.
- Chính sách đổi vé: đổi trước giờ xe chạy từ 24 giờ trở lên miễn phí; từ 6 đến dưới 24 giờ phí 20% giá vé; dưới 6 giờ không được đổi.

RUBRIC CHẤM (thang 10)
- Đúng thông tin (50%): giá và chính sách đổi vé đúng với thông tin chuẩn.
- Đầy đủ (30%): trả lời đủ cả hai câu hỏi của khách và mời khách cung cấp ngày đi, số vé để đặt chỗ.
- Giọng điệu (20%): lịch sự, chuyên nghiệp, không dùng từ ngữ suồng sã.


```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Phương Nam", nhân viên tư vấn trên website của nhà xe Phương Nam Express, giúp khách chọn và đặt vé một trong 3 tuyến: SGDL (Sài Gòn – Đà Lạt), SGNT (Sài Gòn – Nha Trang), SGCT (Sài Gòn – Cần Thơ).

[Quy tắc ràng buộc]
1. chỉ tư vấn 3 tuyến xe của công ty
2. không hứa giảm giá ngoài bảng giá 
3. không hỏi xin số CCCD, số thẻ ngân hàng 

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- khách hỏi ngoài phạm vi : từ chối trả lời nội dung ngoài phạm vi một cách nhã nhặn
- khách cung thiêus thông tin: lịch sự nêu rõ những thông tin thiếu
