# Jenkins-Zero-To-Hero
Bạn có mong muốn tìm hiểu Jenkins ngay từ Zero (cài đặt) đến Hero (Xây dựng đường ống từ đầu đến cuối) không? .

## Cài đặt trên Phiên bản EC2


![Screenshot 2023-07-12 at 5 26 00 PM](https://i.ytimg.com/vi/6YZvp2GwT0A/maxresdefault.jpg)

Cài đặt Jenkins, định cấu hình Docker làm tác nhân, thiết lập cicd, triển khai ứng dụng cho k8s, v.v.

## Phiên bản AWS EC2

- Go to AWS Console
- Instances(running)
- Launch instances

<img width="994" alt="Screenshot 2023-07-12 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">
### Cài đặt Jenkins.

Điều kiện tiên quyết:
 - Java (JDK)

### Chạy các lệnh bên dưới để cài đặt Java và Jenkins
Cài đặt Java

```
sudo apt update
sudo apt install openjdk-11-jre
```
Xác minh Java đã được cài đặt

```
java -version
```

Bây giờ, bạn có thể tiến hành cài đặt Jenkins
```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Lưu ý: **Theo mặc định, thế giới bên ngoài sẽ không thể truy cập Jenkins do AWS hạn chế lưu lượng truy cập vào. Mở cổng 8080 trong quy tắc lưu lượng truy cập trong nước như hiển thị bên dưới.

- EC2 > Instances >Bấm vào <Instance-ID>
- Trong các tab dưới cùng->Bấm vào Security
- Security groups
-Thêm quy tắc lưu lượng truy cập trong nước như trong hình ảnh (bạn cũng có thể cho phép TCP 8080, trong trường hợp của tôi, tôi đã cho phép `All traffic`).

<img width="1187" alt="Screenshot 2023-07-12 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">


### Đăng nhập vào Jenkins bằng URL bên dưới:

http://<ec2-instance-public-ip-address>:8080    [Bạn có thể lấy ec2-instance-public-ip-address từ trang bảng điều khiển AWS EC2 của mình]

Lưu ý: Nếu bạn không muốn cho phép`All Traffic` đến phiên bản EC2 của bạn
      1. Xóa quy tắc lưu lượng truy cập đến cho phiên bản của bạn
      2. Chỉnh sửa quy tắc lưu lượng truy cập vào để chỉ cho phép cổng TCP tùy chỉnh `8080`
  
Sau khi bạn đăng nhập vào Jenkins, 
     -Chạy lệnh để sao chép Mật khẩu quản trị viên Jenkins- `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
     -Nhập mật khẩu Administrator
      
<img width="1291" alt="Screenshot 2023-07-12 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Nhấp vào Cài đặt plugin được đề xuất

<img width="1291" alt="Screenshot 2023-07-12 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Đợi Jenkins cài đặt các plugin được đề xuất
<img width="1291" alt="Screenshot 2023-07-12 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Tạo Người dùng quản trị đầu tiên hoặc Bỏ qua bước [Nếu bạn cũng muốn sử dụng phiên bản Jenkins này cho các trường hợp sử dụng trong tương lai, tốt hơn là tạo người dùng Administrator]

<img width="990" alt="Screenshot 2023-07-12 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Cài đặt Jenkins thành công. Bây giờ bạn có thể bắt đầu sử dụng Jenkins

<img width="990" alt="Screenshot 2023-07-12 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

## Cài đặt plugin Docker Pipeline trong Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   
<img width="1392" alt="Screenshot 2023-07-12 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Đợi Jenkins được khởi động lại.

## Cấu hình Docker Slave

Chạy lệnh dưới đây để cài đặt Docker
```
sudo apt update
sudo apt install docker.io
```
 
### Cấp quyền cho người dùng Jenkins và người dùng Ubuntu cho docker daemon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Khi bạn đã hoàn thành các bước trên, tốt hơn hết là khởi động lại Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

Cấu hình tác nhân docker hiện đã thành công.




