+++
title = "Use the Solutionr"
date = 2025
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

### Sử dụng Innovation Sandbox trên AWS

Sau khi đã triển khai đầy đủ các stack và cấu hình người dùng, bạn có thể bắt đầu sử dụng giải pháp Innovation Sandbox trên AWS. Dưới đây là hướng dẫn chi tiết cách sử dụng với vai trò **administrator**.

#### Tổng quan

Giải pháp hỗ trợ ba vai trò sử dụng chính:

- **Administrator (Quản trị viên)**
- **Manager (Quản lý)**
- **End user (Người dùng cuối)**

Thời gian thực hành dự kiến: khoảng 30 phút.

### Sử dụng với vai trò Administrator

#### 1. Truy cập Innovation Sandbox trên AWS

- Lấy URL truy cập giải pháp (CloudFrontDistributionUrl đã lưu từ bước triển khai).
- Mở URL này trên trình duyệt.
- Đăng nhập bằng tài khoản administrator. Nếu là lần đầu đăng nhập, chọn **Forgot password** để thiết lập mật khẩu.

**Lưu ý:**  
Nếu giao diện có hai tab **Accounts** và **Applications**, hãy chọn **Applications** để vào ứng dụng chính. Trang chào mừng sẽ xuất hiện.

![architect](/resources/_gen/images/Anha1.png "Architect")

#### 2. Thêm tài khoản để cho thuê (lease)

- Trong thanh điều hướng bên trái, dưới mục **Administration**, chọn **Accounts**.
- Nhấn **Add accounts**.
- Ở mục **Select accounts**, bạn sẽ thấy danh sách các tài khoản có sẵn. Nếu chưa có tài khoản nào, hãy đảm bảo các sandbox account đã được đưa vào OU **Entry** trong organizational unit có tên dạng `your-namespace_InnovationSandboxAccountPool`.
- Chọn một hoặc nhiều tài khoản muốn thêm, nhấn **Next** và sau đó **Submit**.

![architect](/resources/_gen/images/Anha2.png "Architect")

- Quay lại mục **Accounts** để kiểm tra trạng thái các tài khoản đã thêm.

#### 3. Quản lý cài đặt hệ thống

- Trong thanh điều hướng, dưới **Administration**, chọn **Settings**.
- Có ba tab chính:
  - **General Settings**
  - **Lease Settings** (giới hạn ngân sách tối đa mặc định là $100 USD)
  - **Clean Up Settings**
- Để thay đổi các cài đặt này, bạn cần sử dụng **AWS AppConfig** (sẽ được hướng dẫn ở phần tiếp theo).

![architect](/resources/_gen/images/Anha3.png "Architect")

#### 4. Tạo Service Control Policy (SCP) giới hạn tài nguyên

Ví dụ: Tạo SCP để ngăn người dùng sandbox khởi tạo EC2 instance loại `m5.large`.

**Các bước thực hiện:**

- Đăng nhập vào **organization management account** (tài khoản quản lý tổ chức).
- Mở **AWS Organizations**.
- Trong thanh điều hướng, chọn **Policies**.
- Dưới **Supported policy types**, chọn **Service control policies**.

![architect](/resources/_gen/images/Anha4.png "Architect")

- Nhấn **Create policy** và nhập:
  - **Policy name:** RestrictEC2Instances
  - **Policy description:** This SCP restricts launching certain EC2 instance types.
- Dán đoạn JSON sau vào policy editor:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": [
            "m5.large"
          ]
        }
      }
    }
  ]
}
```

- Nhấn **Create policy** để lưu.
- Quay lại mục **Policies**, chọn policy vừa tạo.
- Chọn tab **Targets**, nhấn **Attach**.
- Chọn OU có tên `your-namespace_InnovationSandboxAccountPool` và nhấn **Attach policy**.
- 
![architect](/resources/_gen/images/Anha6.png "Architect")

**Kết quả:**  
Người dùng trong các sandbox account sẽ không thể khởi tạo EC2 instance loại `m5.large`.

#### Tiếp theo

- Bạn có thể tiếp tục tìm hiểu cách thay đổi cài đặt hệ thống qua **AWS AppConfig** và cách sử dụng giải pháp với vai trò **manager** và **end user** trong các bước tiếp theo.

**Lưu ý:**  
- Các thao tác quản trị cần thực hiện đúng tài khoản và Region đã cấu hình.
- Đảm bảo các tài khoản sandbox đã được onboard vào đúng OU để xuất hiện trong danh sách cho thuê.

------------------------
### Sử dụng Innovation Sandbox on AWS với vai trò Manager

Giải pháp Innovation Sandbox on AWS hỗ trợ ba vai trò: **administrator**, **manager**, và **end user**. Phần này hướng dẫn chi tiết cách sử dụng giải pháp với vai trò **manager**.

#### Tổng quan

Trong phần này, bạn sẽ thực hiện:

- Chỉnh sửa các thiết lập của Innovation Sandbox on AWS bằng AWS AppConfig.
- Tạo mẫu cấp phát tài khoản (lease template) cơ bản.
- Tạo mẫu cấp phát tài khoản nâng cao.

### Chỉnh sửa thiết lập bằng AWS AppConfig

**Lưu ý quan trọng:**  
Thực hiện các bước này trong **tài khoản hub** (hub account), không phải tài khoản quản lý tổ chức. Đảm bảo bạn đang ở đúng **home Region** của tài khoản này.

**Các bước chỉnh sửa thiết lập:**

- Đăng nhập vào tài khoản hub.
- Trên thanh tìm kiếm của console, nhập và chọn **AWS AppConfig**.
- Trong thanh điều hướng, dưới AWS AppConfig, chọn **Applications**.
- Chọn đường dẫn tới ứng dụng đã được tạo cho bạn.
- Dưới phần **Configuration Profiles and Feature Flags**, chọn đường dẫn có tên **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-ID**.



![architect](/resources/_gen/images/Manager1.png "Architect")

- Trong phần **Hosted configuration versions**, chọn **Create**.
- Thay đổi trường **maxBudget** thành **50** (USD).

![architect](/resources/_gen/images/Manager2.png "Architect")

- Chọn **Create hosted configuration version**.
- Chọn **Start deployment**.
- Chọn phiên bản cấu hình vừa tạo, giữ nguyên các thiết lập mặc định còn lại và chọn **Start deployment**.

### Tạo Lease Template (Mẫu cấp phát tài khoản)

#### Giới thiệu về Lease Template

Lease template là tập hợp các thông số và quy tắc định nghĩa cách người dùng truy cập và sử dụng tài nguyên trong môi trường Innovation Sandbox on AWS. Đây là bản mẫu để tạo các lease riêng lẻ (phiên truy cập có kiểm soát vào tài nguyên đám mây). Các thành phần chính của lease template gồm:

- **Phân bổ tài nguyên:** Định nghĩa ngân sách tối đa có thể sử dụng trong thời gian lease.
- **Thời lượng:** Xác định thời gian tối đa một lease có thể hoạt động.
- **Quy trình phê duyệt:** Xác định có cần quản lý phê duyệt trước khi cấp lease hay không.
- **Cảnh báo và ngưỡng:** Thiết lập các cảnh báo dựa trên mức tiêu thụ ngân sách hoặc thời gian còn lại.

Bạn sẽ tạo hai loại mẫu:

- **Mẫu cơ bản (Basic):** Ngân sách tối đa $25, phê duyệt tự động, phù hợp cho dự án nhỏ và kiểm thử thông thường.
- **Mẫu nâng cao (Advanced):** Ngân sách tối đa $50, yêu cầu quản lý phê duyệt, phù hợp cho dự án lớn cần kiểm soát chặt chẽ hơn.

### Tạo Lease Template cơ bản

Hướng dẫn này giúp bạn tạo mẫu lease cho các tài khoản không cần phê duyệt của quản lý. Mẫu này cho phép ngân sách tối đa $25, cảnh báo khi tiêu $15, thời lượng 168 giờ (1 tuần), cảnh báo trước khi hết hạn 24 giờ.

**Các bước thực hiện:**

- Trên console Innovation Sandbox on AWS, chọn profile người dùng ở góc trên bên phải và chọn **Sign out**.
- Đăng nhập bằng tài khoản manager. Nếu là lần đầu đăng nhập, sử dụng chức năng **Forgot password**.
- Nếu màn hình hiển thị hai tab **Accounts** và **Applications**, chọn **Applications** để vào ứng dụng. Bạn sẽ thấy trang chào mừng của Innovation Sandbox.
- Trong thanh điều hướng, chọn **Lease Templates**.
- Chọn **Add new lease template**.
- Ở trường **Name**, nhập: `Basic`.
- Ở trường **Description**, nhập: `Basic account lease template`.
- Bỏ chọn ô **Approval required** (người dùng không cần quản lý phê duyệt khi yêu cầu loại tài khoản này).

![architect](/resources/_gen/images/Manager3.png "Architect")

- Chọn **Next**.
- Ở trường **Maximum budget**, nhập: `100`, chọn **Next**.
- Nếu bạn đã chỉnh sửa AppConfig đúng, sẽ xuất hiện thông báo lỗi về ngân sách tối đa là $50. Đổi trường **Maximum Budget Amount** thành `25`.
- Dưới **Budget Thresholds**, chọn **Add a threshold**.
  - Ở **amount consumed**, nhập `15`.
  - Ở **action**, chọn **Send Alert**. Người dùng sẽ nhận cảnh báo khi tiêu $15, và tài khoản sẽ bị xóa khi tiêu hết $25.
- Chọn **Next**.
- Ở **Maximum Duration**, chọn **Set a maximum duration** và nhập `168`.
- Dưới **Duration Thresholds**, chọn **Add a threshold**.
  - Ở **remaining hours**, nhập `24`.
  - Ở **action**, chọn **Send Alert**. Người dùng sẽ nhận cảnh báo khi còn 24 giờ lease.
- Chọn **Submit**.
- Dưới **Lease Templates**, bạn sẽ thấy mẫu lease vừa tạo.

![architect](/resources/_gen/images/Manager4.png "Architect")


### Tạo Lease Template nâng cao

Phần này hướng dẫn bạn tạo mẫu lease yêu cầu quản lý phê duyệt. Mẫu này có ngân sách tối đa $50, cảnh báo ở mức $40, thời lượng 168 giờ, cảnh báo trước khi hết hạn 24 giờ. Thích hợp cho dự án lớn cần giám sát kỹ hơn.

**Các bước thực hiện:**

- Trong thanh điều hướng, chọn **Lease Templates**.
- Chọn **Add new lease template**.
- Ở trường **Name**, nhập: `Advanced`.
- Ở trường **Description**, nhập: `Advanced account lease template`.
- Giữ nguyên ô **Approval required** được chọn (người dùng cần quản lý phê duyệt khi yêu cầu loại tài khoản này).
- Chọn **Next**.
- Ở trường **Maximum budget**, nhập: `50`, chọn **Next**.
- Dưới **Budget Thresholds**, chọn **Add a threshold**.
  - Ở **amount consumed**, nhập `40`.
  - Ở **action**, chọn **Send Alert**. Người dùng sẽ nhận cảnh báo khi tiêu $40, và tài khoản sẽ bị xóa khi tiêu hết $50.
- Chọn **Next**.
- Ở **Maximum Duration**, chọn **Set a maximum duration** và nhập `168`.
- Dưới **Duration Thresholds**, chọn **Add a threshold**.
  - Ở **remaining hours**, nhập `24`.
  - Ở **action**, chọn **Send Alert**. Người dùng sẽ nhận cảnh báo khi còn 24 giờ lease.
- Chọn **Submit**.
- Dưới **Lease Templates**, bạn sẽ thấy hai mẫu lease đã tạo.

![architect](/resources/_gen/images/Manager5.png "Architect")

### Bước tiếp theo

Sau khi đã tạo hai lease template với vai trò manager, ở phần tiếp theo bạn sẽ:

- Đăng nhập vào Innovation Sandbox on AWS với tư cách người dùng cuối (end user).
- Yêu cầu tài khoản cơ bản:
  - Trải nghiệm quy trình phê duyệt tự động.
  - Truy cập tài khoản sandbox vừa tạo.
- Yêu cầu tài khoản nâng cao:
  - Gửi yêu cầu cần quản lý phê duyệt.
  - Chờ quản lý xem xét và phê duyệt.
  - Truy cập tài khoản sandbox đã được phê duyệt.
- Quản lý và giám sát các tài khoản sandbox của bạn.

### Sử dụng Innovation Sandbox on AWS với vai trò End-User

Giải pháp Innovation Sandbox on AWS hỗ trợ ba vai trò: **administrator**, **manager** và **end user**. Dưới đây là hướng dẫn chi tiết cho người dùng cuối (end user) khi sử dụng hệ thống.

#### 1. Yêu cầu và truy cập tài khoản AWS cơ bản

Các tài khoản cơ bản (basic account) sẽ được phê duyệt tự động, cho phép bạn truy cập ngay vào tài nguyên AWS trong phạm vi giới hạn đã định.

**Các bước thực hiện:**

- Trên giao diện Innovation Sandbox on AWS, chọn profile người dùng ở góc trên bên phải và chọn **Sign out**.
- Đăng nhập lại bằng thông tin của người dùng cuối (end user).
- Nếu màn hình có hai tab **Accounts** và **Applications**, chọn **Applications** để vào ứng dụng. Bạn sẽ thấy trang chào mừng của Innovation Sandbox.
- Chọn **Request a new account**.
- Chọn mẫu **Basic lease template**, nhấn **Next**.
- Đánh dấu **I accept the above terms of service**, nhấn **Next**.

![architect](/resources/_gen/images/User1.png "Architect")

- Ở phần **Comments**, nhập lý do (ví dụ: `Request for a basic account`).
- Chọn **Submit**.
- Ở thanh điều hướng bên trái, chọn **Home**. Trong mục **Account**, nhấn **Refresh** để cập nhật trạng thái tài khoản.

![architect](/resources/_gen/images/User2.png "Architect")

- Khi tài khoản đã được cấp, dưới mục **Access**, chọn **Login to account** cho tài khoản vừa thêm.
- Chọn vai trò (role) xuất hiện ở mục **Select a role** để vào AWS Management Console.

![architect](/resources/_gen/images/User3.png "Architect")

#### 2. Kiểm tra giới hạn Service Control Policy (SCP)

Bạn có thể kiểm tra hiệu lực của SCP bằng cách thử tạo một EC2 instance bị giới hạn.

**Các bước kiểm tra:**

- Trong AWS Management Console, nhập **EC2** vào thanh tìm kiếm và chọn dịch vụ.
- Ở thanh điều hướng bên trái, chọn **Instances**.
- Chọn **Launch instances**.
- Ở trường **Name**, nhập `test-scp`. Giữ nguyên Amazon Machine Image (AMI) mặc định.

![architect](/resources/_gen/images/User4.png "Architect")

- Ở trường **Instance type**, chọn **m5.large** (loại này đã bị SCP chặn).
- Ở **Key pair**, chọn **Proceed without a key pair**.
- Giữ nguyên các thiết lập còn lại, sau đó chọn **Launch instance**.
- Bạn sẽ thấy thông báo lỗi do SCP không cho phép khởi tạo EC2 instance loại **m5.large**.


![architect](/resources/_gen/images/User5.png "Architect")

#### 3. Yêu cầu tài khoản nâng cao (Advanced Account)

Các tài khoản nâng cao yêu cầu sự phê duyệt của manager trước khi được cấp quyền truy cập.

**Các bước thực hiện:**

- Quay lại giao diện Innovation Sandbox on AWS, đăng nhập với tư cách end user.
- Chọn **Request a new account**.
- Chọn mẫu **Advanced lease template**, nhấn **Next**.
- Đánh dấu **I accept the above terms of service**, nhấn **Next**.
- Ở phần **Comments**, nhập lý do (ví dụ: `Request for an advanced account`).
- Chọn **Submit**.

**Quy trình phê duyệt:**

- Đăng xuất khỏi Innovation Sandbox on AWS và đăng nhập bằng tài khoản manager.
- Ở trang chủ, bạn sẽ thấy yêu cầu đang chờ phê duyệt. Trong thanh điều hướng bên trái, chọn **Approvals**.
- Chọn yêu cầu, vào **Actions** và chọn **Approve request**.

**Quản lý lease:**

- Trong thanh điều hướng, chọn **Leases**.
- Bạn sẽ thấy hai lease: một cho tài khoản basic, một cho tài khoản advanced.
- Có thể chọn lease và dùng **Actions** để chấm dứt, tạm ngưng hoặc cập nhật lease.

![architect](/resources/_gen/images/User6.png "Architect")

![architect](/resources/_gen/images/User7.png "Architect")

#### 4. Tổng kết chức năng và trải nghiệm người dùng

- **Quản trị viên**: Thiết lập hệ thống, tạo SCP để kiểm soát tài nguyên, quản lý tài khoản và cấu hình môi trường Innovation Sandbox, tích hợp với AWS AppConfig.
- **Manager**: Cấu hình ngưỡng ngân sách, tạo lease template (Basic, Advanced), thiết lập quy trình phê duyệt, giám sát sử dụng tài nguyên.
- **End user**: Yêu cầu và truy cập tài khoản AWS, kiểm thử hiệu lực SCP, sử dụng các loại tài khoản với quy trình phê duyệt phù hợp.



