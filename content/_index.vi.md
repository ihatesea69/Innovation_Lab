+++
title = "Innovation Lab"
date = 2025
weight = 1
chapter = false
+++

# Xây dựng một không gian thực hành thoải mái sáng tạo và học tập trên AWS

## Tổng quan về Workshop

**Innovation Sandbox on AWS Workshop** là một hướng dẫn giúp các nhóm học tập hay công ty có một nơi để vọc vạch hay là thử nghiệm này kia mà không phải lo về bảo mật hay chi phí




![architect](../images/high-level.png "Architect")
### Mục đích chính


- **Quản lý aws account riêng cho thử nghiệm**
- **Quản lý chi phí tiện lợi qua giao diện web**.


## Yêu cầu kiến thức (Nên biết trước)

- Biết  **AWS Organizations** và **AWS IAM Identity Center**
- Biết sơ sơ **AWS CloudFormation** (chỉ cần hiểu khái niệm vì đã có template file có sẵn sẵn)

## Thời gian hiện thực

- **Tổng thời gian thực hiện:** Khoảng 2 giờ (có thể lâu hơn nếu bạn muốn tìm hiểu sâu)

## Các thành phần chính của giải pháp

| Thành phần                | Vai trò chính                                                                                  |
|--------------------------|-----------------------------------------------------------------------------------------------|
| AWS IAM Identity Center  | Xác thực người dùng, quản lý truy cập                                                         |
| Amazon CloudFront        | Phân phối giao diện người dùng, phục vụ web từ S3, chuyển tiếp API đến Lambda                 |
| AWS WAF                  | Lọc bảo mật cho các tuyến động                                                                |
| AWS AppConfig            | Lưu trữ cấu hình giải pháp                                                                    |
| AWS Organizations        | Quản lý nhiều tài khoản AWS dưới một tổ chức, nhóm theo OU, quản lý toàn bộ môi trường        |
| AWS Lambda               | Thực thi logic cấp phát, dọn dẹp, quản lý tài khoản sandbox, giao diện động                  |
| Amazon EventBridge       | Kích hoạt các tác vụ dựa vào thời gian hoặc sự kiện                                           |
| AWS Step Functions       | Quản lý trạng thái tài khoản sandbox theo quy trình logic                                     |
| AWS accounts             | Tài khoản sandbox được chuẩn bị và phân phối đến người dùng cuối                              |

![Account LifeCycle](../images/sandbox-account-ou-lifecycle.png "a title")

## Chu trình tài khoản (Account Lifecycle)

1. **Onboard tài khoản**: Tài khoản được đưa vào hệ thống và dọn dẹp sạch sẽ trước khi sử dụng.
2. **Available**: Tài khoản sẵn sàng để cấp phát cho người dùng.
3. **Active**: Tài khoản đang được sử dụng, hệ thống sẽ giám sát chi phí và thời gian sử dụng.
4. **Frozen**: Tài khoản bị đóng băng khi vượt quá ngân sách hoặc thời gian, hoặc do quản trị viên.
5. **Cleanup**: Khi tài khoản hết hạn sử dụng, hệ thống sẽ tự động dọn dẹp tài nguyên.
6. **Quarantine**: Nếu quá trình dọn dẹp thất bại, tài khoản sẽ bị cách ly để xử lý lại.
7. **Exit**: Tài khoản có thể bị xóa hệ thống khi cần thiết.

## Giải thích chi tiết sơ đồ vòng đời tài khoản

Sơ đồ vòng đời tài khoản mô tả các trạng thái chính của một tài khoản sandbox trong hệ thống:
- **Available**: Tài khoản sẵn sàng để cấp phát.
- **Active**: Đang được sử dụng, giám sát chi phí và thời gian.
- **Frozen**: Đóng băng khi vượt quá giới hạn chi tiêu/thời gian hoặc do quản trị viên yêu cầu.
- **Cleanup**: Dọn dẹp tài khoản khi hết hạn sử dụng.
- **Quarantine**: Cách ly nếu dọn dẹp thất bại, có thể thử lại.
- **Entry/Exit**: Onboard hoặc loại bỏ tài khoản khỏi hệ thống.





## Quy trình triển khai Workshop

### 1. Prerequisites (Chuẩn bị)
- Chuẩn bị môi trường AWS, tạo tài khoản.
- Thời gian: 15-20 phút (có thể lâu hơn nếu phải tạo mới tài khoản).

### 2. Deployment (Triển khai)
- Triển khai các CloudFormation stack, cấu hình hạ tầng , thiết lập quản lý.
- Thời gian: ~45 phút.

### 3. Configuration (Cấu hình)
- Cấu hình tích hợp SAML cho xác thực, thiết lập truy cập người dùng, onboard tài khoản sandbox.
- Thời gian: ~15 phút.

### 4. Using the sandbox
- Học cách sử dụng giải pháp qua ba vai trò khác nhau.
- Thời gian: ~30 phút.

#### References Links:

https://catalog.us-east-1.prod.workshops.aws/workshops/23f635fc-dc98-4f75-8b91-6f334d0e22c3

https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/solution-overview.html

https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/






