+++
title = "Các bước chuẩn bị"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

### Yêu cầu trước khi triển khai Innovation Sandbox trên AWS

#### Tổng quan

Để triển khai giải pháp Innovation Sandbox trên AWS, bạn cần chuẩn bị một số bước cơ bản cho môi trường AWS của mình. Các bước này bao gồm:

- Xác định các tài khoản AWS cần thiết
- Chọn region AWS 
- Thiết lập tổ chức AWS (AWS Organization)
- Kích hoạt các sau:
  - AWS IAM Identity Center
  - AWS Resource Access Manager (RAM)
  - AWS CloudFormation StackSets
  - Service Control Policies (SCPs)

**Thời gian dự kiến:** Khoảng 15-20 phút (có thể lâu hơn nếu bạn cần tạo mới các tài khoản AWS)

#### Xác định các tài khoản AWS cần thiết

- Innovation Sandbox sử dụng tính năng của AWS Organizations, giúp bạn quản lý tập trung và kiểm soát môi trường AWS khi mở rộng quy mô.
- Bạn cần chọn một tài khoản ban đầu (gọi là tài khoản quản lý – management account).
- Có hai lựa chọn cho tài khoản ban đầu:
  - Sử dụng tài khoản AWS hiện có
  - Tạo tài khoản AWS mới

**Lưu ý:**
- Nếu đã có tổ chức AWS: tài khoản ban đầu chính là tài khoản quản lý hiện có.
- Nếu tạo tổ chức AWS mới: có thể dùng một tài khoản AWS độc lập (sẽ trở thành management account) hoặc tạo tài khoản mới cho vai trò này.
- Tài khoản này sẽ là tài khoản quản lý cho toàn bộ tổ chức AWS của bạn.

#### Chọn vùng AWS chính (home Region)

- Tất cả các thành phần của Innovation Sandbox phải được triển khai trong cùng một vùng AWS.
- Vùng được chọn cần đáp ứng:
  - Hỗ trợ đầy đủ các dịch vụ bắt buộc: AWS Organizations, IAM Identity Center, CloudFormation, DynamoDB, Lambda, CloudWatch, Secrets Manager, AppConfig.
  - Là nơi đã bật IAM Identity Center.
  - Có thể truy cập cho tất cả người dùng dự kiến sử dụng giải pháp.

**Khi chọn vùng AWS, cân nhắc:**
- Vị trí địa lý, khoảng cách với người dùng, yêu cầu lưu trữ dữ liệu, độ trễ mạng.
- Chi phí dịch vụ và sự khác biệt giá giữa các vùng.
- Chi phí truyền dữ liệu giữa các vùng.
- Sự khác biệt về giá của từng dịch vụ.

#### Thiết lập AWS Organizations

- Bạn cần tổ chức AWS có ít nhất 4 tài khoản:
  - Tài khoản quản lý (management account)
  - Tài khoản hub (hub account)
  - Sandbox account 1
  - Sandbox account 2

- Có thể thêm nhiều tài khoản sandbox tùy nhu cầu.



**Các bước tạo tổ chức AWS mới:**
- Đăng nhập AWS Management Console với quyền quản trị.
- Mở AWS Organizations console và chọn "Create an organization".
- Xác thực email quản lý theo hướng dẫn gửi về hộp thư.
- Thêm các tài khoản thành viên:
  - Có thể tạo tài khoản mới (cung cấp tên, email root, giữ nguyên IAM role mặc định).
  - Hoặc mời tài khoản AWS hiện có vào tổ chức (bằng email hoặc ID tài khoản).
- Lặp lại cho đến khi có ít nhất 4 tài khoản như cấu trúc sau:


```
Root
├── Management account
├── Hub account
├── Sandbox account 1
└── Sandbox account 2
```
![architect](/resources/_gen/images/anh2.jpg "Architect")

![architect](/resources/_gen/images/Anh1.jpg "Architect")

- Kiểm tra trong phần Settings của AWS Organizations console:
  - Đã bật consolidated billing
  - Đã bật all features

#### Kích hoạt các dịch vụ bắt buộc

Trước khi triển khai giải pháp, hãy đảm bảo các dịch vụ sau đã được bật trên tài khoản quản lý:

- **AWS IAM Identity Center:** Quản lý truy cập tập trung cho các tài khoản AWS.
  - Mở IAM Identity Center ở tài khoản quản lý, kiểm tra đúng vùng, chọn Enable.

- **AWS Resource Access Manager (RAM):** Chia sẻ tài nguyên AWS an toàn giữa các tài khoản trong tổ chức.
  - Mở RAM ở tài khoản quản lý, vào Settings, bật "Enable sharing with AWS Organizations", lưu lại.

- **CloudFormation StackSets trusted access:** Quản lý CloudFormation stack trên nhiều tài khoản/vùng.
  - Mở CloudFormation ở tài khoản quản lý, chọn StackSets, bật "Enable trusted access".

- **Service Control Policies (SCPs):** Quản lý quyền truy cập trên toàn tổ chức.
  - Mở AWS Organizations ở tài khoản quản lý, vào Policies, chọn Service control policies, bật tính năng này.

