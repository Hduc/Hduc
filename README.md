## Lệnh trong linux
```
apt-get update

ln -s /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime

apt-get install <name>

```
### Stop Apache from Starting on Linux (cần chạy khi sử dụng docker || deloy server)
`sudo systemctl disable httpd && sudo systemctl stop httpd`
### Lệnh setup docker server
```

yum install git

sudo yum check-update

curl -fsSL https://get.docker.com/ | sh

sudo systemctl start docker

sudo systemctl enable docker (khi khởi dộng lại tự dộng chạy docker)

sudo yum update

sudo yum upgrade

sudo yum install curl

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
### tạo folder
`mkdir <path>`

### xóa folder 
`rm -rf <folder>`

### di chuyển folder || hoặc đổi tên
`mv <path from> <path to>`

### Kiểm tra CPU
`cat /proc/cpuinfo`

### Kiểm tra RAM trống
`free -h`
Chi tiết hơn:`cat /proc/meminfo`

### Kiểm tra ổ cứng
`df -h`

