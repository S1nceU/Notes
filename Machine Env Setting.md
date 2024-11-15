# Docker command
docker run -it --gpus=all --shm-size="16g" --mount type=bind,source=d://,target=/data -w /workspace --name machine1 -p 8088:99 machine:latest /bin/bash

# SSH 設定
- passwd
- vim /etc/ssh/sshd_config ##修改ssh连接的设置，修改下面几项配置
```
PermitRootLogin yes 
port 99 
PubkeyAuthentication yes 
PasswordAuthentication yes
```
## SSH 自啟動
- touch /root/start_ssh.sh
- vim /root/start_ssh.sh
- chmod +x /root/start_ssh.sh

- /etc/init.d/ssh restart
- /etc/init.d/ssh status

- start_ssh.sh
```
#!/bin/bash
LOGTIME=$(date "+%Y-%m-%d %H:%M:%S")
echo "[$LOGTIME] startup run..." >>/root/start_ssh.log
service ssh start >>/root/start_ssh.log
##service mysql start >>/root/star_mysql.log   //其他服务也可这么实现
```

- vim /root/.bashrc
```
if [ -f /root/start_ssh.sh ]; then
      /root/start_ssh.sh
fi
```

# 激活 moead

- vim ~/.bashrc
```
export PATH="/opt/conda/bin:$PATH"
```
- source ~/.bachrc
- conda init
- exec bash
- conda activate moead
# cuda 以及 cuDMM 環境變數
```
export PATH=/usr/local/cuda/bin:$PATH 
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CUDNN_INCLUDE_DIR=/usr/include
export CUDNN_LIB_DIR=/usr/lib/x86_64-linux-gnu
export LD_LIBRARY_PATH=\$CUDNN_LIB_DIR:\$LD_LIBRARY_PATH
```


```
CUDA_DEVICE_ORDER="PCI_BUS_ID" CUDA_VISIBLE_DEVICES=0,4 python -c "import torch;print(torch.cuda.is_available());"

# Checks if `cuda` is available via an `nvml-based` check which won't trigger the drivers and leave cuda uninitialized.
CUDA_DEVICE_ORDER="PCI_BUS_ID" PYTORCH_NVML_BASED_CUDA_CHECK=1 CUDA_VISIBLE_DEVICES=0,1,2,3 python -c "import torch;print(torch.cuda.is_available());"


CUDA_DEVICE_ORDER="PCI_BUS_ID" CUDA_VISIBLE_DEVICES=0,1,2,3 python -c "from accelerate import Accelerator;import torch;print(torch.cuda.is_available());"

```