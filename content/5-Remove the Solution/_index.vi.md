+++
title = "Khởi tạo Auto Scaling Group"
date = 2020
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

### Xóa hết khi không cần nữa

Nếu bạn đã thử xong rồi và muốn dọn dẹp sạch sẽ, làm theo các bước này. Dự kiến mất khoảng 15 phút.

#### 1. Xóa hai stack đầu (ở tài khoản hub)

Đăng nhập tài khoản hub, đảm bảo đúng home Region.

Mở AWS CloudFormation console.

Chọn compute stack (ví dụ: isb-compute), click Delete.

Đợi xóa xong.

Tiếp tục chọn data stack (ví dụ: isb-data), click Delete.

Đợi xóa xong.

#### 2. Xóa cấu hình IAM Identity Center App (ở tài khoản quản lý)

Đăng nhập tài khoản quản lý, đảm bảo đúng home Region.

Mở IAM Identity Center console.

Thanh điều hướng chọn Applications.

Chuyển sang tab Customer managed.

Chọn app Innovation Sandbox on AWS.

Vào Actions > Remove.

Xác nhận bằng cách nhập tên app để hoàn tất.

#### 3. Xóa hai stack còn lại (ở tài khoản quản lý)

Mở AWS CloudFormation console.

Chọn IDC stack (ví dụ: isb-idc), click Delete.

Đợi xóa xong.

Chọn AccountPool stack (ví dụ: isb-accountpool), click Delete.

Đợi xóa xong.

#### 4. Đóng tài khoản AWS Sandbox (tùy chọn)

Lưu ý: Trước khi đóng tài khoản AWS, đọc kỹ hướng dẫn và tác động của việc đóng tài khoản trong tài liệu AWS.

Mở AWS Organizations console ở tài khoản quản lý.

Truy cập organizational unit của Innovation Sandbox on AWS.

Mở từng sub-organizational unit để tìm các tài khoản sandbox.

Với mỗi tài khoản cần đóng:

Chọn tài khoản.

Vào Actions > Close.

Xác nhận bằng cách nhập AWS account ID.

#### Xong!

Thế là đã dọn dẹp xong toàn bộ Innovation Sandbox on AWS khỏi môi trường thử nghiệm rồi.