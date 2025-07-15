+++
title = "Use the Solutio"
date = 2025
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

### S? d?ng h? th?ng

Okay gi? d� setup xong h?t r?i, ch�ng ta s? th? x�i h? th?ng v?i 3 vai tr� kh�c nhau d? xem n� ho?t d?ng th? n�o.

#### Ba vai tr� ch�nh

- **Administrator** - qu?n tr? vi�n, quy?n cao nh?t
- **Manager** - qu?n l�, t?o template v� approve request  
- **End user** - ngu?i d�ng cu?i, request t�i kho?n d? x�i

D? ki?n m?t kho?ng 30 ph�t d? th? h?t.

### Vai tr� Administrator

#### 1. �ang nh?p v�o h? th?ng

- L?y URL t? CloudFrontDistributionUrl d� luu l�c deploy
- M? URL tr�n browser
- �ang nh?p b?ng t�i kho?n admin. L?n d?u th� ch?n **Forgot password** d? set m?t kh?u

**Luu �:** N?u th?y hai tab **Accounts** v� **Applications**, ch?n **Applications** d? v�o app ch�nh.

![architect](/images/Anha1.png "Architect")

#### 2. Th�m t�i kho?n cho thu�

- Thanh di?u hu?ng b�n tr�i, **Administration** > **Accounts**
- Click **Add accounts**
- M?c **Select accounts**, s? th?y danh s�ch t�i kho?n c� s?n. N?u kh�ng c� g� th� check l?i xem sandbox account d� du?c move v�o OU **Entry** chua
- Ch?n m?t v�i t�i kho?n, **Next** r?i **Submit**

![architect](/images/Anha2.png "Architect")

- Quay l?i **Accounts** d? check tr?ng th�i

#### 3. Qu?n l� c�i d?t

- Thanh di?u hu?ng, **Administration** > **Settings**
- C� ba tab:
  - **General Settings**
  - **Lease Settings** (ng�n s�ch t?i da m?c d?nh $100)
  - **Clean Up Settings**
- Mu?n d?i th� ph?i d�ng **AWS AppConfig** (hu?ng d?n ? ph?n sau)

![architect](/images/Anha3.png "Architect")

#### 4. T?o Service Control Policy d? gi?i h?n

V� d?: T?o SCP d? kh�ng cho user t?o EC2 instance lo?i `m5.large`.

**C�c bu?c:**

- �ang nh?p **organization management account**
- M? **AWS Organizations**
- Thanh di?u hu?ng ch?n **Policies**
- **Supported policy types** > **Service control policies**

![architect](/images/Anha4.png "Architect")

- Click **Create policy**:
  - **Policy name:** RestrictEC2Instances
  - **Policy description:** This SCP restricts launching certain EC2 instance types.
- D�n JSON n�y v�o policy editor:

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
- Ch?n OU t�n `your-namespace_InnovationSandboxAccountPool`, **Attach policy**

![architect](/images/Anha6.png "Architect")

**K?t qu?:** User trong sandbox account kh�ng th? t?o EC2 instance `m5.large` n?a.

### Vai tr� Manager

Manager c� th? ch?nh s?a c�i d?t v� t?o template cho user request t�i kho?n.

#### Ch?nh s?a c�i d?t b?ng AppConfig

**Luu � quan tr?ng:** L�m ? **t�i kho?n hub**, kh�ng ph?i t�i kho?n qu?n l�. ��ng **home Region**.

**C�c bu?c:**

- �ang nh?p t�i kho?n hub
- Thanh t�m ki?m console, nh?p **AWS AppConfig**
- Thanh di?u hu?ng ch?n **Applications**
- Ch?n app d� t?o
- Ch?n **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-ID**

![architect](/images/Manager1.png "Architect")

- **Hosted configuration versions** > **Create**
- �?i **maxBudget** th�nh **50** (USD)

![architect](/images/Manager2.png "Architect")

- **Create hosted configuration version**
- **Start deployment**
- Ch?n phi�n b?n v?a t?o, gi? nguy�n setting kh�c, **Start deployment**

### T?o Lease Template

#### Lease Template l� g�

V? co b?n th� d�y l� b? quy t?c d?nh nghia user c� th? x�i t�i nguy�n th? n�o:

- **Ng�n s�ch t?i da:** C� th? ti�u bao nhi�u ti?n
- **Th?i gian:** �u?c x�i bao l�u
- **Approval:** C� c?n manager duy?t kh�ng
- **C?nh b�o:** B�o khi s?p h?t ti?n ho?c th?i gian

S? t?o hai lo?i:
- **Basic:** $25, t? d?ng approve, cho test nh?
- **Advanced:** $50, c?n approve, cho d? �n l?n

### T?o Basic Template

Template n�y cho nh?ng request kh�ng c?n duy?t. $25 t?i da, b�o khi ti�u $15, d�ng du?c 168 ti?ng (1 tu?n), b�o tru?c 24 ti?ng khi h?t h?n.

**C�c bu?c:**

- Console Innovation Sandbox, ch?n profile g�c tr�n ph?i > **Sign out**
- �ang nh?p b?ng t�i kho?n manager. L?n d?u d�ng **Forgot password**
- N?u c� hai tab **Accounts** v� **Applications**, ch?n **Applications**
- Thanh di?u hu?ng ch?n **Lease Templates**
- **Add new lease template**
- **Name:** `Basic`
- **Description:** `Basic account lease template`
- B? tick **Approval required** (kh�ng c?n manager duy?t)

![architect](/images/Manager3.png "Architect")

- **Next**
- **Maximum budget:** `100`, **Next**
- N?u d� ch?nh AppConfig d�ng s? b�o l?i ng�n s�ch t?i da $50. �?i **Maximum Budget Amount** th�nh `25`
- **Budget Thresholds** > **Add a threshold**:
  - **amount consumed:** `15`
  - **action:** **Send Alert**. User s? nh?n c?nh b�o khi ti�u $15, x�a account khi ti�u h?t $25
- **Next**
- **Maximum Duration** > **Set a maximum duration** > `168`
- **Duration Thresholds** > **Add a threshold**:
  - **remaining hours:** `24`
  - **action:** **Send Alert**. B�o khi c�n 24 ti?ng
- **Submit**

![architect](/images/Manager4.png "Architect")

### T?o Advanced Template

Template n�y c?n manager approve. $50 t?i da, b�o ? $40, th?i gian 168 ti?ng, b�o tru?c 24 ti?ng.

**C�c bu?c:**

- **Lease Templates** > **Add new lease template**
- **Name:** `Advanced`
- **Description:** `Advanced account lease template`
- Gi? nguy�n **Approval required** (c?n manager duy?t)
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

### Vai tr� End-User

Gi? s? th? l�m user thu?ng, request t�i kho?n v� x�i.

#### 1. Request t�i kho?n co b?n

T�i kho?n basic s? du?c approve t? d?ng.

**C�c bu?c:**

- Giao di?n Innovation Sandbox, ch?n profile g�c tr�n ph?i > **Sign out**
- �ang nh?p l?i b?ng t�i kho?n end user
- N?u c� hai tab th� ch?n **Applications**
- **Request a new account**
- Ch?n **Basic lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**

![architect](/images/User1.png "Architect")

- **Comments:** nh?p l� do (v� d?: `Request for a basic account`)
- **Submit**
- Thanh di?u hu?ng b�n tr�i ch?n **Home**. M?c **Account**, click **Refresh** d? c?p nh?t

![architect](/images/User2.png "Architect")

- Khi t�i kho?n du?c c?p, m?c **Access** ch?n **Login to account**
- Ch?n role ? **Select a role** d? v�o AWS Management Console

![architect](/images/User3.png "Architect")

#### 2. Test gi?i h?n SCP

Check xem SCP c� ho?t d?ng kh�ng b?ng c�ch th? t?o EC2 instance b? c?m.

**C�c bu?c:**

- AWS Management Console, nh?p **EC2** v�o thanh t�m ki?m
- Thanh di?u hu?ng b�n tr�i ch?n **Instances**
- **Launch instances**
- **Name:** `test-scp`. Gi? nguy�n AMI m?c d?nh

![architect](/images/User4.png "Architect")

- **Instance type:** ch?n **m5.large** (lo?i n�y b? SCP ch?n)
- **Key pair:** **Proceed without a key pair**
- Gi? nguy�n setting kh�c, **Launch instance**
- S? th?y b�o l?i do SCP kh�ng cho t?o **m5.large**

![architect](/images/User5.png "Architect")

#### 3. Request t�i kho?n n�ng cao

T�i kho?n advanced c?n manager approve.

**C�c bu?c:**

- Quay l?i giao di?n Innovation Sandbox, dang nh?p end user
- **Request a new account**
- Ch?n **Advanced lease template**, **Next**
- Tick **I accept the above terms of service**, **Next**
- **Comments:** nh?p l� do (v� d?: `Request for an advanced account`)
- **Submit**

**Quy tr�nh approve:**

- �ang xu?t, dang nh?p b?ng t�i kho?n manager
- Trang ch? s? th?y request ch? duy?t. Thanh di?u hu?ng ch?n **Approvals**
- Ch?n request, **Actions** > **Approve request**

**Qu?n l� lease:**

- Thanh di?u hu?ng ch?n **Leases**
- S? th?y hai lease: basic v� advanced
- C� th? ch?n lease d�ng **Actions** d? terminate, suspend ho?c c?p nh?t

![architect](/images/User6.png "Architect")

![architect](/images/User7.png "Architect")

#### 4. T?ng k?t

- **Administrator:** Setup h? th?ng, t?o SCP ki?m so�t t�i nguy�n, qu?n l� t�i kho?n, t�ch h?p AppConfig
- **Manager:** C?u h�nh ng�n s�ch, t?o lease template (Basic, Advanced), thi?t l?p quy tr�nh approve, gi�m s�t s? d?ng
- **End user:** Request v� truy c?p t�i kho?n AWS, test SCP, d�ng c�c lo?i t�i kho?n v?i quy tr�nh approve ph� h?p

Th? l� xong, gi? b?n d� bi?t c�ch s? d?ng Innovation Sandbox v?i c? 3 vai tr� r?i!



