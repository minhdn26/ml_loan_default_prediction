# 04. Data Preparation

## 1. Mục tiêu (Objectives)
Chuyển đổi dữ liệu từ bước Data Understanding thành tập dữ liệu sạch (Cleaned Dataset) và sẵn sàng cho việc huấn luyện mô hình Baseline. Tập trung vào tính nhất quán và xử lý các lỗi logic.

## 2. Quy trình thực hiện (Data Pipeline)

### 2.1. Xử lý giá trị thiếu (Missing Values Imputation)
Dựa trên đặc điểm của từng loại biến, chúng ta áp dụng các chiến lược sau:
* **Biến số (Numerical):** `Income`, `CreditScore`, `InterestRate`, `DTIRatio`.
    * *Chiến lược:* Điền bằng giá trị **Median** (Trung vị) để tránh ảnh hưởng của Outliers.
* **Biến phân loại (Categorical):** `Education`, `EmploymentType`, `LoanPurpose`.
    * *Chiến lược:* Điền bằng giá trị **Mode** (Yếu vị - giá trị xuất hiện nhiều nhất) hoặc gắn nhãn `"Unknown"`.

### 2.2. Xử lý giá trị bất thường (Outliers Handling)
Đối với các biến như `Age`, `Income`, `LoanAmount`:
* Sử dụng phương pháp **IQR (Interquartile Range)** để xác định ngưỡng trên và ngưỡng dưới.
* *Hành động:* Thực hiện **Capping** (Giới hạn giá trị) thay vì xóa bỏ để giữ lại quy mô của tập dữ liệu.

### 2.3. Mã hóa dữ liệu (Encoding Categorical Variables)
Máy tính chỉ làm việc với số, do đó cần chuyển đổi các biến định tính:
* **Label Encoding (Thứ tự):** Áp dụng cho `Education` (High School < Bachelor < Master < PhD) vì các giá trị này có thứ bậc rõ ràng.
* **One-Hot Encoding (Định danh):** Áp dụng cho `EmploymentType`, `MaritalStatus`, `LoanPurpose`.
* **Binary Encoding:** Chuyển các biến `HasMortgage`, `HasCoSigner`, `HasDependents` về dạng `0` và `1`.

### 2.4. Kỹ thuật chia dữ liệu (Data Splitting)
Để đánh giá mô hình khách quan, dữ liệu được chia theo tỷ lệ:
* **Train set (80%):** Dùng để huấn luyện mô hình.
* **Test set (20%):** Dùng để đánh giá hiệu năng cuối cùng.
* **Lưu ý:** Sử dụng `Stratified Sampling` dựa trên biến `Default` để đảm bảo tỷ lệ nợ xấu ở cả hai tập là tương đương nhau.

---

## 3. Chiến lược xử lý mất cân bằng (Imbalance Strategy)
Vì đây là bản Baseline, chúng ta sẽ bắt đầu với phương pháp đơn giản nhất nhưng hiệu quả:
* **Cost-sensitive Learning:** Điều chỉnh trọng số (weight) của lớp `Default = 1` cao hơn trong thuật toán (ví dụ tham số `class_weight='balanced'` trong Scikit-learn).
* *Lý do:* Giúp mô hình tập trung hơn vào việc nhận diện các ca nợ xấu mà không cần sinh thêm dữ liệu ảo (như SMOTE) ở bước đầu.

---

## 4. Danh sách kiểm tra đầu ra (Output Checklist)

| Công việc | Trạng thái mong đợi |
| :--- | :--- |
| **Missing Values** | 0% missing trên tất cả các cột. |
| **Data Types** | Tất cả các cột phải là kiểu số (float, int). |
| **Inconsistent Labels** | Các giá trị như "fulltime" và "Full-time" đã được quy chuẩn hóa. |
| **Feature Scaling** | (Tùy chọn cho Baseline) Áp dụng `StandardScaler` nếu dùng Logistic Regression. |

---

## 5. Cấu trúc mã nguồn (Notebook Snippet)

Trong file `04_data_preparation.ipynb`, cấu trúc code chính sẽ như sau:

```python
# Ví dụ xử lý biến Education có thứ tự
education_map = {'High School': 0, 'Bachelor': 1, 'Master': 2, 'PhD': 3}
df['Education'] = df['Education'].map(education_map)

# Chia tập dữ liệu với tính năng giữ nguyên tỷ lệ nợ xấu
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
```

---