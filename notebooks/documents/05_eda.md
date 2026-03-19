# 05. Exploratory Data Analysis (EDA)

## 1. Mục tiêu (Objectives)
* Hiểu rõ phân phối của từng biến độc lập (Univariate Analysis).
* Tìm mối tương quan giữa các đặc trưng và biến mục tiêu `Default` (Bivariate Analysis).
* Phát hiện các nhóm khách hàng có rủi ro cao thông qua kết hợp nhiều biến (Multivariate Analysis).
* Lựa chọn các biến có "lực dự báo" mạnh nhất cho mô hình Baseline.

## 2. Các nội dung phân tích trọng tâm

### 2.1. Phân tích đơn biến (Univariate Analysis)
Chúng ta cần xem xét hình dáng của dữ liệu:
* **Biến số (Numerical):** Sử dụng **Histogram** và **Boxplot** cho `Income`, `LoanAmount`, `CreditScore`.
    * *Câu hỏi:* Dữ liệu có bị lệch (skewed) không? Có quá nhiều người thu nhập thấp không?
* **Biến phân loại (Categorical):** Sử dụng **Countplot** cho `Education`, `EmploymentType`, `LoanPurpose`.
    * *Câu hỏi:* Mục đích vay nào chiếm đa số? Tỷ lệ nợ xấu (`Default`) tổng thể là bao nhiêu %?

### 2.2. Phân tích hai biến (Bivariate Analysis) - Tìm kiếm "Tín hiệu rủi ro"
Đây là phần quan trọng nhất để hiểu tại sao khách hàng vỡ nợ:
* **CreditScore vs Default:** Sử dụng **KDE Plot** hoặc **Boxplot**.
    * *Kỳ vọng:* Những người vỡ nợ (`Default=1`) sẽ có vùng tập trung `CreditScore` thấp hơn rõ rệt.
* **Income vs Default:** Kiểm tra xem thu nhập cao có thực sự tỷ lệ nghịch với vỡ nợ không.
* **LoanPurpose vs Default:** Sử dụng **Stacked Bar Chart**.
    * *Câu hỏi:* Vay để "Kinh doanh" (Business) có rủi ro hơn vay để "Giáo dục" (Education) không?
* **HasCoSigner vs Default:** So sánh tỷ lệ nợ xấu giữa nhóm có và không có người bảo lãnh.



### 2.3. Phân tích đa biến (Multivariate Analysis)
* **Heatmap Correlation:** Xem xét mối tương quan giữa tất cả các biến số.
    * *Lưu ý:* Nếu `LoanAmount` và `Income` quá tương quan với nhau (Multicollinearity), chúng ta có thể chỉ cần giữ lại một biến hoặc dùng tỷ lệ `DTIRatio`.
* **DTI vs CreditScore vs Default:** Sử dụng **Scatter Plot** với màu sắc phân biệt theo `Default`.
    * *Mục tiêu:* Tìm ra "vùng đỏ" (vùng nguy hiểm) – ví dụ: DTI cao kết hợp Credit Score thấp.



---

## 3. Các giả thuyết cần kiểm chứng (Hypothesis Testing)

| Giả thuyết | Phép trực quan hóa đề xuất |
| :--- | :--- |
| **H1:** Điểm tín dụng càng cao, xác suất vỡ nợ càng thấp. | Line Plot (Binning CreditScore) |
| **H2:** Người có người bảo lãnh (`HasCoSigner`) ít vỡ nợ hơn. | Bar Chart (Proportion of Default) |
| **H3:** Tỷ lệ nợ trên thu nhập (`DTIRatio`) trên 0.4 là ngưỡng rủi ro. | Histogram (phân lớp theo Default) |

---

## 4. Công cụ & Kỹ thuật (Implementation)

Trong file `05_eda.ipynb`, chúng ta sẽ sử dụng thư viện `Seaborn` và `Matplotlib`:

```python
import seaborn as sns
import matplotlib.pyplot as plt

# 1. Kiểm tra tỷ lệ nợ xấu theo mục đích vay
sns.barplot(x='LoanPurpose', y='Default', data=df)
plt.title('Tỷ lệ vỡ nợ theo Mục đích vay')
plt.show()

# 2. Phân phối điểm tín dụng giữa 2 nhóm khách hàng
sns.kdeplot(data=df, x='CreditScore', hue='Default', shade=True)
plt.title('Phân phối Credit Score theo Trạng thái Default')
plt.show()
```

---

## 5. Kết luận mong đợi sau EDA
Kết thúc bước này, chúng ta phải trả lời được:
1. Những biến nào ảnh hưởng trực tiếp nhất đến `Default`?
2. Có cần tạo thêm biến mới (Feature Engineering) từ các phát hiện này không?
3. Có các giá trị ngoại lai nào cần xử lý triệt để ở bước sau không?

---