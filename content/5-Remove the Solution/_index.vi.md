+++
title = "5-Remove the Solution"
date = 2025
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

Hướng dẫn Gỡ Bỏ Hoàn Toàn Innovation Sandbox on AWS
Nếu bạn đã triển khai Innovation Sandbox on AWS trong môi trường thử nghiệm và cần xóa bỏ hoàn toàn, hãy thực hiện theo các bước dưới đây. Thời gian dự kiến: khoảng 15 phút.

1. Xóa Hai CloudFormation Stack Đầu Tiên (Trên Hub Account)
Đăng nhập vào hub account và đảm bảo đang ở đúng home Region.

Mở AWS CloudFormation console.

Chọn stack compute (ví dụ: isb-compute), nhấn Delete.

Chờ quá trình xóa hoàn tất.

Tiếp tục chọn stack data (ví dụ: isb-data), nhấn Delete.

Chờ quá trình xóa hoàn tất.

2. Xóa Cấu Hình Ứng Dụng IAM Identity Center (Trên Organization Management Account)
Đăng nhập vào organization management account và đảm bảo đang ở đúng home Region.

Mở IAM Identity Center console.

Trong thanh điều hướng, chọn Applications.

Chuyển sang tab Customer managed.

Chọn ứng dụng Innovation Sandbox on AWS.

Vào Actions > Remove.

Xác nhận bằng cách nhập tên ứng dụng để hoàn tất việc xóa.

3. Xóa Các CloudFormation Stack Còn Lại (Trên Organization Management Account)
Mở AWS CloudFormation console.

Chọn stack IDC (ví dụ: isb-idc), nhấn Delete.

Chờ quá trình xóa hoàn tất.

Chọn stack AccountPool (ví dụ: isb-accountpool), nhấn Delete.

Chờ quá trình xóa hoàn tất.

4. Đóng Các Tài Khoản Sandbox AWS (Tùy chọn)
Lưu ý: Trước khi đóng tài khoản AWS, hãy đọc kỹ quy trình, điều kiện tiên quyết và ảnh hưởng của việc đóng tài khoản trong tài liệu hướng dẫn quản lý tài khoản AWS.

Mở AWS Organizations console trong organization management account.

Truy cập organizational unit dành cho Innovation Sandbox on AWS.

Mở từng sub-organizational unit để tìm các tài khoản sandbox.

Với mỗi tài khoản cần đóng:

Chọn tài khoản.

Vào Actions > Close.

Xác nhận bằng cách nhập AWS account ID.

Hoàn tất!
Bạn đã hoàn thành việc gỡ bỏ toàn bộ giải pháp Innovation Sandbox on AWS khỏi môi trường thử nghiệm.