+++
title = "Configure the Solution"
date = 2025
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

### Cấu hình Giải pháp (Configure the Solution)

Sau khi chạy đủ các stack, bạn cần thực hiện các bước cấu hình để cho phép người dùng truy cập và chuẩn bị môi trường Innovation Sandbox trên AWS để xài nha.

#### Tổng quan các bước cấu hình

- Thiết lập liên kết SAML 2.0 (SAML federation)
- Cấu hình người dùng AWS IAM Identity Center
- Cấu hình ứng dụng web
- Onboard (gắn kết) các tài khoản vào cấu trúc Innovation Sandbox

#### Thiết lập liên kết SAML 2.0

**1. Cấu hình tích hợp SAML**

- Truy cập AWS IAM Identity Center trong tài khoản quản lý tổ chức.
- Chọn mục “Settings” và chuyển sang phần “Identity Source”.
- Chọn “External identity provider” để sử dụng SAML 2.0.

**2. Tải về certification**

- Trong giao diện cấu hình SAML, tải về certificate (file .pem) của AWS IAM Identity Center.
- Lưu lại file này để sử dụng khi cấu hình trên hệ thống nhận dạng bên ngoài 

#### Cấu hình người dùng AWS IAM Identity Center

**1. Tạo và cấu hình nhóm (Groups)**

- Trong IAM Identity Center, vào mục “Groups”.
- Tạo các nhóm phù hợp với vai trò sử dụng (ví dụ: Admin, Developer, Viewer...).
- Gán quyền truy cập (permission sets) tương ứng cho từng nhóm.

**2. Thiết lập người dùng ban đầu**

- Vào mục “Users” trong IAM Identity Center.
- Thêm mới người dùng hoặc đồng bộ từ hệ thống nhận dạng ngoài (nếu đã cấu hình SAML).
- Gán người dùng vào các nhóm đã tạo.

#### Cấu hình ứng dụng web

**1. Cập nhật cài đặt AWS AppConfig**

- Truy cập AWS AppConfig trong tài khoản hub.
- Chọn ứng dụng Innovation Sandbox, cập nhật các thông số cấu hình cần thiết (ví dụ: endpoint, tham số môi trường, v.v.).
- Xác nhận các thông tin cấu hình đã phù hợp với môi trường triển khai.

**2. Cấu hình xác thực (Authentication)**

- Đảm bảo ứng dụng web sử dụng xác thực thông qua AWS IAM Identity Center.
- Kiểm tra lại thông tin kết nối SAML hoặc các tham số xác thực khác nếu có.

#### Onboard các tài khoản

**1. Di chuyển tài khoản vào cấu trúc Innovation Sandbox**

- Truy cập AWS Organizations bằng tài khoản quản lý.
- Di chuyển các tài khoản thành viên (hub, sandbox...) vào đúng organizational unit (OU) đã thiết kế cho Innovation Sandbox.

**2. Xác minh cấu hình tài khoản**

- Kiểm tra lại trong AWS Organizations để đảm bảo các tài khoản đã nằm đúng vị trí trong cấu trúc OU.
- Đảm bảo các chính sách, quyền truy cập và cấu hình liên quan đã được áp dụng chính xác.

**Thời gian dự kiến:** Khoảng 15 phút

Thực hiện đầy đủ các bước trên sẽ giúp giải pháp Innovation Sandbox trên AWS sẵn sàng cho người dùng truy cập và sử dụng, đồng thời đảm bảo tuân thủ các yêu cầu quản trị, bảo mật của tổ chức.

### Thiết lập Ứng dụng SAML 2.0 cho Innovation Sandbox trên AWS

Việc cấu hình tích hợp SAML 2.0 giữa AWS IAM Identity Center và Innovation Sandbox trên AWS giúp đảm bảo xác thực người dùng an toàn khi truy cập giao diện web của giải pháp.

#### Lưu ý quan trọng

- Thực hiện toàn bộ các bước dưới đây trong **tài khoản quản lý tổ chức** (organization management account) – nơi bạn đã cấu hình AWS IAM Identity Center.
- Đảm bảo bạn đang ở đúng **home Region** đã chọn cho toàn bộ hệ thống.

#### Các bước cấu hình

**1. Tạo ứng dụng SAML trong IAM Identity Center**

- Mở **AWS IAM Identity Center console** bằng tài khoản quản lý tổ chức.
- Trong thanh điều hướng, chọn **Applications**.
- Chuyển sang tab **Customer managed**.
![architect](/resources/_gen/images/organiza.jpg "Architect")

**2. Thêm ứng dụng mới**

- Chọn **Add application**.
- Chọn **I have an application I want to set up**.
- Chọn **SAML 2.0**, sau đó nhấn **Next**.

![architect](/resources/_gen/images/Buoc3.png "Architect")

**3. Cấu hình ứng dụng**

- Ở trang cấu hình, nhập **Display name** (ví dụ: MyISB app) và mô tả cho ứng dụng.
- 
![architect](/resources/_gen/images/Buoc5.png "Architect")

- Trong phần **IAM Identity Center metadata**:
  - Tải về **SAML metadata file** (metadata của nhà cung cấp danh tính).
  - Tải về **certificate** (chứng chỉ của nhà cung cấp danh tính).

![architect](/resources/_gen/images/Buoc6.png "Architect")


- Ghi chú lại các **Sign-in URL** và **Sign-out URL** – sẽ cần dùng khi cấu hình ở tài khoản hub.

![architect](/resources/_gen/images/Buoc7.png "Architect")

**4. Nhập thông tin metadata cho ứng dụng**

- Ở phần **Application metadata**, chọn **Manually type your metadata values**.
- Nhập các giá trị sau:

  | Trường                  | Giá trị nhập                                                                                          |
  |-------------------------|-------------------------------------------------------------------------------------------------------|
  | Application ACS URL     | `{ISB_WEB_URL}/api/auth/login/callback`  (Thay `{ISB_WEB_URL}` bằng CloudFrontDistributionUrl đã lưu, ví dụ: `https://duyXXXXXXXeh.cloudfront.net/api/auth/login/callback`) |
  | Application SAML audience | Giá trị định danh ứng dụng (có thể dùng tên namespace hoặc tên stack ban đầu, ví dụ: `Isb-dev-namespace-Audience`). Lưu lại giá trị này để cấu hình cho AppConfig ở tài khoản hub. |
![architect](/resources/_gen/images/Buoc8.png "Architect")
**5. Hoàn tất cấu hình**

- Nhấn **Submit**.
- Bạn sẽ nhận được thông báo xác nhận cấu hình thành công cho ứng dụng SAML.
- Lưu ý: Bạn cần cấu hình thêm **attribute mappings** cho IAM Identity Center để việc xác thực hoạt động đúng.

#### Thông tin cần lưu lại cho các bước cấu hình tiếp theo

- **Sign-in URL** (từ bước tải metadata)
- **Sign-out URL** (từ bước tải metadata)
- **Audience value** (giá trị đã nhập ở Application SAML audience)

Các thông tin này sẽ được sử dụng trong quá trình cấu hình ứng dụng web và xác thực người dùng cho Innovation Sandbox trên AWS.

### Cấu hình Người dùng và Nhóm trong IAM Identity Center

Việc thiết lập người dùng và nhóm trong AWS IAM Identity Center giúp kiểm soát truy cập và phân quyền cho người dùng sử dụng giải pháp Innovation Sandbox trên AWS. Thực hiện các bước dưới đây trong **tài khoản quản lý tổ chức** (organization management account) và đúng **home Region** đã chọn.

#### 1. Ánh xạ thuộc tính ứng dụng (Map application attributes)

- Mở **IAM Identity Center console** trong tài khoản quản lý.
- Chọn **Applications** trong thanh điều hướng.
- Trong danh sách **Customer managed applications**, chọn ứng dụng đã tạo (ví dụ: MyISB app).
- Ở trang chi tiết ứng dụng, chọn **Actions** > **Edit attribute mappings**.
- Trong ô thứ hai ("Maps to this string value or user attribute in IAM Identity Center"), nhập:  
  ```
  ${user:email}
  ```
- Ở cột thứ ba, chọn định dạng **emailAddress**.
- Nhấn **Save changes** để lưu cấu hình.

#### 2. Gán nhóm cho ứng dụng (Assign groups to your application)

- Quay lại ứng dụng vừa cấu hình.
- Chọn **Assign users and groups**.
- Tìm và thêm cả ba nhóm đã được tạo bởi IDC stack:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Chọn **Assign** để hoàn tất.

#### 3. Gán nhóm cho người dùng (Assign a group to your user)

- Trong IAM Identity Center console, chọn **Users** ở thanh điều hướng.
- Trong danh sách người dùng, chọn người dùng muốn gán vào nhóm. Để thêm người dùng mới, chọn **Add user**.
- Chọn tab **Groups**, sau đó nhấn **Add user to groups**.
- Chọn một trong ba nhóm đã tạo:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Nhấn **Add user to 1 group** để hoàn tất.

![architect](/resources/_gen/images/users-and-groups.png "Architect")

**Lưu ý:**  
Để kiểm thử đủ ba vai trò người dùng (user personas), hãy đảm bảo có ít nhất ba người dùng, mỗi người thuộc một nhóm khác nhau.

Sau khi hoàn thành các bước trên, bạn đã cấu hình thành công người dùng và nhóm cho giải pháp Innovation Sandbox trên AWS. Tiếp theo, bạn sẽ tiến hành cấu hình ứng dụng web trên tài khoản hub.

### Cấu hình Ứng dụng Web với AWS AppConfig và AWS Secrets Manager

Việc cấu hình ứng dụng web Innovation Sandbox trên AWS giúp kích hoạt xác thực người dùng và thiết lập các thông số toàn cục cho ứng dụng. Các thao tác này cần được thực hiện trong **tài khoản hub** và đúng **home Region** đã chọn.

#### 1. Cấu hình AWS AppConfig

- **Mở AWS AppConfig console** trong tài khoản hub.
- Ở thanh điều hướng, chọn **Applications**.
- Chọn ứng dụng **InnovationSandboxData-Config-Application-XXXXXXX**.
- Chọn **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-XXXXX** (configuration profile).
- Chọn **Create** để tạo một phiên bản cấu hình mới (hosted configuration version).


![architect](/resources/_gen/images/appconfig.png "Architect")

**Cập nhật cấu hình:**
- Đặt giá trị `maintenanceMode` thành `false` để mở ứng dụng cho người dùng.

**Trong phần `auth`, cập nhật các trường sau:**

| Trường              | Giá trị cập nhật                                                                                       |
|---------------------|-------------------------------------------------------------------------------------------------------|
| idpSignInUrl        | URL đăng nhập từ bước cấu hình SAML 2.0 Application                                                  |
| idpSignOutUrl       | URL đăng xuất từ bước cấu hình SAML 2.0 Application                                                  |
| idpAudience         | Audience ID từ bước cấu hình SAML 2.0 Application                                                    |
| webAppUrl           | CloudFront Distribution URL (Innovation Sandbox URL) từ Outputs tab của CloudFormation (tài khoản hub)|
| awsAccessPortalUrl  | AWS Access Portal URL từ IAM Identity Center dashboard (tài khoản quản lý tổ chức)                   |
| sessionDurationInMinutes | Thời lượng phiên đăng nhập, ví dụ: 240                                                           |
| emailFrom           | Địa chỉ email đăng nhập đã xác thực của bạn                                                           |

- Sau khi cập nhật, chọn **Create hosted configuration version**.
- Chọn **Start Deployment** và chọn phiên bản cấu hình vừa tạo.
- Chọn **Start Deployment** để áp dụng cấu hình mới.

#### 2. Cập nhật giá trị chứng chỉ trong AWS Secrets Manager

- Mở **AWS Secrets Manager console** trong tài khoản hub.
- Tìm và chọn secret:  
  `/InnovationSandbox//Auth/IDPCert`
- Ở tab **Overview**, mục **Secret value**:
  - Chọn **Retrieve secret value**.
  - Chọn **Edit**.
  - Chọn **Plaintext**.
  - Chọn **Edit** lần nữa.
  - Dán nội dung file chứng chỉ IAM Identity Center (.pem) đã tải về khi tạo ứng dụng SAML 2.0.
  - Chọn **Save** để lưu lại giá trị mới.

**Lưu ý:**  
- Đảm bảo mọi giá trị đều chính xác và nhất quán với các thông tin đã lấy ở các bước cấu hình trước (SAML, CloudFront, IAM Identity Center).
- Việc cấu hình đúng giúp hệ thống xác thực và phân quyền người dùng hiệu quả, đồng thời đảm bảo ứng dụng web Innovation Sandbox trên AWS hoạt động ổn định và an toàn.

### Onboard Sandbox Accounts vào Innovation Sandbox trên AWS

Việc onboard các tài khoản sandbox là bước cấu hình cuối cùng để hoàn thiện giải pháp Innovation Sandbox trên AWS. Bước này giúp di chuyển các tài khoản AWS vào đúng cấu trúc tổ chức và áp dụng các kiểm soát cần thiết.

#### Lưu ý quan trọng

- Thực hiện các bước này trong **tài khoản quản lý tổ chức** (organization management account) – nơi đã cấu hình AWS IAM Identity Center.
- Đảm bảo bạn đang ở đúng **home Region** đã chọn cho toàn bộ hệ thống.

#### Các bước Onboard Tài khoản Sandbox

**1. Mở AWS Organizations console**

- Đăng nhập vào AWS Management Console bằng tài khoản quản lý tổ chức.
- Mở **AWS Organizations console**.


**2. Tìm và chọn các tài khoản cần di chuyển**

- Chọn mục **AWS accounts**.
- Tìm các tài khoản bạn muốn chuyển vào môi trường sandbox. Có thể duyệt theo cấu trúc OU hoặc chọn **List** để xem danh sách tài khoản dạng phẳng.
- **Lưu ý:** Không chọn tài khoản quản lý tổ chức hoặc tài khoản hub.
  
![architect](/resources/_gen/images/account-onboarding.png "Architect")

**3. Di chuyển tài khoản vào OU Entry**

- Đánh dấu chọn các tài khoản cần di chuyển.
- Tại menu **Actions** (hoặc menu thao tác tương ứng), chọn **Move** dưới mục AWS account.
- Trong hộp thoại **Move AWS account**, nhấn vào mũi tên cạnh OU của Innovation Sandbox on AWS (OU này đã được tạo khi triển khai AccountPool stack) để mở rộng và xem các OU con.
- Chọn **Entry OU** (OU con dùng cho sandbox accounts).
- Nhấn **Move AWS accounts** để xác nhận di chuyển.

#### Kết quả

- Các tài khoản sandbox đã được di chuyển vào đúng OU trong cấu trúc tổ chức Innovation Sandbox trên AWS.
- Các chính sách kiểm soát và cấu hình liên quan sẽ tự động được áp dụng cho các tài khoản này.

Việc onboard đúng các tài khoản vào Entry OU đảm bảo môi trường sandbox hoạt động tách biệt, an toàn và tuân thủ các chính sách quản trị của tổ chức.