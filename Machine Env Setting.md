docker run -it --gpus=all --shm-size="16g" -v /d -w /workspace --name machine1 -p 8088:99 machine:latest /bin/bash


root@c19bdcee0db9:/workspace$ vim /etc/ssh/sshd_config ##修改ssh连接的设置，修改下面几项配置
PermitRootLogin yes 
port 99 
PubkeyAuthentication yes 
PasswordAuthentication yes







touch /root/start_ssh.sh
vim /root/start_ssh.sh
chmod +x /root/start_ssh.sh


start_ssh.sh

#!/bin/bash
LOGTIME=$(date "+%Y-%m-%d %H:%M:%S")
echo "[$LOGTIME] startup run..." >>/root/start_ssh.log
service ssh start >>/root/start_ssh.log
##service mysql start >>/root/star_mysql.log   //其他服务也可这么实现




vim /root/.bashrc

if [ -f /root/start_ssh.sh ]; then
      /root/start_ssh.sh
fi


export PATH="/opt/conda/bin:$PATH"