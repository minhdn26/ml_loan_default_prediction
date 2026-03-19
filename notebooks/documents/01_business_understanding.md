# 01. Business Understanding

## 1. Bối cảnh

Các tổ chức tài chính (ngân hàng, công ty tài chính) cung cấp các khoản vay cho khách hàng cá nhân. Tuy nhiên, một phần khách hàng có thể **không trả được nợ đúng hạn**, dẫn đến tình trạng **loan default**.

Loan default gây ra nhiều tổn thất cho tổ chức tài chính, bao gồm:

* mất tiền gốc
* mất doanh thu từ lãi
* chi phí thu hồi nợ
* gia tăng rủi ro danh mục tín dụng

Do đó, việc **đánh giá rủi ro tín dụng của khách hàng trước khi cấp khoản vay** là rất quan trọng.

---

## 2. Bài toán Business

Câu hỏi chính của business:

> Làm thế nào để dự đoán khả năng khách hàng sẽ **không trả được khoản vay** trước khi phê duyệt khoản vay?

Nếu có thể nhận diện sớm khách hàng rủi ro cao, tổ chức tài chính có thể:

* từ chối khoản vay
* điều chỉnh hạn mức vay
* yêu cầu thêm thông tin hoặc tài sản đảm bảo
* áp dụng chính sách lãi suất phù hợp

---

## 3. Mục tiêu Business

Xây dựng một hệ thống giúp:

* dự đoán **xác suất khách hàng vỡ nợ**
* hỗ trợ **ra quyết định phê duyệt khoản vay**
* giảm **tỷ lệ nợ xấu trong danh mục tín dụng**

---

## 4. Stakeholders

Các bên liên quan trong bài toán:

* **Credit Risk Team**: đánh giá rủi ro tín dụng
* **Loan Approval Team**: ra quyết định phê duyệt khoản vay
* **Risk Management**: quản lý rủi ro danh mục tín dụng
* **Data Science Team**: xây dựng mô hình dự đoán

---

## 5. Giá trị kỳ vọng

Nếu hệ thống hoạt động hiệu quả, tổ chức tài chính có thể:

* giảm tỷ lệ **loan default**
* giảm **tổn thất tài chính**
* cải thiện **chất lượng danh mục tín dụng**
* hỗ trợ **ra quyết định dựa trên dữ liệu**

---

## 6. Định hướng giải pháp

Giải pháp được đề xuất là xây dựng một **mô hình Machine Learning** có khả năng:

* học từ dữ liệu lịch sử các khoản vay
* nhận diện các đặc điểm của khách hàng có rủi ro cao
* dự đoán xác suất **loan default** cho khách hàng mới

Kết quả dự đoán sẽ được sử dụng như **một công cụ hỗ trợ ra quyết định** trong quy trình phê duyệt khoản vay.

---