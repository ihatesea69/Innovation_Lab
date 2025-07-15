+++
title = "Deploy the Solution"
date = 2025
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

### Triển khai Giải pháp (Deploy the Solution)

Giải pháp Innovation Sandbox trên AWS sử dụng bốn stack AWS CloudFormation, cần được triển khai theo thứ tự nhất định. Mỗi stack phải được triển khai đúng tài khoản và vùng AWS (Region) đã chọn để đảm bảo hoạt động chính xác. Trong suốt quá trình triển khai, hãy sử dụng cùng một home Region đã xác định từ trước.

#### Tổng quan về các Stack CloudFormation

Việc triển khai bao gồm bốn stack CloudFormation, mỗi stack xây dựng dựa trên stack trước đó:

- **AccountPool Stack**
  - Triển khai trên tài khoản quản lý tổ chức (organization management account)
  - Tạo cấu trúc tổ chức AWS
  - Thiết lập các chính sách và kiểm soát nền tảng

- **IDC Stack**
  - Triển khai trên tài khoản quản lý tổ chức
  - Cấu hình tích hợp AWS IAM Identity Center
  - Thiết lập quản lý danh tính và truy cập

- **Data Stack**
  - Triển khai trên tài khoản hub
  - Triển khai các thành phần lưu trữ và quản lý dữ liệu
  - Cấu hình các dịch vụ backend

- **Compute Stack**
  - Triển khai trên tài khoản hub
  - Thiết lập hạ tầng giải pháp
  - Tạo giao diện web và các thành phần xử lý

#### Sơ đồ quan hệ giữa các Stack

Mỗi stack CloudFormation có mối quan hệ phụ thuộc với các stack khác và được triển khai trên các tài khoản khác nhau, đảm bảo các thành phần của giải pháp hoạt động phối hợp.

![architect](/images/stack-relationships.png "Architect")

#### Mẫu Template CloudFormation

Để triển khai giải pháp, sử dụng các đường dẫn template CloudFormation sau (Workshop gốc của AWS thì có một số lỗi liên quan tới Compute Stack nên tác giả đã thay thế bằng link drive có stack mới, nếu link này lỗi xin vui long liên hệ qua email hieunghiwork123@gmail.com):

| Stack         | Template URL                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------|
| AccountPool   | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-AccountPool.template |
| IDC           | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-IDC.template         |
| Data          | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-Data.template        |
| Compute       | https://drive.google.com/file/d/1oJ9DeANLgpscQSEGuO_gk7rxiDSTWf-_/view?usp=sharing     |

#### Trước khi bắt đầu

Trước khi triển khai, hãy đảm bảo bạn đã hoàn thành các bước sau:

- Hoàn thành tất cả các bước trong phần Yêu cầu trước (Prerequisites module)
- Đảm bảo có quyền truy cập vào cả hai tài khoản: tài khoản quản lý tổ chức (organization management account) và tài khoản hub
  - Tài khoản dùng để thiết lập AWS Organizations hoặc AWS Control Tower chính là tài khoản quản lý tổ chức

**Thời gian dự kiến:** Khoảng 45 phút

Việc triển khai đúng thứ tự và đúng tài khoản/vùng sẽ đảm bảo giải pháp Innovation Sandbox trên AWS hoạt động hiệu quả và tuân thủ các chính sách quản trị của tổ chức.

### Triển khai AccountPool Stack

Triển khai AccountPool stack là bước đầu tiên trong tài khoản quản lý tổ chức (organization management account). Stack này sẽ tạo cấu trúc tổ chức và các chính sách nền tảng cần thiết cho giải pháp.

#### Các bước triển khai

**Đăng nhập vào AWS Management Console**

- Mở AWS Management Console.
- Đăng nhập bằng tài khoản quản lý tổ chức của bạn.
- Chuyển sang vùng AWS chính (home Region) đã chọn.

**Thu thập thông tin cần thiết**

- **HubAccountId:** Đây là ID tài khoản AWS nơi bạn sẽ triển khai hai trong bốn stack. Để xem tất cả các tài khoản trong tổ chức, vào AWS Organizations trong console, chọn một tài khoản làm hub và ghi lại ID để sử dụng ở bước 5 và các bước sau.
- **ParentOuID:** Đây là ID của root hoặc organizational unit (OU) nơi các OU của Innovation Sandbox sẽ được tạo. Để tìm ID này, mở AWS Organizations, cấu trúc tổ chức sẽ hiển thị ID dưới tên từng OU. Bạn cũng có thể dùng root organization ID.

![architect](/images/organiza.jpg "Architect")

**Khởi tạo stack**

- Mở AWS CloudFormation console.
- Chọn mục Stack, sau đó chọn "Create stack".
- Chọn "With new resources (standard)".

**Chỉ định template**

- Chọn "Amazon S3 URL".
- Nhập đường dẫn template của AccountPool stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-AccountPool.template`
- Chọn "Next".

**Nhập thông tin chi tiết cho stack**

| Tham số           | Mô tả                                                                 | Ví dụ                |
|-------------------|-----------------------------------------------------------------------|----------------------|
| Stack name        | Tên stack (tùy chọn)                                                  | fcj-accountpool      |
| Namespace         | Định danh duy nhất cho triển khai này (3-8 ký tự, chữ/số)             | fcj                |
| HubAccountId      | AWS Account ID của tài khoản hub                                      | 123456789012         |
| ParentOuId        | Root ID hoặc OU ID nơi tạo các OU của Innovation Sandbox              | r-emd9               |
| IsbManagedRegions | Danh sách các Region cho phép, cách nhau bằng dấu phẩy                | us-east-1,us-west-2  |

**Lưu ý thêm:**

- **Namespace:** Sử dụng cùng một giá trị này cho cả bốn stack. Ghi nhớ giá trị này để dùng cho các bước tiếp theo.
- **HubAccountId:** Nhập ID tài khoản hub đã lấy ở bước 2.
- **ParentOuID:** Nhập root ID hoặc OU ID đã lấy ở bước 2.
- **IsbManagedRegions:** Nhập danh sách các Region mà người dùng Innovation Sandbox được phép truy cập.

**Cấu hình các tuỳ chọn stack**

- Xem lại các tuỳ chọn nâng cao nếu cần.
- Thêm tags nếu muốn.
- Chọn "Next".

**Xem lại và tạo stack**

- Kiểm tra lại toàn bộ thông tin đã nhập.
- Chọn "Submit" để bắt đầu tạo stack.

**Kiểm tra quá trình triển khai**

- Mở lại CloudFormation console, chọn mục Stacks.
- Tìm stack vừa tạo và kiểm tra trạng thái. Đợi đến khi trạng thái chuyển sang "CREATE_COMPLETE".

Bạn đã triển khai thành công stack đầu tiên. Tiếp tục với việc triển khai stack thứ hai theo hướng dẫn tiếp theo.

### Triển khai IDC Stack

IDC (Identity Center) stack là thành phần thứ hai của giải pháp Innovation Sandbox trên AWS. Stack này được triển khai trên tài khoản quản lý tổ chức (organization management account) và có nhiệm vụ cấu hình tích hợp với AWS IAM Identity Center, đồng thời thiết lập quản lý truy cập cho giải pháp.

#### Các bước triển khai

**1. Đăng nhập vào AWS Management Console**

- Mở AWS Management Console.
- Đăng nhập bằng tài khoản quản lý tổ chức của bạn.
- Chuyển sang vùng AWS chính (home Region) đã chọn.

**2. Thu thập thông tin cho bước 5**

- Mở IAM Identity Center console.
- Chọn mục Settings (Cài đặt).
- Sao chép và lưu lại hai giá trị: **Identity store ID** và **Instance ARN** để sử dụng ở bước 5.

**3. Khởi tạo stack**

- Mở AWS CloudFormation console.
- Chọn mục Stacks, sau đó chọn "Create stack".
- Chọn "With new resources (standard)".

**4. Chỉ định template**

- Chọn "Amazon S3 URL".
- Nhập đường dẫn template của IDC stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-IDC.template`
- Chọn "Next".

**5. Nhập thông tin chi tiết cho stack**

| Tham số           | Mô tả                                                        | Ví dụ                                   |
|-------------------|--------------------------------------------------------------|-----------------------------------------|
| Stack name        | Tên stack (tùy chọn)                                         | fcj-idc                                 |
| Namespace         | Giá trị namespace giống như ở stack AccountPool              | fcj01                                   |
| HubAccountId      | AWS Account ID của tài khoản hub                             | 123456789012                            |
| IdentityStoreId   | Identity store ID từ IAM Identity Center (bước 2)            | d-1234567890                            |
| SsoInstanceArn    | ARN của instance SSO trong IAM Identity Center (bước 2)      | arn:aws:sso:::instance/ssoins-12345678901234567 |

**Lưu ý thêm:**

- **Namespace:** Dùng cùng một giá trị cho cả bốn stack, giống với stack trước.
- **HubAccountId:** Là AWS Account ID nơi sẽ triển khai hai trong bốn stack.
- **IdentityStoreId** và **SsoInstanceArn:** Lấy từ mục Settings của IAM Identity Center ở bước 2.

**6. Cấu hình các tuỳ chọn stack**

- Xem lại các tuỳ chọn nâng cao nếu cần thiết.
- Thêm tags nếu muốn.
- Chọn "Next".

**7. Xem lại và tạo stack**

- Kiểm tra lại toàn bộ thông tin đã nhập.
- Chọn "Submit" để bắt đầu tạo stack.

**Kiểm tra quá trình triển khai**

- Mở lại CloudFormation console, chọn mục Stacks.
- Tìm stack vừa tạo và kiểm tra trạng thái. Đợi đến khi trạng thái chuyển sang "CREATE_COMPLETE".

Bạn đã triển khai thành công IDC stack. Tiếp tục triển khai stack thứ ba trên tài khoản hub theo hướng dẫn tiếp theo.

### Triển khai Data Stack

Data stack là thành phần thứ ba của giải pháp Innovation Sandbox trên AWS. Stack này được triển khai trên tài khoản hub và có nhiệm vụ thiết lập hạ tầng dữ liệu cũng như các dịch vụ backend cần thiết cho giải pháp.

#### Lưu ý quan trọng

- Đảm bảo bạn đăng nhập đúng vào tài khoản hub.
- Sử dụng cùng một AWS Region như các lần triển khai trước đó.

#### Các bước triển khai

**1. Đăng nhập vào AWS Management Console**

- Mở AWS Management Console.
- Đăng nhập bằng tài khoản hub của bạn.
- Chuyển sang vùng AWS chính (home Region) đã chọn.

**2. Khởi tạo stack**

- Mở AWS CloudFormation console.
- Chọn mục Stacks, sau đó chọn "Create stack".
- Chọn "With new resources (standard)".

**3. Chỉ định template**

- Chọn "Amazon S3 URL".
- Nhập đường dẫn template của Data stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-Data.template`
- Chọn "Next".

**4. Nhập thông tin chi tiết cho stack**

| Tham số    | Mô tả                                                    | Ví dụ   |
|------------|----------------------------------------------------------|---------|
| Stack name | Tên stack (tùy chọn)                                     | fcj-data|
| Namespace  | Giá trị namespace giống như ở các lần triển khai trước   | fcj01   |

- **Namespace:** Dùng cùng một giá trị đã sử dụng ở các stack trước (ví dụ: fcj01).

**5. Cấu hình các tuỳ chọn stack**

- Xem lại các tuỳ chọn nâng cao nếu cần.
- Thêm tags nếu muốn.
- Chọn "Next".

**6. Xem lại và tạo stack**

- Kiểm tra lại toàn bộ thông tin đã nhập.
- Chọn "Submit" để bắt đầu tạo stack.

**7. Kiểm tra quá trình triển khai**

- Mở lại CloudFormation console, chọn mục Stacks.
- Tìm stack vừa tạo và kiểm tra trạng thái.
- Đợi đến khi trạng thái chuyển sang "CREATE_COMPLETE".

Bạn đã triển khai thành công Data stack. Tiếp tục triển khai stack cuối cùng trên tài khoản hub theo hướng dẫn tiếp theo.

### Triển khai Compute Stack

Compute stack là thành phần cuối cùng của giải pháp Innovation Sandbox trên AWS. Stack này được triển khai trên tài khoản hub và chịu trách nhiệm thiết lập hạ tầng giải pháp, bao gồm giao diện web và các thành phần xử lý.

#### Các bước triển khai

**1. Đăng nhập vào AWS Management Console**

- Mở AWS Management Console.
- Đăng nhập bằng tài khoản hub của bạn.
- Chuyển sang vùng AWS chính (home Region) đã chọn.

**2. Lấy thông tin tài khoản quản lý**

- Bạn cần ID tài khoản quản lý tổ chức (management account ID) cho bước 5.
- Nếu chưa có, đăng nhập vào AWS Management Console bằng tài khoản quản lý.
- Nhấn vào menu tên tài khoản ở góc trên bên phải.
- Sao chép 12 chữ số ID tài khoản để dùng ở bước tiếp theo.

**3. Khởi tạo stack**

- Mở AWS CloudFormation console.
- Chọn mục Stacks, sau đó chọn "Create stack".
- Chọn "With new resources (standard)".

**4. Chỉ định template**

- Chọn "Amazon S3 URL".
- Nhập đường dẫn template của Compute stack:  
  `https://drive.google.com/file/d/1oJ9DeANLgpscQSEGuO_gk7rxiDSTWf-_/view?usp=sharing `
- Chọn "Next".

**5. Nhập thông tin chi tiết cho stack**

| Tham số           | Mô tả                                                          | Ví dụ         |
|-------------------|----------------------------------------------------------------|---------------|
| Stack name        | Tên stack (tùy chọn)                                           | fcj-compute   |
| Namespace         | Giá trị namespace giống như ở các stack trước                  | fcj01         |
| OrgMgtAccountId   | AWS Account ID của tài khoản quản lý tổ chức                   | 123456789012  |
| IdcAccountId      | AWS Account ID nơi đã cấu hình IAM Identity Center             | 123456789012  |

- **Namespace:** Dùng cùng một giá trị cho tất cả các stack (ví dụ: isb01).
- **OrgMgtAccountId:** ID tài khoản quản lý tổ chức.
- **IdcAccountId:** ID tài khoản đã cấu hình IAM Identity Center (thường cũng là tài khoản quản lý).

**6. Cấu hình các tuỳ chọn stack**

- Xem lại các tuỳ chọn nâng cao nếu cần.
- Thêm tags nếu muốn.
- Chọn "Next".

**7. Xem lại và tạo stack**

- Kiểm tra lại toàn bộ thông tin đã nhập.
- Chọn "Submit" để bắt đầu tạo stack.

**8. Kiểm tra quá trình triển khai**

- Mở lại CloudFormation console, chọn mục Stacks.
- Tìm stack vừa tạo và kiểm tra trạng thái.
- Đợi đến khi trạng thái chuyển sang "CREATE_COMPLETE".

#### Sau khi triển khai

- Vào CloudFormation console, chọn mục Stacks.
- Chọn Compute stack vừa triển khai.
- Truy cập tab **Outputs**.
- Sao chép giá trị **CloudFrontDistributionUrl**.

Ví dụ:  
`https://duyXXXXXXXeh.cloudfront.net`

Lưu lại giá trị này, bạn sẽ cần dùng trong phần cấu hình tiếp theo. Đây cũng là URL chính để truy cập giải pháp Innovation Sandbox trên AWS của bạn.
