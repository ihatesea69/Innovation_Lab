+++
title = "Use the Solutio"
date = 2025
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

### Sử dụng hệ thống

Okay giờ đã setup xong hết rồi, chúng ta sẽ thử xài hệ thống với 3 vai trò khác nhau để xem nó hoạt động thế nào.

#### Ba vai trò chính

- **Administrator** - quản trị viên, quyền cao nhất
- **Manager** - quản lý, tạo template và approve request  
- **End user** - người dùng cuối, request tài khoản để xài

Dự kiến mất khoảng 30 phút để thử hết.

### Vai trò Administrator

#### 1. Đăng nhập vào hệ thống

- Lấy URL từ CloudFrontDistributionUrl đã lưu lúc deploy
- Mở URL trên browser
- Đăng nhập bằng tài khoản admin. Lần đầu thì chọn **Forgot password** để set mật khẩu

**Lưu ý:** Nếu thấy hai tab **Accounts** và **Applications**, chọn **Applications** để vào app chính.

![architect](/resources/_gen/images/Anha1.png "Architect")

#### 2. Thêm tài khoản cho thuê

- Thanh điều hướng bên trái, **Administration** > **Accounts**
- Click **Add accounts**
- Mục **Select accounts**, sẽ thấy danh sách tài khoản có sẵn. Nếu không có gì thì check lại xem sandbox account đã được move vào OU **Entry** chưa
- Chọn một vài tài khoản, **Next** rồi **Submit**

![architect](/resources/_gen/images/Anha2.png "Architect")

- Quay lại **Accounts** để check trạng thái

#### 3. Quản lý cài đặt

- Thanh điều hướng, **Administration** > **Settings**
- Có ba tab:
  - **General Settings**
  - **Lease Settings** (ngân sách tối đa mặc định $100)
  - **Clean Up Settings**
- Muốn đổi thì phải dùng **AWS AppConfig** (hướng dẫn ở phần sau)

![architect](/resources/_gen/images/Anha3.png "Architect")

#### 4. Tạo Service Control Policy để giới hạn

Ví dụ: Tạo SCP để không cho user tạo EC2 instance loại `m5.large`.

**Các bước:**

- Đăng nhập **organization management account**
- Mở **AWS Organizations**
- Thanh điều hướng chọn **Policies**
- **Supported policy types** > **Service control policies**

![architect](/resources/_gen/images/Anha4.png "Architect")

- Click **Create policy**:
  - **Policy name:** RestrictEC2Instances
  - **Policy description:** This SCP restricts launching certain EC2 instance types.
- Dán JSON này vào policy editor:

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

- **Create policy**
- Quay lại **Policies**, chọn policy vừa tạo
- Tab **Targets**, click **Attach**
- Chọn OU tên `your-namespace_InnovationSandboxAccountPool`, **Attach policy**

![architect](/resources/_gen/images/Anha6.png "Architect")

**Kết quả:** User trong sandbox account không thể tạo EC2 instance `m5.large` nữa.

### Vai trò Manager

Manager có thể chỉnh sửa cài đặt và tạo template cho user request tài khoản.

#### Chỉnh sửa cài đặt bằng AppConfig

**Lưu ý quan trọng:** Làm ở **tài khoản hub**, không phải tài khoản quản lý. Đúng **home Region**.

**Các bước:**

- Đăng nhập tài khoản hub
- Thanh tìm kiếm console, nhập **AWS AppConfig**
- Thanh điều hướng chọn **Applications**
- Chọn app đã tạo
- Chọn **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-ID**

![architect](/resources/_gen/images/Manager1.png "Architect")

- **Hosted configuration versions** > **Create**
- Đổi **maxBudget** thành **50** (USD)

![architect](/resources/_gen/images/Manager2.png "Architect")

- **Create hosted configuration version**
- **Start deployment**
- Chọn phiên bản vừa tạo, giữ nguyên setting khác, **Start deployment**

### Tạo Lease Template

#### Lease Template là gì

Về cơ bản thì đây là bộ quy tắc định nghĩa user có thể xài tài nguyên thế nào:

- **Ngân sách tối đa:** Có thể tiêu bao nhiêu tiền
- **Thời gian:** Được xài bao lâu
- **Approval:** Có cần manager duyệt không
- **Cảnh báo:** Báo khi sắp hết tiền hoặc thời gian

Sẽ tạo hai loại:
- **Basic:** $25, tự động approve, cho test nhỏ
- **Advanced:** $50, cần approve, cho dự án lớn

### Tạo Basic Template

Template này cho những request không cần duyệt. $25 tối đa, báo khi tiêu $15, dùng được 168 tiếng (1 tuần), báo trước 24 tiếng khi hết hạn.

**Các bước:**

- Console Innovation Sandbox, chọn profile góc trên phải > **Sign out**
- Đăng nhập bằng tài khoản manager. Lần đầu dùng **Forgot password**
- Nếu có hai tab **Accounts** và **Applications**, chọn **Applications**
- Thanh điều hướng chọn **Lease Templates**
- **Add new lease template**
- **Name:** `Basic`
- **Description:** `Basic account lease template`
- Bỏ tick **Approval required** (không cần manager duyệt)

![architect](/resources/_gen/images/Manager3.png "Architect")

- **Next**
- **Maximum budget:** `100`, **Next**
- Nếu đã chỉnh AppConfig đúng sẽ báo lỗi ngân sách tối đa $50. Đổi **Maximum Budget Amount** thành `25`
- **Budget Thresholds** > **Add a threshold**:
  - **amount consumed:** `15`
  - **action:** **Send Alert**. User sẽ nhận cảnh báo khi tiêu $15, xóa account khi tiêu hết $25
- **Next**
- **Maximum Duration** > **Set a maximum duration** > `168`
- **Duration Thresholds** > **Add a threshold**:
  - **remaining hours:** `24`
  - **action:** **Send Alert**. Báo khi còn 24 tiếng
- **Submit**

![architect](/resources/_gen/images/Manager4.png "Architect")

### Tạo Advanced Template

Template này cần manager approve. $50 tối đa, báo ở $40, thời gian 168 tiếng, báo trước 24 tiếng.

**Các bước:**

- **Lease Templates** > **Add new lease template**
- **Name:** `Advanced`
- **Description:** `Advanced account lease template`
- Giữ nguyên **Approval required** (cần manager duyệt)
- **Next**
- **Maximum budget:** `50`, **Next**
- **Budget Thresholds** > **Add a threshold**:
  - **amount consumed:** `40`
  - **action:** **Send Alert**
- **Next**
- **Maximum Duration** > **Set a maximum duration** > `168`
- **Duration Thresholds** > **Add a threshold**:
  - **remaining hours:** `24`
  - **action:** **Send Alert**
- **Submit**

![architect](/resources/_gen/images/Manager5.png "Architect")

### Vai trò End-User

Giờ sẽ thử làm user thường, request tài khoản và xài.

#### 1. Request tài khoản cơ bản

Tài khoản basic sẽ được approve tự động.

**Các bước:**

- Giao diện Innovation Sandbox, chọn profile góc trên phải > **Sign out**
- Đăng nhập lại bằng tài khoản end user
- Nếu có hai tab thì chọn **Applications**
- **Request a new account**
- Chọn **Basic lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**

![architect](/resources/_gen/images/User1.png "Architect")

- **Comments:** nhập lý do (ví dụ: `Request for a basic account`)
- **Submit**
- Thanh điều hướng bên trái chọn **Home**. Mục **Account**, click **Refresh** để cập nhật

![architect](/resources/_gen/images/User2.png "Architect")

- Khi tài khoản được cấp, mục **Access** chọn **Login to account**
- Chọn role ở **Select a role** để vào AWS Management Console

![architect](/resources/_gen/images/User3.png "Architect")

#### 2. Test giới hạn SCP

Check xem SCP có hoạt động không bằng cách thử tạo EC2 instance bị cấm.

**Các bước:**

- AWS Management Console, nhập **EC2** vào thanh tìm kiếm
- Thanh điều hướng bên trái chọn **Instances**
- **Launch instances**
- **Name:** `test-scp`. Giữ nguyên AMI mặc định

![architect](/resources/_gen/images/User4.png "Architect")

- **Instance type:** chọn **m5.large** (loại này bị SCP chặn)
- **Key pair:** **Proceed without a key pair**
- Giữ nguyên setting khác, **Launch instance**
- Sẽ thấy báo lỗi do SCP không cho tạo **m5.large**

![architect](/resources/_gen/images/User5.png "Architect")

#### 3. Request tài khoản nâng cao

Tài khoản advanced cần manager approve.

**Các bước:**

- Quay lại giao diện Innovation Sandbox, đăng nhập end user
- **Request a new account**
- Chọn **Advanced lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**
- **Comments:** nhập lý do (ví dụ: `Request for an advanced account`)
- **Submit**

**Quy trình approve:**

- Đăng xuất, đăng nhập bằng tài khoản manager
- Trang chủ sẽ thấy request chờ duyệt. Thanh điều hướng chọn **Approvals**
- Chọn request, **Actions** > **Approve request**

**Quản lý lease:**

- Thanh điều hướng chọn **Leases**
- Sẽ thấy hai lease: basic và advanced
- Có thể chọn lease dùng **Actions** để terminate, suspend hoặc cập nhật

![architect](/resources/_gen/images/User6.png "Architect")

![architect](/resources/_gen/images/User7.png "Architect")

#### 4. Tổng kết

- **Administrator:** Setup hệ thống, tạo SCP kiểm soát tài nguyên, quản lý tài khoản, tích hợp AppConfig
- **Manager:** Cấu hình ngân sách, tạo lease template (Basic, Advanced), thiết lập quy trình approve, giám sát sử dụng
- **End user:** Request và truy cập tài khoản AWS, test SCP, dùng các loại tài khoản với quy trình approve phù hợp

Thế là xong, giờ bạn đã biết cách sử dụng Innovation Sandbox với cả 3 vai trò rồi!



