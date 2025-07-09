+++
title = "Kiểm tra Kết quả"
date = 2020
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

Trong bài thực hành này, chúng ta sẽ kiếm tra truy cập tới ShareNote và tiến hành tăng số lượng yêu cầu truy cập đến ứng dụng thông qua việc mở đồng loạt nhiều kết nối sử dụng công cụ **Webserver Stress Tool 8**. Bạn hãy nhấn vào [**link**](https://www.paessler.com/tools/webstress) này để tải về.

Bạn cũng có thể tải về trực tiếp tại link dưới:

{{%attachments style="orange" title="Webserver Stress Tool 8" pattern=".*(zip)"/%}}

#### Kiểm tra khả năng tự mở rộng của ứng dụng ShareNote được triển khai

1. Giải nén file zip và cài đặt **Webserver Stress Tool 8** với tùy chọn mặc định.
2. Khởi dộng **Webserver Stress Tool 8** để tiến hành tạo số lượng truy cập lớn đến Endpoint của Load Balancer.
3. Nhấn vào tab **Test Type** và nhập thông số như hình dưới đây:
  + **Run Unit** : 100000
  + **Number of Users** : 101
  + **Click Delay** : 1 

![Testing Result](../../../images/6/6_TestType.png?width=90pc)

4. Nhấn vào tab **URLs**, copy DNSName của ứng dụng ShareNote vào ô URL ( DNSName khi bạn tạo Load Balancer ở bước 4.Khởi tạo Load Balancer), và nhấn **Start Test**

![Testing Result](../../../images/6/6_HTTP.png?width=90pc)

5. Sau một khoảng thời gian, kiểm tra phản hồi của Auto Scaling Group. Ta thấy số lượng instance được tăng lên số lượng tối đa mà chúng ta thiết lập là 3.

![Testing Result](../../../images/asg/043.png?width=90pc)

6. Kiểm tra truy cập vào ứng dụng từ trình duyệt không bị gián đoạn.

![Testing Result](../../../images/6/6.png?width=90pc)

Chúc mừng bạn vừa hoàn thành bài thực hành Triển khai ứng dụng ShareNote với Auto Scaling Group và Elastic Load Balancer.
