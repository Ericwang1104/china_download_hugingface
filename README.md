# china_download_hugingface
# # huggingface模型下载到国内本地NAS共享使用的方法

## linux下载到nas 共享模型的方法

- linux 下安装smb协议
  
  ```
  sudo apt update
  sudo apt install samba
  sudo systemctl enable smbd
  ```
  
- 设置映射
  修改fstab 加入如下内容 具体ip及路径 参考自己的nas 设置 
  ```//192.168.1.123/public/ /mnt/public cifs credentials=/etc/samba/credentials,file_mode=0777,dir_mode=0777 0 0```
  
- 编辑/etc/samba/credentials 文件 文件内容如下
  
  ```
  user=xxxxx   
  password=xxxxx 
  ```
  
- 安装python3.6 及以上
  
- 安装 huggingface_cli
  
  ```
  pip install huggingface-hub
  ```
  
- 编写 linux 下 shell dw_model.sh
  

```
#!/bin/bash    
#设置环境变量
export HF_HOME=/mnt/public/huggingface
export HF_ENDPOINT=https://hf-mirror.com                                                                        
#模型名称作为参数                                                                     
MODEL_NAME=$1
#检查是否提供了模型名称                                                               
if [ -z "$MODEL_NAME" ]; then                                                          
    echo "错误：没有提供模型名称。"
    echo "用法：$0 [模型名称]"
    exit 1
fi                                                                                     
#使用 huggingface-cli 下载模型                                                        
huggingface-cli download --resume-download --local-dir-use-symlinks False $MODEL_NAME    
```

保存shell 为 dw_model.sh

执行 ``` .\dw_model.sh xxxxxx\xxxxx ``` 就可以了

## windows下载到nas 共享模型的方法

- 安装python3.6 及以上
  
- 安装 huggingface_cli
  
  ```
  pip install huggingface-hub
  ```
  
- 编辑dw_model.bat 批处理文件
  

```
@echo off
rem 设置环境变量
set HF_HOME=\\192.168.1.9\public\huggingface
set HF_ENDPOINT=https://hf-mirror.com

rem 检查是否提供了模型名称
if "%~1"=="" (
    echo 错误: 没有提供模型名称。
    echo 用法: %0 [模型名称]
    exit /b 1
)

rem 使用 huggingface-cli 下载模型
huggingface-cli download --resume-download --local-dir-use-symlinks False  %1
```
