# 07. Model Development

## 1. Mục tiêu (Objectives)
* Huấn luyện các mô hình Machine Learning đa dạng để so sánh hiệu năng.
* Tối ưu hóa tham số (Hyperparameter Tuning) cho mô hình Baseline.
* Xây dựng Pipeline hoàn chỉnh từ khâu Tiền xử lý đến Dự báo để đảm bảo tính nhất quán (tránh Data Leakage).

## 2. Lựa chọn thuật toán (Algorithm Selection)
Với bài toán phân loại nợ xấu (Binary Classification) trên dữ liệu bảng (Tabular Data), chúng ta sẽ thử nghiệm 3 nhóm mô hình từ đơn giản đến phức tạp:

| Thuật toán | Ưu điểm | Vai trò |
| :--- | :--- | :--- |
| **Logistic Regression** | Cực kỳ dễ giải thích, chạy nhanh, là chuẩn mực trong ngành ngân hàng. | **Baseline 1** |
| **Random Forest** | Khả năng bắt các mối quan hệ phi tuyến, không nhạy cảm với Outliers. | **Baseline 2** |
| **XGBoost / LightGBM** | Hiệu năng cao nhất cho dữ liệu bảng, xử lý tốt dữ liệu mất cân bằng. | **Challenger** |



## 3. Quy trình huấn luyện (Training Workflow)

### 3.1. Thiết lập Cross-Validation
Để đảm bảo mô hình không bị Overfitting (học vẹt), chúng ta sử dụng **Stratified K-Fold Cross-Validation** (thường là $K=5$). Phương pháp này giữ nguyên tỷ lệ `Default` trong mỗi fold, giúp đánh giá rủi ro khách quan hơn.

### 3.2. Xử lý mất cân bằng tại tầng Model
Thay vì sinh thêm dữ liệu, chúng ta sử dụng tham số trọng số lớp:
* `class_weight='balanced'` (trong Scikit-learn).
* `scale_pos_weight` (trong XGBoost).
* *Mục đích:* Phạt mô hình nặng hơn khi dự đoán sai một ca vỡ nợ (Default=1).

### 3.3. Tối ưu hóa tham số (Hyperparameter Tuning)
Sử dụng `RandomizedSearchCV` hoặc `Optuna` để tìm bộ thông số tốt nhất:
* **Random Forest:** `n_estimators`, `max_depth`, `min_samples_split`.
* **XGBoost:** `learning_rate`, `subsample`, `colsample_bytree`.

---

## 4. Xây dựng ML Pipeline
Chúng ta sẽ đóng gói toàn bộ quy trình vào một `Pipeline` để dễ dàng triển khai (Deployment) sau này:

```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder

# 1. Định nghĩa các nhóm cột
numeric_features = ['Age', 'Income', 'CreditScore', 'LoanAmount', 'DTIRatio']
categorical_features = ['EmploymentType', 'LoanPurpose', 'HasCoSigner']

# 2. Tạo bộ tiền xử lý (Preprocessing)
preprocessor = ColumnTransformer(
    transformers=[
        ('num', StandardScaler(), numeric_features),
        ('cat', OneHotEncoder(handle_unknown='ignore'), categorical_features)
    ])

# 3. Tạo Pipeline hoàn chỉnh
pipeline = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('classifier', RandomForestClassifier(class_weight='balanced', random_state=42))
])

# 4. Huấn luyện
pipeline.fit(X_train, y_train)
```

---

## 5. Lưu trữ mô hình (Model Versioning)
Sau khi huấn luyện, các mô hình đạt kết quả tốt sẽ được lưu vào thư mục `models/` dưới định dạng `.joblib` hoặc `.pkl` kèm theo version để đối chiếu sau này (ví dụ: `rf_baseline_v1.joblib`).

---

## 6. Kết luận giai đoạn
Kết thúc bước này, chúng ta sẽ có danh sách các mô hình kèm theo kết quả dự báo trên tập Train/Validation. Chúng ta chưa kết luận mô hình nào tốt nhất cho đến khi thực hiện bước tiếp theo.