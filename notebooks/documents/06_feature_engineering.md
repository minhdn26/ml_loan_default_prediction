# 06. Feature Engineering

## 1. Mục tiêu (Objectives)
* Tạo ra các đặc trưng mới (Derived Features) có khả năng phân loại nợ xấu tốt hơn dữ liệu thô.
* Biến đổi các đặc trưng hiện có để phù hợp với thuật toán (Scaling & Transformation).
* Lựa chọn các đặc trưng quan trọng nhất (Feature Selection) để giữ cho mô hình Baseline gọn nhẹ và dễ giải thích.

## 2. Chiến lược tạo đặc trưng mới (Feature Creation)

Dựa trên các trường thông tin thực tế, chúng ta sẽ tập trung vào 3 nhóm chỉ số tài chính cốt lõi:

### 2.1. Nhóm chỉ số Gánh nặng nợ (Debt Burden)
* **MonthlyIncome:** `Income / 12`. (Chuyển thu nhập năm về tháng để khớp với kỳ thanh toán).
* **MonthlyLoanPayment:** Ước tính số tiền phải trả mỗi tháng dựa trên `LoanAmount`, `InterestRate`, và `LoanTerm`.
* **TotalMonthlyDebt:** `MonthlyIncome * DTIRatio`. (Tổng số tiền khách hàng đang trả cho tất cả các khoản nợ hàng tháng).

### 2.2. Nhóm chỉ số Đòn bẩy & Rủi ro (Leverage & Risk)
* **LoanToIncomeRatio:** `LoanAmount / Income`. (Khoản vay gấp bao nhiêu lần thu nhập năm? Tỷ lệ này quá cao là dấu hiệu rủi ro).
* **CreditUtilization (Ước tính):** `NumCreditLines / (MonthsEmployed / 12 + 1)`. (Tốc độ mở hạn mức tín dụng theo thời gian làm việc).
* **AgeExperienceRatio:** `MonthsEmployed / (Age * 12)`. (Tỷ lệ thời gian đi làm so với tuổi đời - đo lường sự ổn định trong sự nghiệp).

### 2.3. Nhóm tương tác (Interaction Features)
* **ScorePerIncome:** `CreditScore / Income`. (Hiệu suất tín dụng trên mỗi đơn vị thu nhập).
* **HighRiskCombo:** Tạo biến nhị phân (1/0) nếu đồng thời có `CreditScore < 600` và `DTIRatio > 0.45`.

---

## 3. Biến đổi đặc trưng (Feature Transformation)

* **Scaling (Chuẩn hóa):** Vì chúng ta sử dụng `InterestRate` (đơn vị %) và `Income` (đơn vị hàng chục triệu), cần dùng **StandardScaler** hoặc **MinMaxScaler** để đưa về cùng một thang đo (đặc biệt quan trọng nếu dùng Logistic Regression hoặc SVM).
* **Binning (Nhóm khoảng):** Biến đổi `Age` thành các nhóm: `Young` (18-25), `Middle` (26-45), `Senior` (>45). Đôi khi rủi ro nợ xấu tập trung theo nhóm tuổi thay vì tuyến tính theo số tuổi.

---

## 4. Lựa chọn đặc trưng (Feature Selection)

Sau khi tạo ra rất nhiều biến, chúng ta cần lọc lại để tránh "nhiễu" cho mô hình Baseline:
1.  **Correlation Analysis:** Loại bỏ các biến có tương quan quá cao với nhau ($> 0.85$) để tránh đa cộng tuyến.
2.  **Feature Importance (Sơ bộ):** Chạy một mô hình Random Forest nhanh để xem biến nào có đóng góp cao nhất cho `Default`.
3.  **Lọc biến ID:** Loại bỏ hoàn toàn `LoanID` khỏi quá trình huấn luyện.

---

## 5. Cấu trúc mã nguồn (Notebook Snippet)

Trong file `06_feature_engineering.ipynb`, bạn sẽ triển khai như sau:

```python
# 1. Tạo biến tỷ lệ vay trên thu nhập
df['LoanToIncome'] = df['LoanAmount'] / df['Income']

# 2. Ước tính số tiền trả nợ hàng tháng (giả định trả góp đều)
# Công thức đơn giản cho Baseline: (Gốc / Kỳ hạn) + (Gốc * Lãi suất tháng)
df['MonthlyPayment'] = (df['LoanAmount'] / df['LoanTerm']) + (df['LoanAmount'] * (df['InterestRate']/100) / 12)

# 3. Kết hợp đặc trưng rủi ro
df['Is_High_Risk_Profile'] = ((df['CreditScore'] < 600) & (df['DTIRatio'] > 0.4)).astype(int)
```

---

## 6. Kết luận giai đoạn
Sau bước này, tập dữ liệu của bạn sẽ không còn là 18 cột thô nữa mà là một tập hợp các đặc trưng "giàu thông tin" (Information-rich features). Đây là nền tảng để mô hình ở bước 07 đạt được ROC-AUC cao ngay từ lần chạy đầu tiên.