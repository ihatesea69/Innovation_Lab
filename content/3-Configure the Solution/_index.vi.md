+++
title = "Khá»Ÿi táº¡o Target Group"
date = 2021
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

### Cáº¥u hÃ¬nh há»‡ thá»‘ng

Okay giá» cÃ¡c stack Ä‘Ã£ cháº¡y xong rá»“i, nhÆ°ng chÆ°a xÃ i Ä‘Æ°á»£c Ä‘Ã¢u. Pháº£i cáº¥u hÃ¬nh thÃªm tÃ­ ná»¯a má»›i cho user vÃ o Ä‘Æ°á»£c.

#### Cáº§n lÃ m gÃ¬

- Thiáº¿t láº­p SAML Ä‘á»ƒ Ä‘Äƒng nháº­p
- Táº¡o user vÃ  nhÃ³m trong IAM Identity Center
- Cáº¥u hÃ¬nh web app
- ÄÆ°a cÃ¡c tÃ i khoáº£n sandbox vÃ o Ä‘Ãºng chá»—

#### Thiáº¿t láº­p SAML

**1. Cáº¥u hÃ¬nh SAML**

- VÃ o AWS IAM Identity Center á»Ÿ tÃ i khoáº£n quáº£n lÃ½
- Chá»n "Settings" rá»“i chuyá»ƒn sang "Identity Source"
- Chá»n "External identity provider" Ä‘á»ƒ dÃ¹ng SAML 2.0

**2. Táº£i certificate**

- á»ž giao diá»‡n SAML, táº£i file certificate (.pem) cá»§a AWS IAM Identity Center
- LÆ°u láº¡i file nÃ y dÃ¹ng sau

#### Táº¡o user vÃ  nhÃ³m

**1. Táº¡o nhÃ³m**

- VÃ o IAM Identity Center, chá»n "Groups"
- Táº¡o cÃ¡c nhÃ³m theo vai trÃ² (Admin, Developer, Viewer...)
- GÃ¡n quyá»n cho tá»«ng nhÃ³m

**2. ThÃªm user**

- VÃ o "Users" trong IAM Identity Center
- ThÃªm user má»›i hoáº·c sync tá»« há»‡ thá»‘ng bÃªn ngoÃ i (náº¿u Ä‘Ã£ cáº¥u hÃ¬nh SAML)
- Cho user vÃ o nhÃ³m phÃ¹ há»£p

#### Cáº¥u hÃ¬nh web app

**1. Cáº­p nháº­t AWS AppConfig**

- VÃ o AWS AppConfig á»Ÿ tÃ i khoáº£n hub
- Chá»n app Innovation Sandbox, cáº­p nháº­t cÃ¡c tham sá»‘ cáº§n thiáº¿t
- Check láº¡i thÃ´ng tin cÃ³ khá»›p vá»›i mÃ´i trÆ°á»ng khÃ´ng

**2. Cáº¥u hÃ¬nh xÃ¡c thá»±c**

- Äáº£m báº£o web app dÃ¹ng IAM Identity Center Ä‘á»ƒ Ä‘Äƒng nháº­p
- Kiá»ƒm tra thÃ´ng tin káº¿t ná»‘i SAML

#### ÄÆ°a tÃ i khoáº£n vÃ o há»‡ thá»‘ng

**1. Di chuyá»ƒn tÃ i khoáº£n**

- VÃ o AWS Organizations báº±ng tÃ i khoáº£n quáº£n lÃ½
- Chuyá»ƒn cÃ¡c tÃ i khoáº£n (hub, sandbox...) vÃ o Ä‘Ãºng OU Ä‘Ã£ thiáº¿t káº¿

**2. Kiá»ƒm tra láº¡i**

- Check trong AWS Organizations xem tÃ i khoáº£n Ä‘Ã£ á»Ÿ Ä‘Ãºng chá»— chÆ°a
- Äáº£m báº£o cÃ¡c policy vÃ  quyá»n Ä‘Ã£ Ä‘Æ°á»£c Ã¡p dá»¥ng Ä‘Ãºng

**Thá»i gian:** Khoáº£ng 15 phÃºt

LÃ m xong cÃ¡c bÆ°á»›c nÃ y thÃ¬ há»‡ thá»‘ng má»›i sáºµn sÃ ng cho user vÃ o xÃ i Ä‘Æ°á»£c.

### Thiáº¿t láº­p SAML cho Innovation Sandbox

Pháº§n nÃ y hÆ¡i ká»¹ thuáº­t tÃ­, nhÆ°ng cáº§n thiáº¿t Ä‘á»ƒ user Ä‘Äƒng nháº­p Ä‘Æ°á»£c vÃ o há»‡ thá»‘ng má»™t cÃ¡ch an toÃ n.

#### LÆ°u Ã½ quan trá»ng

- LÃ m táº¥t cáº£ á»Ÿ **tÃ i khoáº£n quáº£n lÃ½ tá»• chá»©c** - nÆ¡i cÃ³ IAM Identity Center
- Äáº£m báº£o Ä‘ang á»Ÿ Ä‘Ãºng **home Region** Ä‘Ã£ chá»n

#### CÃ¡c bÆ°á»›c lÃ m

**1. Táº¡o SAML app trong IAM Identity Center**

- Má»Ÿ **AWS IAM Identity Center console** báº±ng tÃ i khoáº£n quáº£n lÃ½
- Thanh Ä‘iá»u hÆ°á»›ng chá»n **Applications**
- Chuyá»ƒn sang tab **Customer managed**

![architect](/images/organiza.jpg "Architect")

**2. ThÃªm app má»›i**

- Chá»n **Add application**
- Chá»n **I have an application I want to set up**
- Chá»n **SAML 2.0**, rá»“i **Next**

![architect](/images/Buoc3.png "Architect")

**3. Cáº¥u hÃ¬nh app**

- Trang cáº¥u hÃ¬nh, nháº­p **Display name** (vÃ­ dá»¥: MyISB app) vÃ  mÃ´ táº£

![architect](/images/Buoc5.png "Architect")

- Pháº§n **IAM Identity Center metadata**:
  - Táº£i **SAML metadata file** 
  - Táº£i **certificate**

![architect](/images/Buoc6.png "Architect")

- Ghi láº¡i **Sign-in URL** vÃ  **Sign-out URL** - cáº§n dÃ¹ng khi cáº¥u hÃ¬nh á»Ÿ tÃ i khoáº£n hub

![architect](/images/Buoc7.png "Architect")

**4. Nháº­p thÃ´ng tin metadata**

- Pháº§n **Application metadata**, chá»n **Manually type your metadata values**
- Nháº­p cÃ¡c giÃ¡ trá»‹:

  | TrÆ°á»ng                  | Nháº­p gÃ¬                                                                                          |
  |-------------------------|--------------------------------------------------------------------------------------------------|
  | Application ACS URL     | `{ISB_WEB_URL}/api/auth/login/callback` (Thay `{ISB_WEB_URL}` báº±ng CloudFrontDistributionUrl Ä‘Ã£ lÆ°u) |
  | Application SAML audience | TÃªn Ä‘á»‹nh danh app (cÃ³ thá»ƒ dÃ¹ng tÃªn namespace, vÃ­ dá»¥: `Isb-dev-namespace-Audience`). LÆ°u láº¡i Ä‘á»ƒ cáº¥u hÃ¬nh AppConfig |

![architect](/images/Buoc8.png "Architect")

**5. HoÃ n táº¥t**

- Nháº¥n **Submit**
- Sáº½ cÃ³ thÃ´ng bÃ¡o cáº¥u hÃ¬nh thÃ nh cÃ´ng
- Nhá»› cáº¥u hÃ¬nh thÃªm **attribute mappings** cho IAM Identity Center

#### ThÃ´ng tin cáº§n lÆ°u láº¡i

- **Sign-in URL** 
- **Sign-out URL** 
- **Audience value** 

CÃ¡c thÃ´ng tin nÃ y dÃ¹ng Ä‘á»ƒ cáº¥u hÃ¬nh web app vÃ  xÃ¡c thá»±c user.

### Cáº¥u hÃ¬nh User vÃ  Group

Thiáº¿t láº­p user vÃ  nhÃ³m Ä‘á»ƒ kiá»ƒm soÃ¡t ai Ä‘Æ°á»£c vÃ o vÃ  lÃ m gÃ¬ trong há»‡ thá»‘ng. LÃ m á»Ÿ **tÃ i khoáº£n quáº£n lÃ½** vÃ  Ä‘Ãºng **home Region**.

#### 1. Map thuá»™c tÃ­nh app

- Má»Ÿ **IAM Identity Center console**
- Chá»n **Applications** 
- Chá»n app vá»«a táº¡o (MyISB app)
- Trang chi tiáº¿t app, chá»n **Actions** > **Edit attribute mappings**
- Ã” thá»© hai nháº­p: `${user:email}`
- Cá»™t thá»© ba chá»n **emailAddress**
- **Save changes**

#### 2. GÃ¡n nhÃ³m cho app

- Quay láº¡i app vá»«a cáº¥u hÃ¬nh
- Chá»n **Assign users and groups**
- TÃ¬m vÃ  thÃªm ba nhÃ³m Ä‘Ã£ táº¡o bá»Ÿi IDC stack:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Chá»n **Assign**

#### 3. Cho user vÃ o nhÃ³m

- IAM Identity Center console, chá»n **Users**
- Chá»n user muá»‘n gÃ¡n nhÃ³m (hoáº·c **Add user** Ä‘á»ƒ thÃªm má»›i)
- Tab **Groups**, nháº¥n **Add user to groups**
- Chá»n má»™t trong ba nhÃ³m:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- **Add user to 1 group**

![architect](/images/users-and-groups.png "Architect")

**LÆ°u Ã½:** Äá»ƒ test Ä‘á»§ ba vai trÃ², cáº§n Ã­t nháº¥t ba user, má»—i ngÆ°á»i má»™t nhÃ³m khÃ¡c nhau.

### Cáº¥u hÃ¬nh Web App

Pháº§n nÃ y kÃ­ch hoáº¡t Ä‘Äƒng nháº­p vÃ  thiáº¿t láº­p cÃ¡c tham sá»‘ cho app. LÃ m á»Ÿ **tÃ i khoáº£n hub** vÃ  Ä‘Ãºng **home Region**.

#### 1. Cáº¥u hÃ¬nh AWS AppConfig

- Má»Ÿ **AWS AppConfig console** á»Ÿ tÃ i khoáº£n hub
- Thanh Ä‘iá»u hÆ°á»›ng chá»n **Applications**
- Chá»n **InnovationSandboxData-Config-Application-XXXXXXX**
- Chá»n **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-XXXXX**
- **Create** Ä‘á»ƒ táº¡o phiÃªn báº£n cáº¥u hÃ¬nh má»›i

![architect](/images/appconfig.png "Architect")

**Cáº­p nháº­t cáº¥u hÃ¬nh:**
- Äáº·t `maintenanceMode` thÃ nh `false` Ä‘á»ƒ má»Ÿ app cho user

**Pháº§n `auth`, cáº­p nháº­t:**

| TrÆ°á»ng              | Cáº­p nháº­t gÃ¬                                                                                       |
|---------------------|---------------------------------------------------------------------------------------------------|
| idpSignInUrl        | URL Ä‘Äƒng nháº­p tá»« bÆ°á»›c cáº¥u hÃ¬nh SAML                                                              |
| idpSignOutUrl       | URL Ä‘Äƒng xuáº¥t tá»« bÆ°á»›c cáº¥u hÃ¬nh SAML                                                              |
| idpAudience         | Audience ID tá»« bÆ°á»›c cáº¥u hÃ¬nh SAML                                                                |
| webAppUrl           | CloudFront Distribution URL tá»« Outputs tab CloudFormation (tÃ i khoáº£n hub)                       |
| awsAccessPortalUrl  | AWS Access Portal URL tá»« IAM Identity Center dashboard (tÃ i khoáº£n quáº£n lÃ½)                      |
| sessionDurationInMinutes | Thá»i gian session, vÃ­ dá»¥: 240                                                               |
| emailFrom           | Email Ä‘Ã£ xÃ¡c thá»±c cá»§a báº¡n                                                                         |

- **Create hosted configuration version**
- **Start Deployment** vÃ  chá»n phiÃªn báº£n vá»«a táº¡o
- **Start Deployment** Ä‘á»ƒ Ã¡p dá»¥ng

#### 2. Cáº­p nháº­t certificate trong Secrets Manager

- Má»Ÿ **AWS Secrets Manager console** á»Ÿ tÃ i khoáº£n hub
- TÃ¬m secret: `/InnovationSandbox//Auth/IDPCert`
- Tab **Overview**, má»¥c **Secret value**:
  - **Retrieve secret value**
  - **Edit**
  - **Plaintext**
  - **Edit** láº§n ná»¯a
  - DÃ¡n ná»™i dung file certificate IAM Identity Center (.pem) Ä‘Ã£ táº£i
  - **Save**

**LÆ°u Ã½:** Äáº£m báº£o táº¥t cáº£ thÃ´ng tin chÃ­nh xÃ¡c vÃ  nháº¥t quÃ¡n vá»›i cÃ¡c bÆ°á»›c trÆ°á»›c.

### ÄÆ°a Sandbox Account vÃ o há»‡ thá»‘ng

BÆ°á»›c cuá»‘i Ä‘á»ƒ hoÃ n thÃ nh cáº¥u hÃ¬nh. Chuyá»ƒn cÃ¡c tÃ i khoáº£n AWS vÃ o Ä‘Ãºng cáº¥u trÃºc vÃ  Ã¡p dá»¥ng kiá»ƒm soÃ¡t.

#### LÆ°u Ã½ quan trá»ng

- LÃ m á»Ÿ **tÃ i khoáº£n quáº£n lÃ½ tá»• chá»©c**
- ÄÃºng **home Region** Ä‘Ã£ chá»n

#### CÃ¡c bÆ°á»›c

- ÄÄƒng nháº­p AWS Management Console báº±ng tÃ i khoáº£n quáº£n lÃ½
- Má»Ÿ **AWS Organizations console**

**2. TÃ¬m tÃ i khoáº£n cáº§n chuyá»ƒn**

- Chá»n **AWS accounts**
- TÃ¬m cÃ¡c tÃ i khoáº£n muá»‘n chuyá»ƒn vÃ o mÃ´i trÆ°á»ng sandbox
- **LÆ°u Ã½:** Äá»«ng chá»n tÃ i khoáº£n quáº£n lÃ½ hoáº·c tÃ i khoáº£n hub

![architect](/images/account-onboarding.png "Architect")

**3. Chuyá»ƒn vÃ o Entry OU**

- Chá»n cÃ¡c tÃ i khoáº£n cáº§n chuyá»ƒn
- Menu **Actions**, chá»n **Move** 
- Há»™p thoáº¡i **Move AWS account**, click mÅ©i tÃªn cáº¡nh Innovation Sandbox OU Ä‘á»ƒ má»Ÿ rá»™ng
- Chá»n **Entry OU**
- **Move AWS accounts** Ä‘á»ƒ xÃ¡c nháº­n

#### Káº¿t quáº£

- CÃ¡c tÃ i khoáº£n sandbox Ä‘Ã£ á»Ÿ Ä‘Ãºng vá»‹ trÃ­ trong cáº¥u trÃºc Innovation Sandbox
- CÃ¡c policy kiá»ƒm soÃ¡t sáº½ tá»± Ä‘á»™ng Ã¡p dá»¥ng cho nhá»¯ng tÃ i khoáº£n nÃ y

LÃ m Ä‘Ãºng bÆ°á»›c nÃ y Ä‘áº£m báº£o mÃ´i trÆ°á»ng sandbox hoáº¡t Ä‘á»™ng riÃªng biá»‡t, an toÃ n vÃ  tuÃ¢n thá»§ chÃ­nh sÃ¡ch tá»• chá»©c.
