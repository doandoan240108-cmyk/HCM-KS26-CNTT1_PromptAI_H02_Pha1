# BÀI LÀM PHA 2 – HACKATHON 02

- **Mã đề:** 04
- **Họ tên:** Đoàn Bảo Duy
- **Lớp:** HCM-
- **Link repository Pha2:**
- **Công cụ AI đã dùng (ChatGPT/Claude/Gemini):**

> Pha 2 (20 phút cuối). Mỗi mục: mở cuộc trò chuyện mới, gửi nguyên văn prompt đã viết trong bai_lam_pha1.md, dán câu trả lời và link chia sẻ vào đây. Xong, kéo thả file này vào repository Pha2.

## 4.2. Trích xuất tin nhắn đặt vé xe ra JSON

**KET QUA AI**

```text
[{"ma_tuyen": "SGDL", "ngay_di": "2026-12-22", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "giuong", "ghi_chu": null}, {"ma_tuyen": "SGNT", "ngay_di": "2027-01-02", "nguoi_lon": 3, "tre_em": 0, "loai_ve": null, "ghi_chu": "một người đang mang thai 6 tháng"}, {"ma_tuyen": "SGCT", "ngay_di": null, "nguoi_lon": 4, "tre_em": 0, "loai_ve": "ghe", "ghi_chu": "bà nội đi xe lăn"}]
```

**Link trò chuyện:** https://share.gemini.google/OiLUxAlkm4Es

## 4.3. Báo giá bằng Chain-of-Thought

**KET QUA AI**

```text
```python
adult_price = 320000
num_adults = 8
num_children_5_10 = 3 # ages 6, 7, 9
num_infants = 1 # age 2
luggage = 2

step1 = adult_price * num_adults
step2 = (adult_price * 0.75) * num_children_5_10
step3 = step1 + step2
step4 = step3 * 0.95 # 8 >= 6 adults -> 5% discount
step5 = 100000 * luggage
step6 = step4 + step5

print(f"Step 1: {step1}")
print(f"Step 2: {step2}")
print(f"Step 3: {step3}")
print(f"Step 4: {step4}")
print(f"Step 5: {step5}")
print(f"Step 6: {step6}")


```

```text
Step 1: 2560000
Step 2: 720000.0
Step 3: 3280000.0
Step 4: 3116000.0
Step 5: 200000
Step 6: 3316000.0


```

Bước 1: Tính tiền vé người lớn
8 × 320.000 = 2.560.000 đồng

Bước 2: Tính tiền vé trẻ em (3 trẻ từ 5–10 tuổi tính 75% giá vé; 1 bé 2 tuổi miễn phí)
3 × (320.000 × 75%) + 1 × 0 = 3 × 240.000 + 0 = 720.000 đồng

Bước 3: Tính tiền vé
2.560.000 + 720.000 = 3.280.000 đồng

Bước 4: Tính tiền vé giảm giá đoàn (đoàn 8 người lớn ≥ 6 người lớn nên được giảm 5%)
3.280.000 × (100% − 5%) = 3.280.000 × 95% = 3.116.000 đồng

Bước 5: Tính tiền phụ thu (2 kiện hành lý cồng kềnh)
2 × 100.000 = 200.000 đồng

Bước 6: Tính tổng tiền
3.116.000 + 200.000 = 3.316.000 đồng

[RESULT:3.316.000 đồng]


**Link trò chuyện:**
https://share.gemini.google/ffsurekfWnyh

### b) Kiểm tra kết quả AI

(Ghi kết luận AI đúng hay sai; nếu sai, ghi bước sai đầu tiên và kết quả đúng của bước đó – xoá dòng này)


## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
{
  "loai_su_co": "sai_loai_xe",
  "so_khach": 3,
  "so_chang_bi_doi": 2,
  "tom_tat": "Khách đi 3 người tuyến Sài Gòn – Đà Lạt ngày 01/11/2026 và về ngày 04/11/2026, đã đặt và thanh toán giường nằm nhưng cả hai chặng đi và về đều bị đổi sang xe ghế ngồi; không phàn nàn về tài xế."
}
```

**Link trò chuyện:**
https://share.gemini.google/B4iMy9Djatos

## 4.5. Prompt chấm bài theo Rubric

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
```

**Link trò chuyện:**
