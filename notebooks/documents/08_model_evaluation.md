# 08. Model Evaluation

## 1. Mục tiêu (Objectives)
* Đánh giá khả năng tổng quát hóa (Generalization) của mô hình trên dữ liệu chưa từng thấy (Test set).
* Phân tích sự đánh đổi giữa các sai số (Error Analysis).
* Lựa chọn **Ngưỡng cắt (Classification Threshold)** tối ưu để cân bằng giữa lợi nhuận và rủi ro.

## 2. Các chỉ số đánh giá kỹ thuật (ML Metrics)

Vì dữ liệu nợ xấu thường mất cân bằng, chúng ta tập trung vào các chỉ số sau:

### 2.1. ROC-AUC (Area Under the Receiver Operating Characteristic Curve)
* **Ý nghĩa:** Khả năng phân loại giữa khách hàng Tốt và Xấu. 
* **Mục tiêu:** Càng gần 1.0 càng tốt. (Mức > 0.75 là có thể sử dụng, > 0.85 là xuất sắc).

### 2.2. Precision & Recall (PR Curve)
Đây là "bài toán kinh tế" của ngân hàng:
* **Recall (Sensitivity):** Trong 100 người thực sự vỡ nợ, mô hình bắt được bao nhiêu người? 
    * *Hệ quả:* Recall thấp = Ngân hàng mất vốn.
* **Precision:** Trong 100 người mô hình dự báo vỡ nợ, có bao nhiêu người thực sự vỡ nợ?
    * *Hệ quả:* Precision thấp = Ngân hàng từ chối nhầm khách hàng tốt, mất doanh thu từ lãi.

### 2.3. Confusion Matrix (Ma trận nhầm lẫn)
Giúp quan sát trực quan 4 kịch bản:
* **True Positive (TP):** Bắt đúng nợ xấu (Thành công).
* **True Negative (TN):** Phê duyệt đúng người tốt (Thành công).
* **False Positive (FP):** Từ chối nhầm người tốt (Mất cơ hội).
* **False Negative (FN):** Cho vay nhầm người xấu (**Rủi ro lớn nhất**).

---

## 3. Đánh giá dưới góc độ Business (Business Impact)

### 3.1. Tìm kiếm Optimal Threshold
Mặc định mô hình dùng ngưỡng 0.5, nhưng trong tài liệu này, chúng ta sẽ thực hiện **Threshold Tuning**:
* Nếu chi phí của một ca `FN` (mất vốn) gấp 10 lần chi phí của một ca `FP` (mất lãi), chúng ta sẽ hạ thấp ngưỡng dự báo (ví dụ 0.3) để ưu tiên tăng **Recall**.

### 3.2. Đồ thị Cumulative Gains & Lift Chart
Giúp Credit Risk Team trả lời câu hỏi: *"Nếu tôi chỉ kiểm tra 20% danh sách khách hàng có rủi ro cao nhất từ mô hình, tôi sẽ tóm gọn được bao nhiêu % tổng số nợ xấu?"*

---

## 4. Công cụ & Trực quan hóa (Implementation)

Trong file `08_model_evaluation.ipynb`, chúng ta sẽ thực hiện như sau:

```python
from sklearn.metrics import confusion_matrix, classification_report, roc_curve, auc

# 1. Vẽ đường ROC
fpr, tpr, thresholds = roc_curve(y_test, y_probs)
roc_auc = auc(fpr, tpr)

# 2. Báo cáo chi tiết Precision/Recall
print(classification_report(y_test, y_preds))

# 3. Trực quan hóa Confusion Matrix
sns.heatmap(confusion_matrix(y_test, y_preds), annot=True, fmt='d', cmap='Blues')
plt.xlabel('Dự báo')
plt.ylabel('Thực tế')
```

---

## 5. Danh sách kiểm tra (Checklist)
* [ ] Mô hình có bị Overfitting không? (Hiệu năng tập Train và Test có chênh lệch quá 5-10% không?)
* [ ] Precision-Recall có đạt mức Business chấp nhận được không?
* [ ] Đã so sánh kết quả giữa Baseline (Logistic) và Challenger (XGBoost) chưa?

---

## 6. Kết luận giai đoạn
Sau bước này, chúng ta sẽ chọn ra **"Final Model"** để đưa vào sản xuất. Một bản báo cáo hiệu năng (Performance Report) sẽ được gửi cho các Stakeholders (Credit Risk Team) để xin phê duyệt.

---