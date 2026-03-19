# Loan Default Prediction

Dự án **Machine Learning end-to-end** nhằm dự đoán khả năng **khách hàng không trả được nợ (loan default)**.

Mục tiêu của dự án là minh họa **quy trình phát triển một hệ thống Machine Learning hoàn chỉnh**, bắt đầu từ **phân tích yêu cầu business → xây dựng mô hình → đánh giá và giải thích mô hình**.

Toàn bộ dự án được triển khai bằng **Jupyter Notebook**, trong đó mỗi giai đoạn của pipeline ML sẽ được tách thành:

* **File Markdown** → mô tả công việc cần thực hiện
* **Notebook** → triển khai code và phân tích dữ liệu

---

# Flow tổng thể của dự án

Dự án tuân theo quy trình chuẩn của một dự án Machine Learning:

```
Business Understanding
        ↓
Problem Framing in ML
        ↓
Data Understanding
        ↓
Data Preparation
        ↓
Exploratory Data Analysis (EDA)
        ↓
Feature Engineering
        ↓
Model Development
        ↓
Model Evaluation
        ↓
Model Interpretation
```

Mỗi giai đoạn đều có:

```
documents/   → mô tả công việc
code/        → notebook thực thi
```

---

# 1. Mô tả yêu cầu của Business

## Bối cảnh

Các tổ chức tài chính (ngân hàng, công ty tài chính) cung cấp các khoản vay cho khách hàng cá nhân.

Tuy nhiên, một số khách hàng **không có khả năng trả nợ đúng hạn**, dẫn đến tình trạng **loan default**.

Điều này gây ra nhiều tổn thất cho tổ chức tài chính, bao gồm:

* Mất tiền gốc
* Mất doanh thu từ lãi
* Chi phí thu hồi nợ
* Gia tăng rủi ro danh mục tín dụng

---

## Bài toán Business

Làm thế nào để **dự đoán sớm khả năng vỡ nợ của khách hàng** trước khi quyết định cấp khoản vay?

Nếu có thể dự đoán được khách hàng có rủi ro cao, tổ chức tài chính có thể:

* từ chối khoản vay
* điều chỉnh hạn mức
* tăng lãi suất
* yêu cầu thêm tài sản đảm bảo

---

## Mục tiêu Business

Xây dựng một hệ thống giúp:

* **đánh giá mức độ rủi ro của khách hàng**
* hỗ trợ **quyết định phê duyệt khoản vay**
* **giảm tỷ lệ vỡ nợ**

---

## Giá trị mang lại

Nếu hệ thống hoạt động tốt, tổ chức tài chính có thể:

* giảm tỷ lệ nợ xấu
* cải thiện chất lượng danh mục tín dụng
* giảm tổn thất tài chính
* hỗ trợ ra quyết định dựa trên dữ liệu

---

# 2. Problem Framing in Machine Learning

## Định nghĩa bài toán ML

Bài toán được định nghĩa là **Binary Classification**.

Biến mục tiêu:

```
default = 1 → khách hàng vỡ nợ
default = 0 → khách hàng trả được nợ
```

Mô hình sẽ dự đoán:

```
P(default | thông tin khách hàng)
```

---

## Input của mô hình

Các đặc trưng có thể bao gồm:

* tuổi
* thu nhập
* nghề nghiệp
* lịch sử tín dụng
* số tiền vay
* lãi suất
* tỷ lệ nợ trên thu nhập
* lịch sử thanh toán

---

## Output của mô hình

Kết quả dự đoán:

```
xác suất khách hàng vỡ nợ
```

Ví dụ:

| Khách hàng | P(default) | Đánh giá    |
| ---------- | ---------- | ----------- |
| A          | 0.85       | rủi ro cao  |
| B          | 0.12       | rủi ro thấp |

---

## Performance Metric (ML)

Đây là các chỉ số đánh giá **hiệu quả của mô hình Machine Learning**.

### ROC-AUC

Đánh giá khả năng phân biệt giữa:

* khách hàng vỡ nợ
* khách hàng không vỡ nợ

Giá trị càng cao → mô hình càng tốt.

---

### Precision

```
Trong các khách hàng được dự đoán sẽ vỡ nợ,
bao nhiêu người thực sự vỡ nợ?
```

---

### Recall

```
Trong các khách hàng thực sự vỡ nợ,
mô hình phát hiện được bao nhiêu?
```

Recall quan trọng vì **bỏ sót khách hàng rủi ro cao có thể gây tổn thất lớn**.

---

### F1 Score

Chỉ số cân bằng giữa:

* Precision
* Recall

---

## Business Metric

Trong thực tế, **ML metric không phải là mục tiêu cuối cùng**.

Điều quan trọng hơn là **impact đến business**.

---

### Tỷ lệ nợ xấu (Default Rate)

```
Default Rate = Số khoản vay vỡ nợ / Tổng số khoản vay
```

Mục tiêu là **giảm tỷ lệ này**.

---

### Tổn thất tài chính

Loan default gây ra:

```
Loss = tiền gốc chưa trả + lãi bị mất
```

Mô hình tốt giúp **giảm tổn thất kỳ vọng**.

---

### Chất lượng phê duyệt khoản vay

Hệ thống giúp:

* phê duyệt khách hàng rủi ro thấp
* hạn chế khách hàng rủi ro cao

---

## Chiến lược ra quyết định

Mô hình thường được sử dụng theo dạng:

```
Nếu P(default) > threshold
    → khách hàng rủi ro cao
    → cần review thêm
Ngược lại
    → có thể phê duyệt
```

Việc lựa chọn **threshold** phụ thuộc vào:

* khẩu vị rủi ro của tổ chức
* chi phí của false positive / false negative

---

# 3. Data Understanding

## Mục tiêu

Hiểu rõ dữ liệu trước khi xây dựng mô hình.

Các hoạt động chính:

* kiểm tra cấu trúc dữ liệu
* phân tích kiểu dữ liệu
* phát hiện missing values
* kiểm tra chất lượng dữ liệu

---

## Input

```
Raw dataset
```

---

## Output

```
Data profiling
Data quality report
Mô tả các biến trong dataset
```

---

# 4. Data Preparation

## Mục tiêu

Chuẩn bị dữ liệu cho bước phân tích và modeling.

Các bước chính:

* xử lý missing values
* chuẩn hóa kiểu dữ liệu
* lọc dữ liệu lỗi
* chia tập train / test

---

## Input

```
Raw dataset
```

---

## Output

```
Clean dataset
Train / Test dataset
```

---

# 5. Exploratory Data Analysis (EDA)

## Mục tiêu

Khám phá dữ liệu để tìm ra:

* pattern
* mối quan hệ giữa biến
* tín hiệu dự đoán

---

## Input

```
Clean dataset
```

---

## Output

```
Insight từ dữ liệu
Biểu đồ phân tích
Các biến tiềm năng cho mô hình
```

---

# 6. Feature Engineering

## Mục tiêu

Tạo thêm các biến giúp mô hình học tốt hơn.

Ví dụ:

```
Debt-to-Income Ratio
Loan-to-Income Ratio
Credit Utilization
```

---

## Input

```
Clean dataset
```

---

## Output

```
Feature engineered dataset
```

---

# 7. Model Development

## Mục tiêu

Huấn luyện các mô hình Machine Learning.

Các thuật toán có thể thử:

* Logistic Regression
* Random Forest
* Gradient Boosting
* XGBoost

---

## Input

```
Feature dataset
```

---

## Output

```
Trained models
Prediction results
```

---

# 8. Model Evaluation

## Mục tiêu

Đánh giá hiệu năng của mô hình.

Các bước:

* ROC curve
* confusion matrix
* precision / recall
* phân tích threshold

---

## Input

```
Prediction results
Test dataset
```

---

## Output

```
Báo cáo đánh giá mô hình
Mô hình tốt nhất
```

---

# 9. Model Interpretation

## Mục tiêu

Giải thích cách mô hình đưa ra quyết định.

Các phương pháp:

* Feature Importance
* SHAP values
* Partial Dependence

---

## Input

```
Trained model
Test dataset
```

---

## Output

```
Giải thích mô hình
Các yếu tố ảnh hưởng đến rủi ro vỡ nợ
```

---

# Cấu trúc repository

```
loan-default-prediction/
│
├── data/
│   ├── raw
│   └── processed
│
├── notebooks/
│
│   ├── documents/
│   │
│   │   01_business_understanding.md
│   │   02_problem_framing.md
│   │   03_data_understanding.md
│   │   ...
│
│   ├── code/
│   │
│   │   01_business_understanding.ipynb
│   │   02_problem_framing.ipynb
│   │   03_data_understanding.ipynb
│   │   ...
│
├── src/
├── models/
└── reports/
```
---