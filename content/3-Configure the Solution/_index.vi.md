+++
title = "Configure the Solution"
date = 2025
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

### C?u h�nh h? th?ng

Okay gi? c�c stack d� ch?y xong r?i, nhung chua x�i du?c d�u. Ph?i c?u h�nh th�m t� n?a m?i cho user v�o du?c.

#### C?n l�m g�

- Thi?t l?p SAML d? dang nh?p
- T?o user v� nh�m trong IAM Identity Center
- C?u h�nh web app
- �ua c�c t�i kho?n sandbox v�o d�ng ch?

#### Thi?t l?p SAML

**1. C?u h�nh SAML**

- V�o AWS IAM Identity Center ? t�i kho?n qu?n l�
- Ch?n "Settings" r?i chuy?n sang "Identity Source"
- Ch?n "External identity provider" d? d�ng SAML 2.0

**2. T?i certificate**

- ? giao di?n SAML, t?i file certificate (.pem) c?a AWS IAM Identity Center
- Luu l?i file n�y d�ng sau

#### T?o user v� nh�m

**1. T?o nh�m**

- V�o IAM Identity Center, ch?n "Groups"
- T?o c�c nh�m theo vai tr� (Admin, Developer, Viewer...)
- G�n quy?n cho t?ng nh�m

**2. Th�m user**

- V�o "Users" trong IAM Identity Center
- Th�m user m?i ho?c sync t? h? th?ng b�n ngo�i (n?u d� c?u h�nh SAML)
- Cho user v�o nh�m ph� h?p

#### C?u h�nh web app

**1. C?p nh?t AWS AppConfig**

- V�o AWS AppConfig ? t�i kho?n hub
- Ch?n app Innovation Sandbox, c?p nh?t c�c tham s? c?n thi?t
- Check l?i th�ng tin c� kh?p v?i m�i tru?ng kh�ng

**2. C?u h�nh x�c th?c**

- �?m b?o web app d�ng IAM Identity Center d? dang nh?p
- Ki?m tra th�ng tin k?t n?i SAML

#### �ua t�i kho?n v�o h? th?ng

**1. Di chuy?n t�i kho?n**

- V�o AWS Organizations b?ng t�i kho?n qu?n l�
- Chuy?n c�c t�i kho?n (hub, sandbox...) v�o d�ng OU d� thi?t k?

**2. Ki?m tra l?i**

- Check trong AWS Organizations xem t�i kho?n d� ? d�ng ch? chua
- �?m b?o c�c policy v� quy?n d� du?c �p d?ng d�ng

**Th?i gian:** Kho?ng 15 ph�t

L�m xong c�c bu?c n�y th� h? th?ng m?i s?n s�ng cho user v�o x�i du?c.

### Thi?t l?p SAML cho Innovation Sandbox

Ph?n n�y hoi k? thu?t t�, nhung c?n thi?t d? user dang nh?p du?c v�o h? th?ng m?t c�ch an to�n.

#### Luu � quan tr?ng

- L�m t?t c? ? **t�i kho?n qu?n l� t? ch?c** - noi c� IAM Identity Center
- �?m b?o dang ? d�ng **home Region** d� ch?n

#### C�c bu?c l�m

**1. T?o SAML app trong IAM Identity Center**

- M? **AWS IAM Identity Center console** b?ng t�i kho?n qu?n l�
- Thanh di?u hu?ng ch?n **Applications**
- Chuy?n sang tab **Customer managed**

![architect](/images/organiza.jpg "Architect")

**2. Th�m app m?i**

- Ch?n **Add application**
- Ch?n **I have an application I want to set up**
- Ch?n **SAML 2.0**, r?i **Next**

![architect](/images/Buoc3.png "Architect")

**3. C?u h�nh app**

- Trang c?u h�nh, nh?p **Display name** (v� d?: MyISB app) v� m� t?

![architect](/images/Buoc5.png "Architect")

- Ph?n **IAM Identity Center metadata**:
  - T?i **SAML metadata file** 
  - T?i **certificate**

![architect](/images/Buoc6.png "Architect")

- Ghi l?i **Sign-in URL** v� **Sign-out URL** - c?n d�ng khi c?u h�nh ? t�i kho?n hub

![architect](/images/Buoc7.png "Architect")

**4. Nh?p th�ng tin metadata**

- Ph?n **Application metadata**, ch?n **Manually type your metadata values**
- Nh?p c�c gi� tr?:

  | Tru?ng                  | Nh?p g�                                                                                          |
  |-------------------------|--------------------------------------------------------------------------------------------------|
  | Application ACS URL     | `{ISB_WEB_URL}/api/auth/login/callback` (Thay `{ISB_WEB_URL}` b?ng CloudFrontDistributionUrl d� luu) |
  | Application SAML audience | T�n d?nh danh app (c� th? d�ng t�n namespace, v� d?: `Isb-dev-namespace-Audience`). Luu l?i d? c?u h�nh AppConfig |

![architect](/images/Buoc8.png "Architect")

**5. Ho�n t?t**

- Nh?n **Submit**
- S? c� th�ng b�o c?u h�nh th�nh c�ng
- Nh? c?u h�nh th�m **attribute mappings** cho IAM Identity Center

#### Th�ng tin c?n luu l?i

- **Sign-in URL** 
- **Sign-out URL** 
- **Audience value** 

C�c th�ng tin n�y d�ng d? c?u h�nh web app v� x�c th?c user.

### C?u h�nh User v� Group

Thi?t l?p user v� nh�m d? ki?m so�t ai du?c v�o v� l�m g� trong h? th?ng. L�m ? **t�i kho?n qu?n l�** v� d�ng **home Region**.

#### 1. Map thu?c t�nh app

- M? **IAM Identity Center console**
- Ch?n **Applications** 
- Ch?n app v?a t?o (MyISB app)
- Trang chi ti?t app, ch?n **Actions** > **Edit attribute mappings**
- � th? hai nh?p: `${user:email}`
- C?t th? ba ch?n **emailAddress**
- **Save changes**

#### 2. G�n nh�m cho app

- Quay l?i app v?a c?u h�nh
- Ch?n **Assign users and groups**
- T�m v� th�m ba nh�m d� t?o b?i IDC stack:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Ch?n **Assign**

#### 3. Cho user v�o nh�m

- IAM Identity Center console, ch?n **Users**
- Ch?n user mu?n g�n nh�m (ho?c **Add user** d? th�m m?i)
- Tab **Groups**, nh?n **Add user to groups**
- Ch?n m?t trong ba nh�m:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- **Add user to 1 group**

![architect](/images/users-and-groups.png "Architect")

**Luu �:** �? test d? ba vai tr�, c?n �t nh?t ba user, m?i ngu?i m?t nh�m kh�c nhau.

### C?u h�nh Web App

Ph?n n�y k�ch ho?t dang nh?p v� thi?t l?p c�c tham s? cho app. L�m ? **t�i kho?n hub** v� d�ng **home Region**.

#### 1. C?u h�nh AWS AppConfig

- M? **AWS AppConfig console** ? t�i kho?n hub
- Thanh di?u hu?ng ch?n **Applications**
- Ch?n **InnovationSandboxData-Config-Application-XXXXXXX**
- Ch?n **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-XXXXX**
- **Create** d? t?o phi�n b?n c?u h�nh m?i

![architect](/images/appconfig.png "Architect")

**C?p nh?t c?u h�nh:**
- �?t `maintenanceMode` th�nh `false` d? m? app cho user

**Ph?n `auth`, c?p nh?t:**

| Tru?ng              | C?p nh?t g�                                                                                       |
|---------------------|---------------------------------------------------------------------------------------------------|
| idpSignInUrl        | URL dang nh?p t? bu?c c?u h�nh SAML                                                              |
| idpSignOutUrl       | URL dang xu?t t? bu?c c?u h�nh SAML                                                              |
| idpAudience         | Audience ID t? bu?c c?u h�nh SAML                                                                |
| webAppUrl           | CloudFront Distribution URL t? Outputs tab CloudFormation (t�i kho?n hub)                       |
| awsAccessPortalUrl  | AWS Access Portal URL t? IAM Identity Center dashboard (t�i kho?n qu?n l�)                      |
| sessionDurationInMinutes | Th?i gian session, v� d?: 240                                                               |
| emailFrom           | Email d� x�c th?c c?a b?n                                                                         |

- **Create hosted configuration version**
- **Start Deployment** v� ch?n phi�n b?n v?a t?o
- **Start Deployment** d? �p d?ng

#### 2. C?p nh?t certificate trong Secrets Manager

- M? **AWS Secrets Manager console** ? t�i kho?n hub
- T�m secret: `/InnovationSandbox//Auth/IDPCert`
- Tab **Overview**, m?c **Secret value**:
  - **Retrieve secret value**
  - **Edit**
  - **Plaintext**
  - **Edit** l?n n?a
  - D�n n?i dung file certificate IAM Identity Center (.pem) d� t?i
  - **Save**

**Luu �:** �?m b?o t?t c? th�ng tin ch�nh x�c v� nh?t qu�n v?i c�c bu?c tru?c.

### �ua Sandbox Account v�o h? th?ng

Bu?c cu?i d? ho�n th�nh c?u h�nh. Chuy?n c�c t�i kho?n AWS v�o d�ng c?u tr�c v� �p d?ng ki?m so�t.

#### Luu � quan tr?ng

- L�m ? **t�i kho?n qu?n l� t? ch?c**
- ��ng **home Region** d� ch?n

#### C�c bu?c

- �ang nh?p AWS Management Console b?ng t�i kho?n qu?n l�
- M? **AWS Organizations console**

**2. T�m t�i kho?n c?n chuy?n**

- Ch?n **AWS accounts**
- T�m c�c t�i kho?n mu?n chuy?n v�o m�i tru?ng sandbox
- **Luu �:** �?ng ch?n t�i kho?n qu?n l� ho?c t�i kho?n hub

![architect](/images/account-onboarding.png "Architect")

**3. Chuy?n v�o Entry OU**

- Ch?n c�c t�i kho?n c?n chuy?n
- Menu **Actions**, ch?n **Move** 
- H?p tho?i **Move AWS account**, click mui t�n c?nh Innovation Sandbox OU d? m? r?ng
- Ch?n **Entry OU**
- **Move AWS accounts** d? x�c nh?n

#### K?t qu?

- C�c t�i kho?n sandbox d� ? d�ng v? tr� trong c?u tr�c Innovation Sandbox
- C�c policy ki?m so�t s? t? d?ng �p d?ng cho nh?ng t�i kho?n n�y

L�m d�ng bu?c n�y d?m b?o m�i tru?ng sandbox ho?t d?ng ri�ng bi?t, an to�n v� tu�n th? ch�nh s�ch t? ch?c.
