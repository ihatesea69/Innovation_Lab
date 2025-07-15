+++
title = "Configure the Solution"
date = 2025
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

### Cấu hình hệ thống

Okay giờ các stack đã chạy xong rồi, nhưng chưa xài được đâu. Phải cấu hình thêm tí nữa mới cho user vào được.

#### Cần làm gì

- Thiết lập SAML để đăng nhập
- Tạo user và nhóm trong IAM Identity Center
- Cấu hình web app
- Đưa các tài khoản sandbox vào đúng chỗ

#### Thiết lập SAML

**1. Cấu hình SAML**

- Vào AWS IAM Identity Center ở tài khoản quản lý
- Chọn "Settings" rồi chuyển sang "Identity Source"
- Chọn "External identity provider" để dùng SAML 2.0

**2. Tải certificate**

- Ở giao diện SAML, tải file certificate (.pem) của AWS IAM Identity Center
- Lưu lại file này dùng sau

#### Tạo user và nhóm

**1. Tạo nhóm**

- Vào IAM Identity Center, chọn "Groups"
- Tạo các nhóm theo vai trò (Admin, Developer, Viewer...)
- Gán quyền cho từng nhóm

**2. Thêm user**

- Vào "Users" trong IAM Identity Center
- Thêm user mới hoặc sync từ hệ thống bên ngoài (nếu đã cấu hình SAML)
- Cho user vào nhóm phù hợp

#### Cấu hình web app

**1. Cập nhật AWS AppConfig**

- Vào AWS AppConfig ở tài khoản hub
- Chọn app Innovation Sandbox, cập nhật các tham số cần thiết
- Check lại thông tin có khớp với môi trường không

**2. Cấu hình xác thực**

- Đảm bảo web app dùng IAM Identity Center để đăng nhập
- Kiểm tra thông tin kết nối SAML

#### Đưa tài khoản vào hệ thống

**1. Di chuyển tài khoản**

- Vào AWS Organizations bằng tài khoản quản lý
- Chuyển các tài khoản (hub, sandbox...) vào đúng OU đã thiết kế

**2. Kiểm tra lại**

- Check trong AWS Organizations xem tài khoản đã ở đúng chỗ chưa
- Đảm bảo các policy và quyền đã được áp dụng đúng

**Thời gian:** Khoảng 15 phút

Làm xong các bước này thì hệ thống mới sẵn sàng cho user vào xài được.

### Thiết lập SAML cho Innovation Sandbox

Phần này hơi kỹ thuật tí, nhưng cần thiết để user đăng nhập được vào hệ thống một cách an toàn.

#### Lưu ý quan trọng

- Làm tất cả ở **tài khoản quản lý tổ chức** - nơi có IAM Identity Center
- Đảm bảo đang ở đúng **home Region** đã chọn

#### Các bước làm

**1. Tạo SAML app trong IAM Identity Center**

- Mở **AWS IAM Identity Center console** bằng tài khoản quản lý
- Thanh điều hướng chọn **Applications**
- Chuyển sang tab **Customer managed**

![architect](../images/organiza.jpg "Architect")

**2. Thêm app mới**

- Chọn **Add application**
- Chọn **I have an application I want to set up**
- Chọn **SAML 2.0**, rồi **Next**

![architect](../images/Buoc3.png "Architect")

**3. Cấu hình app**

- Trang cấu hình, nhập **Display name** (ví dụ: MyISB app) và mô tả

![architect](../images/Buoc5.png "Architect")

- Phần **IAM Identity Center metadata**:
  - Tải **SAML metadata file** 
  - Tải **certificate**

![architect](../images/Buoc6.png "Architect")

- Ghi lại **Sign-in URL** và **Sign-out URL** - cần dùng khi cấu hình ở tài khoản hub

![architect](../images/Buoc7.png "Architect")

**4. Nhập thông tin metadata**

- Phần **Application metadata**, chọn **Manually type your metadata values**
- Nhập các giá trị:

  | Trường                  | Nhập gì                                                                                          |
  |-------------------------|--------------------------------------------------------------------------------------------------|
  | Application ACS URL     | `{ISB_WEB_URL}/api/auth/login/callback` (Thay `{ISB_WEB_URL}` bằng CloudFrontDistributionUrl đã lưu) |
  | Application SAML audience | Tên định danh app (có thể dùng tên namespace, ví dụ: `Isb-dev-namespace-Audience`). Lưu lại để cấu hình AppConfig |

![architect](../images/Buoc8.png "Architect")

**5. Hoàn tất**

- Nhấn **Submit**
- Sẽ có thông báo cấu hình thành công
- Nhớ cấu hình thêm **attribute mappings** cho IAM Identity Center

#### Thông tin cần lưu lại

- **Sign-in URL** 
- **Sign-out URL** 
- **Audience value** 

Các thông tin này dùng để cấu hình web app và xác thực user.

### Cấu hình User và Group

Thiết lập user và nhóm để kiểm soát ai được vào và làm gì trong hệ thống. Làm ở **tài khoản quản lý** và dùng **home Region**.

#### 1. Map thuộc tính app

- Mở **IAM Identity Center console**
- Chọn **Applications** 
- Chọn app vừa tạo (MyISB app)
- Trang chi tiết app, chọn **Actions** > **Edit attribute mappings**
- Ô thứ hai nhập: `${user:email}`
- Cột thứ ba chọn **emailAddress**
- **Save changes**

#### 2. Gán nhóm cho app

- Quay lại app vừa cấu hình
- Chọn **Assign users and groups**
- Tìm và thêm ba nhóm đã tạo bởi IDC stack:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Chọn **Assign**

#### 3. Cho user vào nhóm

- IAM Identity Center console, chọn **Users**
- Chọn user muốn gán nhóm (hoặc **Add user** để thêm mới)
- Tab **Groups**, nhấn **Add user to groups**
- Chọn một trong ba nhóm:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- **Add user to 1 group**

![architect](../images/users-and-groups.png "Architect")

**Lưu ý:** Để test đủ ba vai trò, cần ít nhất ba user, mỗi người một nhóm khác nhau.

### Cấu hình Web App

Phần này kích hoạt đăng nhập và thiết lập các tham số cho app. Làm ở **tài khoản hub** và dùng **home Region**.

#### 1. Cấu hình AWS AppConfig

- Mở **AWS AppConfig console** ở tài khoản hub
- Thanh điều hướng chọn **Applications**
- Chọn **InnovationSandboxData-Config-Application-XXXXXXX**
- Chọn **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-XXXXX**
- **Create** để tạo phiên bản cấu hình mới

![architect](../images/appconfig.png "Architect")

**Cập nhật cấu hình:**
- Đặt `maintenanceMode` thành `false` để mở app cho user

**Phần `auth`, cập nhật:**

| Trường              | Cập nhật gì                                                                                       |
|---------------------|---------------------------------------------------------------------------------------------------|
| idpSignInUrl        | URL đăng nhập từ bước cấu hình SAML                                                              |
| idpSignOutUrl       | URL đăng xuất từ bước cấu hình SAML                                                              |
| idpAudience         | Audience ID từ bước cấu hình SAML                                                                |
| webAppUrl           | CloudFront Distribution URL từ Outputs tab CloudFormation (tài khoản hub)                       |
| awsAccessPortalUrl  | AWS Access Portal URL từ IAM Identity Center dashboard (tài khoản quản lý)                      |
| sessionDurationInMinutes | Thời gian session, ví dụ: 240                                                               |
| emailFrom           | Email đã xác thực của bạn                                                                         |

- **Create hosted configuration version**
- **Start Deployment** và chọn phiên bản vừa tạo
- **Start Deployment** để áp dụng

#### 2. Cập nhật certificate trong Secrets Manager

- Mở **AWS Secrets Manager console** ở tài khoản hub
- Tìm secret: `/InnovationSandbox//Auth/IDPCert`
- Tab **Overview**, mục **Secret value**:
  - **Retrieve secret value**
  - **Edit**
  - **Plaintext**
  - **Edit** lần nữa
  - Dán nội dung file certificate IAM Identity Center (.pem) đã tải
  - **Save**

**Lưu ý:** Đảm bảo tất cả thông tin chính xác và nhất quán với các bước trước.

### Đưa Sandbox Account vào hệ thống

Bước cuối để hoàn thành cấu hình. Chuyển các tài khoản AWS vào đúng cấu trúc và áp dụng kiểm soát.

#### Lưu ý quan trọng

- Làm ở **tài khoản quản lý tổ chức**
- Đúng **home Region** đã chọn

#### Các bước

- Đăng nhập AWS Management Console bằng tài khoản quản lý
- Mở **AWS Organizations console**

**2. Tìm tài khoản cần chuyển**

- Chọn **AWS accounts**
- Tìm các tài khoản muốn chuyển vào môi trường sandbox
- **Lưu ý:** Đừng chọn tài khoản quản lý hoặc tài khoản hub

![architect](../images/account-onboarding.png "Architect")

**3. Chuyển vào Entry OU**

- Chọn các tài khoản cần chuyển
- Menu **Actions**, chọn **Move** 
- Hộp thoại **Move AWS account**, click mũi tên cạnh Innovation Sandbox OU để mở rộng
- Chọn **Entry OU**
- **Move AWS accounts** để xác nhận

#### Kết quả

- Các tài khoản sandbox đã ở đúng vị trí trong cấu trúc Innovation Sandbox
- Các policy kiểm soát sẽ tự động áp dụng cho những tài khoản này

Làm đúng bước này đảm bảo môi trường sandbox hoạt động riêng biệt, an toàn và tuân thủ chính sách tổ chức.
