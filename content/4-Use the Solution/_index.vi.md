+++
title = "Use the Solutio"
date = 2025
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

### S? d?ng h? th?ng

Okay gi? dã setup xong h?t r?i, chúng ta s? th? xài h? th?ng v?i 3 vai trò khác nhau d? xem nó ho?t d?ng th? nào.

#### Ba vai trò chính

- **Administrator** - qu?n tr? viên, quy?n cao nh?t
- **Manager** - qu?n lý, t?o template và approve request  
- **End user** - ngu?i dùng cu?i, request tài kho?n d? xài

D? ki?n m?t kho?ng 30 phút d? th? h?t.

### Vai trò Administrator

#### 1. Ðang nh?p vào h? th?ng

- L?y URL t? CloudFrontDistributionUrl dã luu lúc deploy
- M? URL trên browser
- Ðang nh?p b?ng tài kho?n admin. L?n d?u thì ch?n **Forgot password** d? set m?t kh?u

**Luu ý:** N?u th?y hai tab **Accounts** và **Applications**, ch?n **Applications** d? vào app chính.

![architect](/images/Anha1.png "Architect")

#### 2. Thêm tài kho?n cho thuê

- Thanh di?u hu?ng bên trái, **Administration** > **Accounts**
- Click **Add accounts**
- M?c **Select accounts**, s? th?y danh sách tài kho?n có s?n. N?u không có gì thì check l?i xem sandbox account dã du?c move vào OU **Entry** chua
- Ch?n m?t vài tài kho?n, **Next** r?i **Submit**

![architect](/images/Anha2.png "Architect")

- Quay l?i **Accounts** d? check tr?ng thái

#### 3. Qu?n lý cài d?t

- Thanh di?u hu?ng, **Administration** > **Settings**
- Có ba tab:
  - **General Settings**
  - **Lease Settings** (ngân sách t?i da m?c d?nh $100)
  - **Clean Up Settings**
- Mu?n d?i thì ph?i dùng **AWS AppConfig** (hu?ng d?n ? ph?n sau)

![architect](/images/Anha3.png "Architect")

#### 4. T?o Service Control Policy d? gi?i h?n

Ví d?: T?o SCP d? không cho user t?o EC2 instance lo?i `m5.large`.

**Các bu?c:**

- Ðang nh?p **organization management account**
- M? **AWS Organizations**
- Thanh di?u hu?ng ch?n **Policies**
- **Supported policy types** > **Service control policies**

![architect](/images/Anha4.png "Architect")

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
- Quay l?i **Policies**, ch?n policy v?a t?o
- Tab **Targets**, click **Attach**
- Ch?n OU tên `your-namespace_InnovationSandboxAccountPool`, **Attach policy**

![architect](/images/Anha6.png "Architect")

**K?t qu?:** User trong sandbox account không th? t?o EC2 instance `m5.large` n?a.

### Vai trò Manager

Manager có th? ch?nh s?a cài d?t và t?o template cho user request tài kho?n.

#### Ch?nh s?a cài d?t b?ng AppConfig

**Luu ý quan tr?ng:** Làm ? **tài kho?n hub**, không ph?i tài kho?n qu?n lý. Ðúng **home Region**.

**Các bu?c:**

- Ðang nh?p tài kho?n hub
- Thanh tìm ki?m console, nh?p **AWS AppConfig**
- Thanh di?u hu?ng ch?n **Applications**
- Ch?n app dã t?o
- Ch?n **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-ID**

![architect](/images/Manager1.png "Architect")

- **Hosted configuration versions** > **Create**
- Ð?i **maxBudget** thành **50** (USD)

![architect](/images/Manager2.png "Architect")

- **Create hosted configuration version**
- **Start deployment**
- Ch?n phiên b?n v?a t?o, gi? nguyên setting khác, **Start deployment**

### T?o Lease Template

#### Lease Template là gì

V? co b?n thì dây là b? quy t?c d?nh nghia user có th? xài tài nguyên th? nào:

- **Ngân sách t?i da:** Có th? tiêu bao nhiêu ti?n
- **Th?i gian:** Ðu?c xài bao lâu
- **Approval:** Có c?n manager duy?t không
- **C?nh báo:** Báo khi s?p h?t ti?n ho?c th?i gian

S? t?o hai lo?i:
- **Basic:** $25, t? d?ng approve, cho test nh?
- **Advanced:** $50, c?n approve, cho d? án l?n

### T?o Basic Template

Template này cho nh?ng request không c?n duy?t. $25 t?i da, báo khi tiêu $15, dùng du?c 168 ti?ng (1 tu?n), báo tru?c 24 ti?ng khi h?t h?n.

**Các bu?c:**

- Console Innovation Sandbox, ch?n profile góc trên ph?i > **Sign out**
- Ðang nh?p b?ng tài kho?n manager. L?n d?u dùng **Forgot password**
- N?u có hai tab **Accounts** và **Applications**, ch?n **Applications**
- Thanh di?u hu?ng ch?n **Lease Templates**
- **Add new lease template**
- **Name:** `Basic`
- **Description:** `Basic account lease template`
- B? tick **Approval required** (không c?n manager duy?t)

![architect](/images/Manager3.png "Architect")

- **Next**
- **Maximum budget:** `100`, **Next**
- N?u dã ch?nh AppConfig dúng s? báo l?i ngân sách t?i da $50. Ð?i **Maximum Budget Amount** thành `25`
- **Budget Thresholds** > **Add a threshold**:
  - **amount consumed:** `15`
  - **action:** **Send Alert**. User s? nh?n c?nh báo khi tiêu $15, xóa account khi tiêu h?t $25
- **Next**
- **Maximum Duration** > **Set a maximum duration** > `168`
- **Duration Thresholds** > **Add a threshold**:
  - **remaining hours:** `24`
  - **action:** **Send Alert**. Báo khi còn 24 ti?ng
- **Submit**

![architect](/images/Manager4.png "Architect")

### T?o Advanced Template

Template này c?n manager approve. $50 t?i da, báo ? $40, th?i gian 168 ti?ng, báo tru?c 24 ti?ng.

**Các bu?c:**

- **Lease Templates** > **Add new lease template**
- **Name:** `Advanced`
- **Description:** `Advanced account lease template`
- Gi? nguyên **Approval required** (c?n manager duy?t)
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

### Vai trò End-User

Gi? s? th? làm user thu?ng, request tài kho?n và xài.

#### 1. Request tài kho?n co b?n

Tài kho?n basic s? du?c approve t? d?ng.

**Các bu?c:**

- Giao di?n Innovation Sandbox, ch?n profile góc trên ph?i > **Sign out**
- Ðang nh?p l?i b?ng tài kho?n end user
- N?u có hai tab thì ch?n **Applications**
- **Request a new account**
- Ch?n **Basic lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**

![architect](/images/User1.png "Architect")

- **Comments:** nh?p lý do (ví d?: `Request for a basic account`)
- **Submit**
- Thanh di?u hu?ng bên trái ch?n **Home**. M?c **Account**, click **Refresh** d? c?p nh?t

![architect](/images/User2.png "Architect")

- Khi tài kho?n du?c c?p, m?c **Access** ch?n **Login to account**
- Ch?n role ? **Select a role** d? vào AWS Management Console

![architect](/images/User3.png "Architect")

#### 2. Test gi?i h?n SCP

Check xem SCP có ho?t d?ng không b?ng cách th? t?o EC2 instance b? c?m.

**Các bu?c:**

- AWS Management Console, nh?p **EC2** vào thanh tìm ki?m
- Thanh di?u hu?ng bên trái ch?n **Instances**
- **Launch instances**
- **Name:** `test-scp`. Gi? nguyên AMI m?c d?nh

![architect](/images/User4.png "Architect")

- **Instance type:** ch?n **m5.large** (lo?i này b? SCP ch?n)
- **Key pair:** **Proceed without a key pair**
- Gi? nguyên setting khác, **Launch instance**
- S? th?y báo l?i do SCP không cho t?o **m5.large**

![architect](/images/User5.png "Architect")

#### 3. Request tài kho?n nâng cao

Tài kho?n advanced c?n manager approve.

**Các bu?c:**

- Quay l?i giao di?n Innovation Sandbox, dang nh?p end user
- **Request a new account**
- Ch?n **Advanced lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**
- **Comments:** nh?p lý do (ví d?: `Request for an advanced account`)
- **Submit**

**Quy trình approve:**

- Ðang xu?t, dang nh?p b?ng tài kho?n manager
- Trang ch? s? th?y request ch? duy?t. Thanh di?u hu?ng ch?n **Approvals**
- Ch?n request, **Actions** > **Approve request**

**Qu?n lý lease:**

- Thanh di?u hu?ng ch?n **Leases**
- S? th?y hai lease: basic và advanced
- Có th? ch?n lease dùng **Actions** d? terminate, suspend ho?c c?p nh?t

![architect](/images/User6.png "Architect")

![architect](/images/User7.png "Architect")

#### 4. T?ng k?t

- **Administrator:** Setup h? th?ng, t?o SCP ki?m soát tài nguyên, qu?n lý tài kho?n, tích h?p AppConfig
- **Manager:** C?u hình ngân sách, t?o lease template (Basic, Advanced), thi?t l?p quy trình approve, giám sát s? d?ng
- **End user:** Request và truy c?p tài kho?n AWS, test SCP, dùng các lo?i tài kho?n v?i quy trình approve phù h?p

Th? là xong, gi? b?n dã bi?t cách s? d?ng Innovation Sandbox v?i c? 3 vai trò r?i!



