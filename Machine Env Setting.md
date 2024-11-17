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
export LD_LIBRARY_PATH=$CUDNN_LIB_DIR:$LD_LIBRARY_PATH

export LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64:
                       /usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH
```


```
CUDA_DEVICE_ORDER="PCI_BUS_ID" CUDA_VISIBLE_DEVICES=0,4 python -c "import torch;print(torch.cuda.is_available());"

# Checks if `cuda` is available via an `nvml-based` check which won't trigger the drivers and leave cuda uninitialized.
CUDA_DEVICE_ORDER="PCI_BUS_ID" PYTORCH_NVML_BASED_CUDA_CHECK=1 CUDA_VISIBLE_DEVICES=0,1,2,3 python -c "import torch;print(torch.cuda.is_available());"


CUDA_DEVICE_ORDER="PCI_BUS_ID" CUDA_VISIBLE_DEVICES=0,1,2,3 python -c "from accelerate import Accelerator;import torch;print(torch.cuda.is_available());"

```

- print(torch.cuda.is_available())


---
### **MoEAD 環境設置指南**

  

本指南提供逐步設置 **MoEAD** 專案的流程，使用 Docker 確保能夠成功建置與執行。請按照以下步驟操作。

  

---

  

### **1. 前置準備**

  

1. **作業系統：** Windows 或 Linux，並已安裝 Docker。

2. **NVIDIA 顯示卡：** 支援 GPU 並已安裝驅動程式。

3. **必要軟體：**

   - **Docker:** 可從 [這裡下載](https://www.docker.com/)。

   - **NVIDIA Docker:** 用於 Docker 中 GPU 的通過支持。

  

4. **專案文件：**

   - 確保 `Dockerfile` 位於工作目錄中。

   - 包含 `MoEAD` 目錄以及必要的子模組（如 `fastmoe`）與數據文件。

  

---

  

### **2. 建置 Docker 映像檔**

  

1. **進入工作目錄：**

   ```bash

   cd <你的工作目錄路徑>

   ```

  

2. **建置 Docker 映像檔：**

   ```bash

   docker build -t custom-moead:latest .

   ```

  

3. **確認映像檔是否成功建置：**

   ```bash

   docker images

   ```

   預期輸出：

   ```

   REPOSITORY     TAG       IMAGE ID       CREATED         SIZE

   custom-moead   latest    <映像檔ID>     <建立時間>      <大小>

   ```

  

---

  

### **3. 運行 Docker 容器**

  

1. **啟動容器並啟用 GPU 支持：**

   ```bash

   docker run -it --gpus all --shm-size=32g -v <MoEAD專案目錄的主機路徑>:/app custom-moead:latest

   ```

  

   將 `<MoEAD專案目錄的主機路徑>` 替換為你的 `MoEAD` 專案完整目錄。

  

2. **檢查 NVIDIA GPU 是否可用：**

   在容器內執行：

   ```bash

   nvidia-smi

   ```

   應顯示 GPU 詳細資訊與 CUDA 版本。

  

---

  

### **4. 配置並安裝 FastMoE**

  

1. **啟用 `moead` Conda 環境：**

   ```bash

   source activate moead

   ```

  

2. **進入 FastMoE 目錄：**

   ```bash

   cd /app/fastmoe

   ```

  

3. **安裝其他依賴：**

   ```bash

   python -m pip install -r requirements.txt

   ```

  

4. **安裝 FastMoE：**

   ```bash

   python setup.py install

   ```

  
  

5. **確認 GPU 是否可用：**

   ```bash

   python -c "import torch; print(torch.cuda.is_available())"

   ```

   預期輸出：

   ```

   True

   ```

  

---

  

### **5. 準備訓練**

  

1. **進入實驗目錄：**

   ```bash

   cd /app/experiments/MVTec-AD

   ```

  

2. **確認配置文件：**

   檢查 `config.yaml` 中的路徑是否正確指向數據集與檢查點目錄。如有需要，請進行修改。

  

---

  

### **6. 訓練模型**

  

1. **開始訓練過程：**

   ```bash

   PYTHONPATH=$PYTHONPATH:/app CUDA_VISIBLE_DEVICES=0 torchrun --nproc_per_node=1 /app/tools/train_val.py --config /app/experiments/MVTec-AD/config.yaml

   ```

  

---

  

### **問題排查**

  

- **Docker 建置錯誤：**

  確保所有必要目錄與文件（例如 `fastmoe`）都存在於工作目錄中。

- **CUDA 版本不匹配：**

  驗證 NVIDIA 驅動版本與 Docker 映像檔的 CUDA 版本（例如 12.1）是否一致。

  

- **訓練腳本執行失敗：**

  檢查是否缺少依賴或 `config.yaml` 文件中的數據集路徑是否正確。

  

- **FastMoE 安裝問題：**

  若未配置 NCCL，請確保使用 `USE_NCCL=0`。

  

本指南確保以結構化方式設置與訓練 MoEAD。如果遇到其他問題，請隨時聯繫我！