# 03. Data Understanding

## 1. Tổng quan bộ dữ liệu (Dataset Overview)
Mục tiêu của giai đoạn này là khám phá cấu trúc, đặc điểm và chất lượng của dữ liệu thô trước khi thực hiện tiền xử lý. Bộ dữ liệu dự án bao gồm thông tin chi tiết về các khoản vay cá nhân, hành vi tài chính và lịch sử tín dụng.

* **Đơn vị phân tích:** Một dòng dữ liệu tương ứng với một khoản vay (Loan-level).
* **Số lượng biến:** 18 biến (17 biến độc lập và 1 biến mục tiêu).
* **Biến mục tiêu:** `Default` (Phân loại nhị phân).

---

## 2. Từ điển dữ liệu (Data Dictionary)

Dựa trên yêu cầu nghiệp vụ, các trường thông tin được định nghĩa như sau:

| Tên trường | Kiểu dữ liệu | Mô tả | Vai trò |
| :--- | :--- | :--- | :--- |
| **LoanID** | String | Mã định danh duy nhất của khoản vay. | ID (Loại bỏ khi training) |
| **Age** | Integer | Độ tuổi của người vay tại thời điểm nộp hồ sơ. | Đặc trưng (Feature) |
| **Income** | Float | Thu nhập hàng năm của người vay. | Đặc trưng (Feature) |
| **LoanAmount** | Float | Tổng số tiền giải ngân của khoản vay. | Đặc trưng (Feature) |
| **CreditScore** | Integer | Điểm tín dụng hệ thống (300 - 850). | Đặc trưng (Feature) |
| **MonthsEmployed** | Integer | Số tháng làm việc tại tổ chức/công ty hiện tại. | Đặc trưng (Feature) |
| **NumCreditLines** | Integer | Số lượng các khoản tín dụng khác đang hoạt động. | Đặc trưng (Feature) |
| **InterestRate** | Float | Lãi suất của khoản vay (%). | Đặc trưng (Feature) |
| **LoanTerm** | Integer | Thời hạn vay tính bằng tháng (12, 24, 36...). | Đặc trưng (Feature) |
| **DTIRatio** | Float | Tỷ lệ nợ trên thu nhập (Debt-to-Income). | Đặc trưng (Feature) |
| **Education** | Category | Trình độ học vấn (High School, Bachelor...). | Đặc trưng (Feature) |
| **EmploymentType** | Category | Hình thức việc làm (Full-time, Self-employed...). | Đặc trưng (Feature) |
| **MaritalStatus** | Category | Tình trạng hôn nhân. | Đặc trưng (Feature) |
| **HasMortgage** | Boolean | Có đang thế chấp bất động sản hay không. | Đặc trưng (Feature) |
| **HasDependents** | Boolean | Có người phụ thuộc (con cái, cha mẹ) không. | Đặc trưng (Feature) |
| **LoanPurpose** | Category | Mục đích sử dụng tiền vay. | Đặc trưng (Feature) |
| **HasCoSigner** | Boolean | Có người ký chung/bảo lãnh khoản vay không. | Đặc trưng (Feature) |
| **Default** | Binary | Trạng thái vỡ nợ (1: Vỡ nợ, 0: Trả đúng hạn). | **Biến mục tiêu (Target)** |

---

## 3. Phân tích Phân phối & Đặc điểm Dữ liệu (Initial Profiling)

### 3.1. Phân phối biến mục tiêu (Target Distribution)
Trong bài toán Risk Scoring, việc kiểm tra sự cân bằng giữa lớp `Default = 1` và `Default = 0` là ưu tiên hàng đầu. 
> **Giả thuyết:** Nếu tỷ lệ vỡ nợ < 5%, chúng ta đang đối mặt với bài toán **Imbalanced Classification**, cần áp dụng các kỹ thuật lấy mẫu lại (Resampling).

### 3.2. Mối liên hệ nghiệp vụ sơ bộ
Dựa trên kiến thức chuyên gia (Domain Knowledge), chúng ta cần tập trung vào các "điểm chạm" sau:
* **Khả năng chi trả:** Mối tương quan giữa `Income`, `LoanAmount` và `DTIRatio`.
* **Uy tín cá nhân:** Tác động của `CreditScore` và `NumCreditLines` lên biến `Default`.
* **Cam kết tài chính:** Sự hiện diện của `HasCoSigner` thường làm giảm rủi ro vỡ nợ.

---

## 4. Checklist kiểm tra chất lượng dữ liệu (Data Quality Check)

Để đảm bảo mô hình Baseline không bị "GIGO" (Garbage In, Garbage Out), các đầu mục kiểm tra sau sẽ được thực hiện trong Notebook:

1.  **Missing Values:** Xác định các trường có tỷ lệ khống cao (đặc biệt là `InterestRate` và `Income`).
2.  **Logic Errors:** * `Age` phải lớn hơn 18.
    * `MonthsEmployed` không được lớn hơn `(Age - 15) * 12`.
    * `DTIRatio` thông thường nằm trong khoảng [0, 1].
3.  **Cardinality:** Kiểm tra số lượng các giá trị độc nhất trong các biến Category (`LoanPurpose`, `Education`). Nếu có quá nhiều nhóm nhỏ, cần nhóm lại để tránh Overfitting.
4.  **Outliers:** Sử dụng phương pháp IQR hoặc Z-Score để tìm các giá trị thu nhập hoặc khoản vay bất thường.

---

## 5. Kết luận giai đoạn
Kết thúc giai đoạn này, chúng ta sẽ có một bản **Data Quality Report** chi tiết, xác định được những biến nào cần xử lý kỹ ở bước **Data Preparation** và những biến nào mang lại giá trị dự báo cao nhất.

---