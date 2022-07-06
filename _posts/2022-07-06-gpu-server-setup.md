---
title: Tesla A100服务器搭建踩坑
tags: 文字
key: gpu-server-setup
pageview: true
toc: false      
---

<!--
 * @Date: 2020-04-21 08:06:52
 * @LastEditTime: 2022-07-06 20:23:18
 * @LastEditors: Li Xiang
 * @Description: 
 * @FilePath: \notlixiang.github.io\_posts\2022-07-06-gpu-server-setup.md
-->

<style type="text/css">
	mark { 
        background-color:grey; 
        color:grey; 
    } 
</style>

新到了台Nvidia Tesla A100 x8 的炼丹炉，因为最近急着用所以自己在上面搭了环境。踩了两个坑，这里记录一下。

## pytorch使用cuda设备报错

装好驱动和cuda，准备好我常用的torch1.10.0-cu102环境后，torch用不了cuda设备。
具体表现为表现为tensor.cuda()直接报错(没gpu的话应该直接返回False，不会报错)。

这个问题是除了常规安装驱动以外，还需要安装[NVIDIA Fabric Manager](https://docs.nvidia.com/datacenter/tesla/pdf/fabric-manager-user-guide.pdf)。

```shell
# 安装程序，driver-branch对应驱动版本
sudo apt-get install cuda-drivers-fabricmanager-<driver-branch> 
# 启动服务
sudo systemctl start nvidia-fabricmanager
# 设置开机启动
sudo systemctl enable nvidia-fabricmanager
```

然后就能够使用torch.tensor.cuda()了。

## 多卡并行报错

接下来试着跑了下训练，又挂了，这次是nccl报错。大致定位了一下问题，这次的错误应该是是cuda版本太低了，导致软硬件兼容性不好。

换成了cuda11.3配torch1.10.0-cu113，就正常了。
