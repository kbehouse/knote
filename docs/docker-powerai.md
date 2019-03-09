---
id: docker-powerai
title: Docker for PowerAI
sidebar_label: PowerAI
---

## Generate One Docker

下方這段可產生1個Docker來使用`第0顆`GPU

```
docker run -d --runtime=nvidia  -p 6052:22 -p 6056:6006 -p 6058:8888  -e NVIDIA_VISIBLE_DEVICES=0 --name 6052 -h 6052 -v /home/user01/paper/arm_52:/out ppc64le-tf-keras-nb:py3_tf_r1.7_ok
```
* -d: detached(分離) mode
* --runtime=nvidia: 使用nvidia docker
* -p: port mapping  
    1. -p 6052:22 -> ssh mapping 
    2. -p 6058:8888 -> juppyter-notebook mapping
* -v: 掛載(mount)裝置，這裡是掛載硬碟空間
> Docker的資料在容器(container)結束後，資料也會消失，所以透過掛載硬碟，可讓資料永久保存在硬碟中

* ppc64le-tf-keras-nb:py3_tf_r1.7_ok:是映象檔(image)的名稱


## Generate Multiple Dockers

程式碼如下：
[gen_dokcer.py](https://github.com/kbehouse/docker_note/blob/master/gen_docker.py)

下方這段可產生8個Docker來共用4顆GPU，
> * 因為目前nvidia for docker，還沒有開放限制GPU的使用量
> * 所以二個docker共用一顆GPU會造成GPU之memory不足的問題，因此共用GPU的部份要注意


下面只會顯示出來，不會執行。如果確定要執行，把system(cmd)的註解打開
```
from os import system

num = 8
base_port = 6010
for i in range(num):
    ssh_out     = base_port + i * 5 + 2   # 6012 (first)
    jupyter_out = base_port + i * 5 + 3           # 6013 (first)
    gpu_id = i % 2

    cmd='docker run -d --runtime=nvidia  -p {}:22 -p {}:8888  -e NVIDIA_VISIBLE_DEVICES={} --name ssh{} -h ssh{} ppc64le-tf-keras-nb'.format(ssh_out, jupyter_out, gpu_id,ssh_out,ssh_out)

    print(cmd)
    #system(cmd)
```

