---
title: Connect with server using .pem file - linux
date: 2019-07-20 13:21:20
tags:
  - linux
---
Bạn có nghĩ rằng việc sử dụng password để connect tới server thì tiện hơn không?
Mình nghĩ không!!!
Câu chuyện của mình như này: Mình mua vps bên `OVH`, mua xong, qua vài bước chọn OS, ..blah bleee, install xong thì mình được 1 con centos 7.
OVH thì thật đáng yêu, cấp cho mình nguyên 1 cái password toàn kí tự random nên mình có mà nhớ được. Vả lại mình cũng không muốn nhớ nó, bởi vì mình có tính hay vọc vạch nên nó chết liên tục, mà chết liên tục thì mình lại phải install từ đầu, và đó cũng là lý do mà mình không muốn nhớ nó.
Mình note nó vào `Notes` của mac. nhưng cái note đó thì mình note cả đống thứ, và nó cứ bị stack, cái mới sẽ nằm trên và cuối cùng thì nó nằm dưới dưới của cái list note. Mình khá ngán việc cứ phải scroll xuống tìm.
Thấy anh lead có trò dùng file `.pem` để ssh vô server, nên nay mình quyết định tìm hiểu vô nó.
Ngồi mò mò 15p thì mình thấy nó cũng không khó.
Thôi vào luôn vấn đề chính. Để cho các bạn dễ hiểu hơn thì mình sẽ nói sơ qua về quy trình thao tác của nó.
Bản chất của nó là nó sẽ generate ra 1 file `Key Pair`, file này ở dưới máy tính của mình nhé. Sau đó mình sẽ dùng file `Key Pair` này upload lên server.
Sau khi upload xong thì coi như bạn đã hoàn thành được nó rồi.

Và đây là chi tiết thực hiện:

#### 1. Generate Key Pair mà không cần password.
Bạn mở terminal lên rồi paste đoạn mã bên dưới vô.
```sh
ssh-keygen -t rsa -b 2048 -v
```
Sau khi paste vô và ấn enter thì nó sẽ hỏi vài câu hỏi như tên file, password. bạn cứ ấn enter thôi nhé cho đỡ mất công.
Và sau khi nó generate xong nó sẽ cho bạn 2 file là `id_rsa` và `id_rsa.pub` và đường dẫn chứa file ở trong terminal.
Bước 1 đã xong, giờ bước tiếp theo bạn cd vô đường dẫn nó cho mà chứa 2 file id_rsa.
#### 2. Upload file generated certificate tới server
bạn dùng lệnh để upload file `id_rsa.pub` lên server bằng command:
```sh
ssh-copy-id -i ./id_rsa.pub root@12.34.56.78
```

* Bạn lưu ý rằng `./id_rsa.pub` là đường dẫn chưa file `id_rsa.pub` mà bước 1 bạn đã generate nó ra, `root` là tên user và `12.34.56.78` là ip server của bạn.

Sau khi upload xong thì nó sẽ xuất hiện ở đường dẫn này trên server:
```sh
~/.ssh/authorized_keys
```
#### 3. Kiểm tra kết nối có oke hay không?
bạn dùng lệnh:
```sh
sudo ssh -i ./id_rsa.pem root@12.34.56.78
```

Còn nếu file .pem này được sinh ra từ ngay trên máy bạn thì lệnh connect kia không nhất thiết phải cho đường dẫn file pem nữa, để cho ngắn gọn và nó sẽ như này:
```sh
sudo ssh -i root@12.34.56.78
```

File .pem này được sinh ra để giải quyết việc không cần phải dùng password để access vô server.
vì đó bạn có thể share file .pem này cho người khác để người ta login vô server của bạn.

#### 4. Disable PasswordAuthentication in sshd_config
Và nếu bạn muốn disable login với password ( ví dụ như trường hợp tránh scan port ) thì bạn có để làm việc này:
Bạn login vô server trước, sau đó:
```sh
nano /etc/ssh/sshd_config
```

Đổi `PasswordAuthentication yes` thành `PasswordAuthentication no`
Nó thường ở dòng 79 trong file sshd_config
Sau đó chạy lại lệnh:
```sh
systemctl restart sshd
```

Và nếu bạn đăng nhập bằng password ở 1 máy khác, nó sẽ báo:
`Permission denied (publickey,gssapi-keyex,gssapi-with-mic).`

Còn nếu banh muốn đổi lại cho phép đăng nhập = password thì bạn cần phải đăng nhập = file pem trước rồi vô đổi `PasswordAuthentication no` thành `PasswordAuthentication yes`.

Đó là tất cả những gì mình muốn notice. Happy vọcing!!!