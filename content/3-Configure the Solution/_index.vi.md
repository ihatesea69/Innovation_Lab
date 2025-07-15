+++
title = "Configure the Solution"
date = 2025
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

### C?u hình h? th?ng

Okay gi? các stack dã ch?y xong r?i, nhung chua xài du?c dâu. Ph?i c?u hình thêm tí n?a m?i cho user vào du?c.

#### C?n làm gì

- Thi?t l?p SAML d? dang nh?p
- T?o user và nhóm trong IAM Identity Center
- C?u hình web app
- Ðua các tài kho?n sandbox vào dúng ch?

#### Thi?t l?p SAML

**1. C?u hình SAML**

- Vào AWS IAM Identity Center ? tài kho?n qu?n lý
- Ch?n "Settings" r?i chuy?n sang "Identity Source"
- Ch?n "External identity provider" d? dùng SAML 2.0

**2. T?i certificate**

- ? giao di?n SAML, t?i file certificate (.pem) c?a AWS IAM Identity Center
- Luu l?i file này dùng sau

#### T?o user và nhóm

**1. T?o nhóm**

- Vào IAM Identity Center, ch?n "Groups"
- T?o các nhóm theo vai trò (Admin, Developer, Viewer...)
- Gán quy?n cho t?ng nhóm

**2. Thêm user**

- Vào "Users" trong IAM Identity Center
- Thêm user m?i ho?c sync t? h? th?ng bên ngoài (n?u dã c?u hình SAML)
- Cho user vào nhóm phù h?p

#### C?u hình web app

**1. C?p nh?t AWS AppConfig**

- Vào AWS AppConfig ? tài kho?n hub
- Ch?n app Innovation Sandbox, c?p nh?t các tham s? c?n thi?t
- Check l?i thông tin có kh?p v?i môi tru?ng không

**2. C?u hình xác th?c**

- Ð?m b?o web app dùng IAM Identity Center d? dang nh?p
- Ki?m tra thông tin k?t n?i SAML

#### Ðua tài kho?n vào h? th?ng

**1. Di chuy?n tài kho?n**

- Vào AWS Organizations b?ng tài kho?n qu?n lý
- Chuy?n các tài kho?n (hub, sandbox...) vào dúng OU dã thi?t k?

**2. Ki?m tra l?i**

- Check trong AWS Organizations xem tài kho?n dã ? dúng ch? chua
- Ð?m b?o các policy và quy?n dã du?c áp d?ng dúng

**Th?i gian:** Kho?ng 15 phút

Làm xong các bu?c này thì h? th?ng m?i s?n sàng cho user vào xài du?c.

### Thi?t l?p SAML cho Innovation Sandbox

Ph?n này hoi k? thu?t tí, nhung c?n thi?t d? user dang nh?p du?c vào h? th?ng m?t cách an toàn.

#### Luu ý quan tr?ng

- Làm t?t c? ? **tài kho?n qu?n lý t? ch?c** - noi có IAM Identity Center
- Ð?m b?o dang ? dúng **home Region** dã ch?n

#### Các bu?c làm

**1. T?o SAML app trong IAM Identity Center**

- M? **AWS IAM Identity Center console** b?ng tài kho?n qu?n lý
- Thanh di?u hu?ng ch?n **Applications**
- Chuy?n sang tab **Customer managed**

![architect](/images/organiza.jpg "Architect")

**2. Thêm app m?i**

- Ch?n **Add application**
- Ch?n **I have an application I want to set up**
- Ch?n **SAML 2.0**, r?i **Next**

![architect](/images/Buoc3.png "Architect")

**3. C?u hình app**

- Trang c?u hình, nh?p **Display name** (ví d?: MyISB app) và mô t?

![architect](/images/Buoc5.png "Architect")

- Ph?n **IAM Identity Center metadata**:
  - T?i **SAML metadata file** 
  - T?i **certificate**

![architect](/images/Buoc6.png "Architect")

- Ghi l?i **Sign-in URL** và **Sign-out URL** - c?n dùng khi c?u hình ? tài kho?n hub

![architect](/images/Buoc7.png "Architect")

**4. Nh?p thông tin metadata**

- Ph?n **Application metadata**, ch?n **Manually type your metadata values**
- Nh?p các giá tr?:

  | Tru?ng                  | Nh?p gì                                                                                          |
  |-------------------------|--------------------------------------------------------------------------------------------------|
  | Application ACS URL     | `{ISB_WEB_URL}/api/auth/login/callback` (Thay `{ISB_WEB_URL}` b?ng CloudFrontDistributionUrl dã luu) |
  | Application SAML audience | Tên d?nh danh app (có th? dùng tên namespace, ví d?: `Isb-dev-namespace-Audience`). Luu l?i d? c?u hình AppConfig |

![architect](/images/Buoc8.png "Architect")

**5. Hoàn t?t**

- Nh?n **Submit**
- S? có thông báo c?u hình thành công
- Nh? c?u hình thêm **attribute mappings** cho IAM Identity Center

#### Thông tin c?n luu l?i

- **Sign-in URL** 
- **Sign-out URL** 
- **Audience value** 

Các thông tin này dùng d? c?u hình web app và xác th?c user.

### C?u hình User và Group

Thi?t l?p user và nhóm d? ki?m soát ai du?c vào và làm gì trong h? th?ng. Làm ? **tài kho?n qu?n lý** và dúng **home Region**.

#### 1. Map thu?c tính app

- M? **IAM Identity Center console**
- Ch?n **Applications** 
- Ch?n app v?a t?o (MyISB app)
- Trang chi ti?t app, ch?n **Actions** > **Edit attribute mappings**
- Ô th? hai nh?p: `${user:email}`
- C?t th? ba ch?n **emailAddress**
- **Save changes**

#### 2. Gán nhóm cho app

- Quay l?i app v?a c?u hình
- Ch?n **Assign users and groups**
- Tìm và thêm ba nhóm dã t?o b?i IDC stack:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Ch?n **Assign**

#### 3. Cho user vào nhóm

- IAM Identity Center console, ch?n **Users**
- Ch?n user mu?n gán nhóm (ho?c **Add user** d? thêm m?i)
- Tab **Groups**, nh?n **Add user to groups**
- Ch?n m?t trong ba nhóm:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- **Add user to 1 group**

![architect](/images/users-and-groups.png "Architect")

**Luu ý:** Ð? test d? ba vai trò, c?n ít nh?t ba user, m?i ngu?i m?t nhóm khác nhau.

### C?u hình Web App

Ph?n này kích ho?t dang nh?p và thi?t l?p các tham s? cho app. Làm ? **tài kho?n hub** và dúng **home Region**.

#### 1. C?u hình AWS AppConfig

- M? **AWS AppConfig console** ? tài kho?n hub
- Thanh di?u hu?ng ch?n **Applications**
- Ch?n **InnovationSandboxData-Config-Application-XXXXXXX**
- Ch?n **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-XXXXX**
- **Create** d? t?o phiên b?n c?u hình m?i

![architect](/images/appconfig.png "Architect")

**C?p nh?t c?u hình:**
- Ð?t `maintenanceMode` thành `false` d? m? app cho user

**Ph?n `auth`, c?p nh?t:**

| Tru?ng              | C?p nh?t gì                                                                                       |
|---------------------|---------------------------------------------------------------------------------------------------|
| idpSignInUrl        | URL dang nh?p t? bu?c c?u hình SAML                                                              |
| idpSignOutUrl       | URL dang xu?t t? bu?c c?u hình SAML                                                              |
| idpAudience         | Audience ID t? bu?c c?u hình SAML                                                                |
| webAppUrl           | CloudFront Distribution URL t? Outputs tab CloudFormation (tài kho?n hub)                       |
| awsAccessPortalUrl  | AWS Access Portal URL t? IAM Identity Center dashboard (tài kho?n qu?n lý)                      |
| sessionDurationInMinutes | Th?i gian session, ví d?: 240                                                               |
| emailFrom           | Email dã xác th?c c?a b?n                                                                         |

- **Create hosted configuration version**
- **Start Deployment** và ch?n phiên b?n v?a t?o
- **Start Deployment** d? áp d?ng

#### 2. C?p nh?t certificate trong Secrets Manager

- M? **AWS Secrets Manager console** ? tài kho?n hub
- Tìm secret: `/InnovationSandbox//Auth/IDPCert`
- Tab **Overview**, m?c **Secret value**:
  - **Retrieve secret value**
  - **Edit**
  - **Plaintext**
  - **Edit** l?n n?a
  - Dán n?i dung file certificate IAM Identity Center (.pem) dã t?i
  - **Save**

**Luu ý:** Ð?m b?o t?t c? thông tin chính xác và nh?t quán v?i các bu?c tru?c.

### Ðua Sandbox Account vào h? th?ng

Bu?c cu?i d? hoàn thành c?u hình. Chuy?n các tài kho?n AWS vào dúng c?u trúc và áp d?ng ki?m soát.

#### Luu ý quan tr?ng

- Làm ? **tài kho?n qu?n lý t? ch?c**
- Ðúng **home Region** dã ch?n

#### Các bu?c

- Ðang nh?p AWS Management Console b?ng tài kho?n qu?n lý
- M? **AWS Organizations console**

**2. Tìm tài kho?n c?n chuy?n**

- Ch?n **AWS accounts**
- Tìm các tài kho?n mu?n chuy?n vào môi tru?ng sandbox
- **Luu ý:** Ð?ng ch?n tài kho?n qu?n lý ho?c tài kho?n hub

![architect](/images/account-onboarding.png "Architect")

**3. Chuy?n vào Entry OU**

- Ch?n các tài kho?n c?n chuy?n
- Menu **Actions**, ch?n **Move** 
- H?p tho?i **Move AWS account**, click mui tên c?nh Innovation Sandbox OU d? m? r?ng
- Ch?n **Entry OU**
- **Move AWS accounts** d? xác nh?n

#### K?t qu?

- Các tài kho?n sandbox dã ? dúng v? trí trong c?u trúc Innovation Sandbox
- Các policy ki?m soát s? t? d?ng áp d?ng cho nh?ng tài kho?n này

Làm dúng bu?c này d?m b?o môi tru?ng sandbox ho?t d?ng riêng bi?t, an toàn và tuân th? chính sách t? ch?c.
