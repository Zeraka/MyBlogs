---
layout: post
title: 测试QAT VF时，遇到ADF_UIO_PROXY报错
subtitle: QAT的使用
tags: [Linux, Performance]
---

## 测试QAT VF时，遇到ADF_UIO_PROXY报错

如果在host上测试QAT的PF，必须关闭sriov, 需要在grub中去掉iommu相关的参数。

如果测试QAT的VF，必须开启sriov, 并且在编译QAT的driver的时候，必须要加入sriov相关的参数：

```
./configure --enable-icp-sriov
make samples && make samples-install
```

ICP(QAT Driver)根目录下执行./configure 时用--再加tab键，可以显示出所有可加参数。 --enable-icp-sriov表明QAT开启VF功能，体现在ICP编译完成后，在/etc下出现类似4xxxvf_dev0.conf, 4xxxvf_dev1.conf这些带有vf后缀的conf文件。4xxx是4940型的QAT的驱动名称，如果是PF，/etc下生成的配置文件就是4xxx_dev0.conf这类。

如果在使用QAT VF的时候，/etc下没有相应的Vf的conf文件，则VF必然运行不起来。可以用ICP自带的测试程序测试PF/VF。上面的`make samples && make samples-install`，会在执行编译时在ICP根目录下的build文件夹中生成二进制可执行程序`cpa_sample_code`,它是用来测试QAT功能是否在本机上正常的。如果测试出现了这种问题：

```
[root@83126e499a20 build]# ./cpa_sample_code
qaeMemInit started
ADF_UIO_PROXY err: icp_adf_userProcessToStart: Error reading /dev/qat_dev_processes file
/root/qatdriver/quickassist/lookaside/access_layer/src/sample_code/performance/cpa_sample_code_main.c, main():420 Could not start sal for user space 
```

说明QAT功能没有正常打开。这里分PF和VF两种情况，如果是PF，可能是iommu没有关，同时确认/etc下是否有4xxx_dev0.conf这样的文件。如果是VF，iommu开启的情况下，仍然出现了错误，可以查看dmesg日志。

直接执行dmesg，查看系统启动信息，其中会包含QAT启动时的成功或是错误的日志，最终结果发现:

```
[ 662.213690] QAT: could not find SSL section in any config files
```

是conf文件缺失导致的。再去查看/etc,确实是发现缺少了vf的conf。

