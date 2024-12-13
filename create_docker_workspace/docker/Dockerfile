# 使用基础镜像，可以根据需要选择 Ubuntu、Debian 或其他 Linux 发行版
FROM ubuntu:24.04

# 设置非交互模式，避免安装过程中的交互式提示
ENV DEBIAN_FRONTEND=noninteractive

# 修改为清华源
# https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ noble
RUN sed -i 's#archive.ubuntu.com#mirrors.tuna.tsinghua.edu.cn#g' /etc/apt/sources.list.d/ubuntu.sources
RUN sed -i 's#security.ubuntu.com#mirrors.tuna.tsinghua.edu.cn#g' /etc/apt/sources.list.d/ubuntu.sources

# 更新系统并安装必要的软件包，包括 openssh-server
RUN apt update && apt install -y openssh-server && apt clean

# 创建 SSH 运行目录
RUN mkdir /var/run/sshd

## 设置 root 用户密码
# RUN echo 'root:root' | chpasswd

# 将本地公钥文件复制到镜像中
COPY id_rsa.pub /root/.ssh/authorized_keys

## 允许 root 用户通过 SSH 登录，并启用密码验证
# RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
# RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# 允许 root 用户通过 SSH 登录
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# 设置 SSH 服务的默认端口（22）
EXPOSE 22

# 启动 SSH 服务
CMD ["/usr/sbin/sshd", "-D"]
