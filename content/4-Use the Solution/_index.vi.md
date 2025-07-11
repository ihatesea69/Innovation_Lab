+++
title = "Khá»Ÿi táº¡o Load Balancer"
date = 2020
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

### Sá»­ dá»¥ng há»‡ thá»‘ng

Okay giá» Ä‘Ã£ setup xong háº¿t rá»“i, chÃºng ta sáº½ thá»­ xÃ i há»‡ thá»‘ng vá»›i 3 vai trÃ² khÃ¡c nhau Ä‘á»ƒ xem nÃ³ hoáº¡t Ä‘á»™ng tháº¿ nÃ o.

#### Ba vai trÃ² chÃ­nh

- **Administrator** - quáº£n trá»‹ viÃªn, quyá»n cao nháº¥t
- **Manager** - quáº£n lÃ½, táº¡o template vÃ  approve request  
- **End user** - ngÆ°á»i dÃ¹ng cuá»‘i, request tÃ i khoáº£n Ä‘á»ƒ xÃ i

Dá»± kiáº¿n máº¥t khoáº£ng 30 phÃºt Ä‘á»ƒ thá»­ háº¿t.

### Vai trÃ² Administrator

#### 1. ÄÄƒng nháº­p vÃ o há»‡ thá»‘ng

- Láº¥y URL tá»« CloudFrontDistributionUrl Ä‘Ã£ lÆ°u lÃºc deploy
- Má»Ÿ URL trÃªn browser
- ÄÄƒng nháº­p báº±ng tÃ i khoáº£n admin. Láº§n Ä‘áº§u thÃ¬ chá»n **Forgot password** Ä‘á»ƒ set máº­t kháº©u

**LÆ°u Ã½:** Náº¿u tháº¥y hai tab **Accounts** vÃ  **Applications**, chá»n **Applications** Ä‘á»ƒ vÃ o app chÃ­nh.

![architect](/images/Anha1.png "Architect")

#### 2. ThÃªm tÃ i khoáº£n cho thuÃª

- Thanh Ä‘iá»u hÆ°á»›ng bÃªn trÃ¡i, **Administration** > **Accounts**
- Click **Add accounts**
- Má»¥c **Select accounts**, sáº½ tháº¥y danh sÃ¡ch tÃ i khoáº£n cÃ³ sáºµn. Náº¿u khÃ´ng cÃ³ gÃ¬ thÃ¬ check láº¡i xem sandbox account Ä‘Ã£ Ä‘Æ°á»£c move vÃ o OU **Entry** chÆ°a
- Chá»n má»™t vÃ i tÃ i khoáº£n, **Next** rá»“i **Submit**

![architect](/images/Anha2.png "Architect")

- Quay láº¡i **Accounts** Ä‘á»ƒ check tráº¡ng thÃ¡i

#### 3. Quáº£n lÃ½ cÃ i Ä‘áº·t

- Thanh Ä‘iá»u hÆ°á»›ng, **Administration** > **Settings**
- CÃ³ ba tab:
  - **General Settings**
  - **Lease Settings** (ngÃ¢n sÃ¡ch tá»‘i Ä‘a máº·c Ä‘á»‹nh $100)
  - **Clean Up Settings**
- Muá»‘n Ä‘á»•i thÃ¬ pháº£i dÃ¹ng **AWS AppConfig** (hÆ°á»›ng dáº«n á»Ÿ pháº§n sau)

![architect](/images/Anha3.png "Architect")

#### 4. Táº¡o Service Control Policy Ä‘á»ƒ giá»›i háº¡n

VÃ­ dá»¥: Táº¡o SCP Ä‘á»ƒ khÃ´ng cho user táº¡o EC2 instance loáº¡i `m5.large`.

**CÃ¡c bÆ°á»›c:**

- ÄÄƒng nháº­p **organization management account**
- Má»Ÿ **AWS Organizations**
- Thanh Ä‘iá»u hÆ°á»›ng chá»n **Policies**
- **Supported policy types** > **Service control policies**

![architect](/images/Anha4.png "Architect")

- Click **Create policy**:
  - **Policy name:** RestrictEC2Instances
  - **Policy description:** This SCP restricts launching certain EC2 instance types.
- DÃ¡n JSON nÃ y vÃ o policy editor:

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
- Quay láº¡i **Policies**, chá»n policy vá»«a táº¡o
- Tab **Targets**, click **Attach**
- Chá»n OU tÃªn `your-namespace_InnovationSandboxAccountPool`, **Attach policy**

![architect](/images/Anha6.png "Architect")

**Káº¿t quáº£:** User trong sandbox account khÃ´ng thá»ƒ táº¡o EC2 instance `m5.large` ná»¯a.

### Vai trÃ² Manager

Manager cÃ³ thá»ƒ chá»‰nh sá»­a cÃ i Ä‘áº·t vÃ  táº¡o template cho user request tÃ i khoáº£n.

#### Chá»‰nh sá»­a cÃ i Ä‘áº·t báº±ng AppConfig

**LÆ°u Ã½ quan trá»ng:** LÃ m á»Ÿ **tÃ i khoáº£n hub**, khÃ´ng pháº£i tÃ i khoáº£n quáº£n lÃ½. ÄÃºng **home Region**.

**CÃ¡c bÆ°á»›c:**

- ÄÄƒng nháº­p tÃ i khoáº£n hub
- Thanh tÃ¬m kiáº¿m console, nháº­p **AWS AppConfig**
- Thanh Ä‘iá»u hÆ°á»›ng chá»n **Applications**
- Chá»n app Ä‘Ã£ táº¡o
- Chá»n **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-ID**

![architect](/images/Manager1.png "Architect")

- **Hosted configuration versions** > **Create**
- Äá»•i **maxBudget** thÃ nh **50** (USD)

![architect](/images/Manager2.png "Architect")

- **Create hosted configuration version**
- **Start deployment**
- Chá»n phiÃªn báº£n vá»«a táº¡o, giá»¯ nguyÃªn setting khÃ¡c, **Start deployment**

### Táº¡o Lease Template

#### Lease Template lÃ  gÃ¬

Vá» cÆ¡ báº£n thÃ¬ Ä‘Ã¢y lÃ  bá»™ quy táº¯c Ä‘á»‹nh nghÄ©a user cÃ³ thá»ƒ xÃ i tÃ i nguyÃªn tháº¿ nÃ o:

- **NgÃ¢n sÃ¡ch tá»‘i Ä‘a:** CÃ³ thá»ƒ tiÃªu bao nhiÃªu tiá»n
- **Thá»i gian:** ÄÆ°á»£c xÃ i bao lÃ¢u
- **Approval:** CÃ³ cáº§n manager duyá»‡t khÃ´ng
- **Cáº£nh bÃ¡o:** BÃ¡o khi sáº¯p háº¿t tiá»n hoáº·c thá»i gian

Sáº½ táº¡o hai loáº¡i:
- **Basic:** $25, tá»± Ä‘á»™ng approve, cho test nhá»
- **Advanced:** $50, cáº§n approve, cho dá»± Ã¡n lá»›n

### Táº¡o Basic Template

Template nÃ y cho nhá»¯ng request khÃ´ng cáº§n duyá»‡t. $25 tá»‘i Ä‘a, bÃ¡o khi tiÃªu $15, dÃ¹ng Ä‘Æ°á»£c 168 tiáº¿ng (1 tuáº§n), bÃ¡o trÆ°á»›c 24 tiáº¿ng khi háº¿t háº¡n.

**CÃ¡c bÆ°á»›c:**

- Console Innovation Sandbox, chá»n profile gÃ³c trÃªn pháº£i > **Sign out**
- ÄÄƒng nháº­p báº±ng tÃ i khoáº£n manager. Láº§n Ä‘áº§u dÃ¹ng **Forgot password**
- Náº¿u cÃ³ hai tab **Accounts** vÃ  **Applications**, chá»n **Applications**
- Thanh Ä‘iá»u hÆ°á»›ng chá»n **Lease Templates**
- **Add new lease template**
- **Name:** `Basic`
- **Description:** `Basic account lease template`
- Bá» tick **Approval required** (khÃ´ng cáº§n manager duyá»‡t)

![architect](/images/Manager3.png "Architect")

- **Next**
- **Maximum budget:** `100`, **Next**
- Náº¿u Ä‘Ã£ chá»‰nh AppConfig Ä‘Ãºng sáº½ bÃ¡o lá»—i ngÃ¢n sÃ¡ch tá»‘i Ä‘a $50. Äá»•i **Maximum Budget Amount** thÃ nh `25`
- **Budget Thresholds** > **Add a threshold**:
  - **amount consumed:** `15`
  - **action:** **Send Alert**. User sáº½ nháº­n cáº£nh bÃ¡o khi tiÃªu $15, xÃ³a account khi tiÃªu háº¿t $25
- **Next**
- **Maximum Duration** > **Set a maximum duration** > `168`
- **Duration Thresholds** > **Add a threshold**:
  - **remaining hours:** `24`
  - **action:** **Send Alert**. BÃ¡o khi cÃ²n 24 tiáº¿ng
- **Submit**

![architect](/images/Manager4.png "Architect")

### Táº¡o Advanced Template

Template nÃ y cáº§n manager approve. $50 tá»‘i Ä‘a, bÃ¡o á»Ÿ $40, thá»i gian 168 tiáº¿ng, bÃ¡o trÆ°á»›c 24 tiáº¿ng.

**CÃ¡c bÆ°á»›c:**

- **Lease Templates** > **Add new lease template**
- **Name:** `Advanced`
- **Description:** `Advanced account lease template`
- Giá»¯ nguyÃªn **Approval required** (cáº§n manager duyá»‡t)
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

![architect](/images/Manager5.png "Architect")

### Vai trÃ² End-User

Giá» sáº½ thá»­ lÃ m user thÆ°á»ng, request tÃ i khoáº£n vÃ  xÃ i.

#### 1. Request tÃ i khoáº£n cÆ¡ báº£n

TÃ i khoáº£n basic sáº½ Ä‘Æ°á»£c approve tá»± Ä‘á»™ng.

**CÃ¡c bÆ°á»›c:**

- Giao diá»‡n Innovation Sandbox, chá»n profile gÃ³c trÃªn pháº£i > **Sign out**
- ÄÄƒng nháº­p láº¡i báº±ng tÃ i khoáº£n end user
- Náº¿u cÃ³ hai tab thÃ¬ chá»n **Applications**
- **Request a new account**
- Chá»n **Basic lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**

![architect](/images/User1.png "Architect")

- **Comments:** nháº­p lÃ½ do (vÃ­ dá»¥: `Request for a basic account`)
- **Submit**
- Thanh Ä‘iá»u hÆ°á»›ng bÃªn trÃ¡i chá»n **Home**. Má»¥c **Account**, click **Refresh** Ä‘á»ƒ cáº­p nháº­t

![architect](/images/User2.png "Architect")

- Khi tÃ i khoáº£n Ä‘Æ°á»£c cáº¥p, má»¥c **Access** chá»n **Login to account**
- Chá»n role á»Ÿ **Select a role** Ä‘á»ƒ vÃ o AWS Management Console

![architect](/images/User3.png "Architect")

#### 2. Test giá»›i háº¡n SCP

Check xem SCP cÃ³ hoáº¡t Ä‘á»™ng khÃ´ng báº±ng cÃ¡ch thá»­ táº¡o EC2 instance bá»‹ cáº¥m.

**CÃ¡c bÆ°á»›c:**

- AWS Management Console, nháº­p **EC2** vÃ o thanh tÃ¬m kiáº¿m
- Thanh Ä‘iá»u hÆ°á»›ng bÃªn trÃ¡i chá»n **Instances**
- **Launch instances**
- **Name:** `test-scp`. Giá»¯ nguyÃªn AMI máº·c Ä‘á»‹nh

![architect](/images/User4.png "Architect")

- **Instance type:** chá»n **m5.large** (loáº¡i nÃ y bá»‹ SCP cháº·n)
- **Key pair:** **Proceed without a key pair**
- Giá»¯ nguyÃªn setting khÃ¡c, **Launch instance**
- Sáº½ tháº¥y bÃ¡o lá»—i do SCP khÃ´ng cho táº¡o **m5.large**

![architect](/images/User5.png "Architect")

#### 3. Request tÃ i khoáº£n nÃ¢ng cao

TÃ i khoáº£n advanced cáº§n manager approve.

**CÃ¡c bÆ°á»›c:**

- Quay láº¡i giao diá»‡n Innovation Sandbox, Ä‘Äƒng nháº­p end user
- **Request a new account**
- Chá»n **Advanced lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**
- **Comments:** nháº­p lÃ½ do (vÃ­ dá»¥: `Request for an advanced account`)
- **Submit**

**Quy trÃ¬nh approve:**

- ÄÄƒng xuáº¥t, Ä‘Äƒng nháº­p báº±ng tÃ i khoáº£n manager
- Trang chá»§ sáº½ tháº¥y request chá» duyá»‡t. Thanh Ä‘iá»u hÆ°á»›ng chá»n **Approvals**
- Chá»n request, **Actions** > **Approve request**

**Quáº£n lÃ½ lease:**

- Thanh Ä‘iá»u hÆ°á»›ng chá»n **Leases**
- Sáº½ tháº¥y hai lease: basic vÃ  advanced
- CÃ³ thá»ƒ chá»n lease dÃ¹ng **Actions** Ä‘á»ƒ terminate, suspend hoáº·c cáº­p nháº­t

![architect](/images/User6.png "Architect")

![architect](/images/User7.png "Architect")

#### 4. Tá»•ng káº¿t

- **Administrator:** Setup há»‡ thá»‘ng, táº¡o SCP kiá»ƒm soÃ¡t tÃ i nguyÃªn, quáº£n lÃ½ tÃ i khoáº£n, tÃ­ch há»£p AppConfig
- **Manager:** Cáº¥u hÃ¬nh ngÃ¢n sÃ¡ch, táº¡o lease template (Basic, Advanced), thiáº¿t láº­p quy trÃ¬nh approve, giÃ¡m sÃ¡t sá»­ dá»¥ng
- **End user:** Request vÃ  truy cáº­p tÃ i khoáº£n AWS, test SCP, dÃ¹ng cÃ¡c loáº¡i tÃ i khoáº£n vá»›i quy trÃ¬nh approve phÃ¹ há»£p

Tháº¿ lÃ  xong, giá» báº¡n Ä‘Ã£ biáº¿t cÃ¡ch sá»­ dá»¥ng Innovation Sandbox vá»›i cáº£ 3 vai trÃ² rá»“i!



