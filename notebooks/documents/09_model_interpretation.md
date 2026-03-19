# 09. Model Interpretation

## 1. Mục tiêu (Objectives)
* Xác định các yếu tố quan trọng nhất dẫn đến rủi ro vỡ nợ (Global Interpretability).
* Giải thích lý do tại sao một khách hàng cụ thể bị hệ thống đánh giá là rủi ro cao/thấp (Local Interpretability).
* Xây dựng niềm tin với các Stakeholders (Credit Risk Team, Compliance).
* Phát hiện các sai lệch tiềm ẩn (Bias) trong mô hình.

## 2. Các phương pháp giải thích mô hình

Chúng ta sẽ sử dụng 3 cấp độ tiếp cận:

### 2.1. Feature Importance (Global)
* **Mô tả:** Xếp hạng các đặc trưng dựa trên đóng góp tổng thể của chúng vào khả năng dự báo của mô hình.
* **Câu hỏi trả lời:** "Nhìn chung, yếu tố nào ảnh hưởng nhất đến nợ xấu?" (Ví dụ: `CreditScore` là quan trọng nhất, sau đó là `DTIRatio`).
* **Công cụ:** Built-in `feature_importances_` của Random Forest hoặc XGBoost.

### 2.2. SHAP Values (Shapley Additive Explanations)
* **Mô tả:** Dựa trên lý thuyết trò chơi, SHAP tính toán đóng góp của từng đặc trưng vào giá trị dự báo cuối cùng.
* **Ưu điểm:** Cho thấy hướng tác động (Tăng hay giảm rủi ro).
    * *Ví dụ:* Một điểm SHAP dương cho `LoanAmount` nghĩa là số tiền vay lớn đang đẩy khách hàng về phía vỡ nợ.



### 2.3. Partial Dependence Plots (PDP)
* **Mô tả:** Cho thấy mối liên hệ giữa một đặc trưng và xác suất vỡ nợ khi các yếu tố khác được giữ cố định.
* **Câu hỏi trả lời:** "Khi `Income` tăng lên, xác suất `Default` giảm xuống theo đường cong như thế nào?"

## 3. Ứng dụng trong Business (Actionable Insights)

### 3.1. Lý do từ chối (Adverse Action Reasons)
Khi một hồ sơ bị mô hình đánh giá rủi ro > Threshold, hệ thống cần trích xuất top 3 lý do dựa trên SHAP:
1. `CreditScore` quá thấp (< 600).
2. Tỷ lệ nợ trên thu nhập (`DTIRatio`) quá cao (> 0.5).
3. Thời gian làm việc (`MonthsEmployed`) quá ngắn.

### 3.2. Kiểm tra tính công bằng (Fairness Check)
Đảm bảo mô hình không vô tình phân biệt đối xử dựa trên các yếu tố nhạy cảm như `Age` hay `MaritalStatus`. Nếu `Age` có tầm quan trọng quá cao một cách phi lý, cần xem xét lại Feature Engineering.

---

## 4. Công cụ & Thực thi (Implementation)

Trong file `09_model_interpretation.ipynb`, chúng ta sẽ sử dụng thư viện `SHAP`:

```python
import shap

# 1. Khởi tạo SHAP Explainer cho mô hình đã huấn luyện
explainer = shap.TreeExplainer(final_model)
shap_values = explainer.shap_values(X_test)

# 2. Vẽ biểu đồ Summary (Global)
shap.summary_plot(shap_values, X_test)

# 3. Giải thích cho 1 khách hàng cụ thể (Local)
# Giả sử giải thích cho khách hàng đầu tiên trong tập test
shap.initjs()
shap.force_plot(explainer.expected_value, shap_values[0,:], X_test.iloc[0,:])
```

---

## 5. Kết luận dự án (Final Project Conclusion)

Sau khi hoàn thành bước 09, chúng ta đã có:
1. Một mô hình Baseline (Random Forest/XGBoost) đạt hiệu năng tốt trên các chỉ số tài chính.
2. Một Pipeline tiền xử lý dữ liệu sạch sẽ, không bị Data Leakage.
3. Khả năng giải thích tường minh cho mọi quyết định phê duyệt/từ chối.

---