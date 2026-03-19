# 02. Problem Framing in Machine Learning

## 1. Định nghĩa bài toán (ML Task)
Dựa trên yêu cầu dự đoán khả năng vỡ nợ (Loan Default), đây là bài toán **Supervised Learning** (Học có giám sát) thuộc dạng **Binary Classification** (Phân loại nhị phân).

* **Biến mục tiêu (Target Variable):** `is_default`
    * `1`: Khách hàng không trả được nợ (vỡ nợ).
    * `0`: Khách hàng trả nợ đúng hạn.
* **Output mong muốn:** Một giá trị xác suất $P(y=1 | X)$ nằm trong khoảng $[0, 1]$. Xác suất này đại diện cho mức độ rủi ro của khoản vay.

## 2. Dữ liệu đầu vào (Features)
Để mô hình Baseline có thể hoạt động, chúng ta phân loại các đặc trưng đầu vào thành 4 nhóm chính:

| Nhóm đặc trưng | Ví dụ các biến cụ thể | Ý nghĩa |
| :--- | :--- | :--- |
| **Demographic** | Tuổi, Học vấn, Tình trạng hôn nhân, Số người phụ thuộc | Phác họa chân dung khách hàng. |
| **Financial** | Thu nhập năm, Nghề nghiệp, Nguồn thu nhập | Khả năng tài chính hiện tại. |
| **Loan Details** | Số tiền vay, Lãi suất, Thời hạn vay, Mục đích vay | Đặc điểm của chính khoản vay đó. |
| **Credit History** | Điểm tín dụng (Credit Score), Số lần quá hạn trong quá khứ | Hành vi trả nợ trong lịch sử. |

## 3. Metrics đánh giá (Evaluation Metrics)

### ML Metrics
Trong bài toán tín dụng, dữ liệu thường bị **mất cân bằng (imbalanced)** (số người vỡ nợ luôn ít hơn người trả nợ tốt). Do đó, không sử dụng Accuracy.
* **ROC-AUC:** Đo lường khả năng phân biệt giữa hai nhóm của mô hình.
* **Precision:** Trong số những người mô hình bảo sẽ vỡ nợ, có bao nhiêu người thực sự vỡ nợ? (Tránh việc từ chối nhầm khách hàng tốt).
* **Recall (Quan trọng nhất):** Trong số những người thực tế vỡ nợ, mô hình bắt được bao nhiêu người? (Tránh việc cho vay nhầm người xấu).
* **F1-Score:** Cân bằng giữa Precision và Recall.

### Business Metrics
* **Default Rate Reduction:** Tỷ lệ nợ xấu giảm bao nhiêu % sau khi áp dụng mô hình?
* **Approval Rate:** Tỷ lệ phê duyệt thay đổi thế nào để đảm bảo doanh số kinh doanh?



## 4. Ràng buộc và Giả định (Constraints & Assumptions)
* **Tính giải thích (Interpretability):** Mô hình phải giải thích được *tại sao* khách hàng bị đánh giá rủi ro cao (phục vụ Credit Risk Team).
* **Độ trễ (Latency):** Kết quả dự đoán phải trả về trong vòng vài giây để hỗ trợ Loan Approval Team duyệt hồ sơ nhanh.
* **Dữ liệu tĩnh:** Giả định rằng hành vi trả nợ của khách hàng trong quá khứ vẫn giữ nguyên quy luật trong tương lai gần.

## 5. Chiến lược ra quyết định (Decision Strategy)
Mô hình sẽ cung cấp xác suất, nhưng Business cần một hành động cụ thể. Chúng ta định nghĩa **Classification Threshold ($\tau$)**:

* Nếu $P(default) > \tau$: **Reject** (Từ chối) hoặc chuyển sang **Manual Review** (Duyệt thủ công).
* Nếu $P(default) \le \tau$: **Approve** (Phê duyệt).

> **Lưu ý:** Việc chọn $\tau$ sẽ được quyết định ở giai đoạn **Model Evaluation** dựa trên sự đánh đổi giữa rủi ro mất vốn và mất cơ hội kinh doanh.

---