---
layout: post
title: How to use in-tree qat driver
subtitle: qat
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [qat, linux]
comments: true
---

## How to use in-tree qat driver

There are two types of QAT driver, in-tree and out-of-tree. If the qat driver is in kernel, it is in-tree otherwise it is out-of-tree. The 5.17 or later kernel will be needed if use in-tree qat driver. 

Use modinfo intel_qat to check if a qat driver is in-tree or out-of-tree.

```
# modinfo intel_qat
filename:       /lib/modules/5.15.0-27-generic/kernel/drivers/crypto/qat/qat_common/intel_qat.ko
```

If the driver's path contains kernel, then it is in-tree, if contains "update" then is out-of-tree.

If use in-tree qat driver, need to install qatlib `https://github.com/intel/qatlib`.

Make sure that the host and the guest all use in-tree qat driver.

```
# modinfo qat_4xxx
# lsmod | grep qat
```

If the results of all above are none, then out-tree driver doesn't exist.  Next install qatlib.

`wget https://github.com/intel/qatlib/archive/refs/tags/21.11.0.tar.gz`

Before install qatlib, must unistall the out-of-tree qat driver. Enter the path of the out-of-tree qat driver:

```
./configure
make uninstall 
make clean
```

And stop the running driver:

```
modprobe -r qat_4xxx # 4xxx is one of the type.
modprobe -r intel_qat
```

Install qatlib:(refer to INSTALL)

```
./autogen.sh
./configure --enable-service
make 
make install 
make samples-install
systemctl restart qat
```

Then execute "cpa_sample_code" to check if qatlib is installed successfully.

```
---------------------------------------
Cipher AES128-CBC
Direction             Encrypt
API                   Traditional
Packet Size           64
Number of Threads     16
Total Submissions     1600000
Total Responses       1600000
Total Retries         220700
CPU Frequency(kHz)    1500092
Throughput(Mbps)      9025
---------------------------------------

Inst 0, Affin: 1, Dev: 0, Accel 0, EE 0, BDF 6B:01:00
Inst 1, Affin: 3, Dev: 0, Accel 0, EE 0, BDF 6B:01:00
```

This information represents that it is successful.

And you can find that VFs of QAT's PF are also created  automatically.

```
lspci | grep Co-
```

## use in-tree qat driver in guest

If want to use in-tree qat driver in guest, must enable viommu.

Enable iommu on guest:

Modify the grub and update grub,then reboot:

```
GRUB_CMDLINE_LINUX="intel_iommu=on iommu=pt default_hugepagesz=1G hugepagesz=1G hugepages=20"
```

Before update the grub, check it is bios or FEI.

```
# [ -d /sys/firmware/efi ] && echo EFI || echo BIOS
UFEI # 
```

If it is EFI, execute:

```
grub-mkconfig -o /boot/efi/EFI/ubuntu/grub.cfg
```

If it is BIOS, execute:

````
grub-mkconfig -o /boot/grub2/grub.cfg
````

Install the qatlib like on host.



## DEBUG-1

If this occurs:

```
if #systemctl enable qat
#systemctl start qat
Created symlink /etc/systemd/system/multi-user.target.wants/qat.service → /lib/
Job for qat.service failed because the control process exited with error code.
See "systemctl status qat.service" and "journalctl -xeu qat.service" for detail
root@guangda:/home/zxq/k6_envoy# systemctl status qat.service
× qat.service - QAT service
     Loaded: loaded (/lib/systemd/system/qat.service; enabled; vendor preset: e
     Active: failed (Result: exit-code) since Tue 2022-05-24 01:37:49 UTC; 12s
    Process: 13695 ExecStartPre=/bin/sh -c test $(getent group qat) (code=exite
    Process: 13697 ExecStartPre=/usr/local/sbin/qat_init.sh (code=exited, statu
    Process: 15271 ExecStart=/usr/local/sbin/qatmgr --policy=${POLICY} (code=ex
        CPU: 2.275s

May 24 01:37:46 guangda systemd[1]: Starting QAT service...
May 24 01:37:49 guangda qatmgr[15271]: Failed qat_mgr_build_data
May 24 01:37:49 guangda systemd[1]: qat.service: Control process exited, code=e
May 24 01:37:49 guangda systemd[1]: qat.service: Failed with result 'exit-code'
May 24 01:37:49 guangda systemd[1]: Failed to start QAT service.
May 24 01:37:49 guangda systemd[1]: qat.service: Consumed 2.275s CPU time.
```

It may be because you don't use "modprobe -r" to remove the previous running qat driver.

## DEBUG -2

use envoy-qat.

```
[error] cpaDcGetInstances() - : Invalid API Param - numInstances is 0
Error in cpaDcGetInstances status = -4
g_process.qz_init_status = QZ_NO_HW
[2022-05-25 08:52:06.487][14][critical][assert] [contrib/qat/compression/qatzip/compressor/source/config.cc:98] assert failure: status == QZ_OK || status == QZ_DUPLICATE. Details: failed to initialize hardware
[2022-05-25 08:52:06.487][14][critical][backtrace] [./source/server/backtrace.h:104] Caught Aborted, suspect faulting address 0x1
[2022-05-25 08:52:06.487][14][critical][backtrace] [./source/server/backtrace.h:91] Backtrace (use tools/stack_decode.py to get line numbers):
[2022-05-25 08:52:06.487][14][critical][backtrace] [./source/server/backtrace.h:92] Envoy version: 1b7cff25107d5b549e389f84104853b35f7503a1/1.21.0/Modified/RELEASE/BoringSSL
```

First look up if the qat status is correct.

```
# systemctl status qat
● qat.service - QAT service
     Loaded: loaded (/lib/systemd/system/qat.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2022-05-25 04:50:41 EDT; 4h 9min ago
   Main PID: 4884 (qatmgr)
      Tasks: 1 (limit: 3252)
     Memory: 4.9M
     CGroup: /system.slice/qat.service
             └─4884 /usr/local/sbin/qatmgr --policy=0

May 25 04:50:40 localhost systemd[1]: Starting QAT service...
May 25 04:50:41 localhost systemd[1]: Started QAT service.

```

Then execute "cpa_stateless_sample runTests=32"

```
#cpa_stateless_sample runTests=32
qaeMemInit started
icp_sal_userStartMultiProcess("SSL") started
*** QA version information ***
device ID               = 0
software                = 21.11.0
*** END QA version information ***
There are no compression instances
Sample code completed successfully.
```

The result means that the in-tree driver can only be used for encryption and decryption but not for compression and decompression.

Go on find why. And The INSTALL of qatlib shows this:

```
Common issues
=============

    Issue: "DC Instances are not present" error when trying to run
        compression operations, e.g. using "cpa_sample_code runTests=32"
    Likely cause: QAT driver in Linux kernel before v5.16 doesn't support
        compression service. Upgrade to a later kernel.
```

Must use kernel whose version is larger than 5.16.

### Update kernel

If your release is ubuntu, then downloading deb binary is simplest.

```
# ls
linux-headers-5.17.4_5.17.4-1_amd64.deb  linux-image-5.17.4_5.17.4-1_amd64.deb  linux-image-5.17.4-dbg_5.17.4-1_amd64.deb  linux-libc-dev_5.17.4-1_amd64.deb
# dpkg -i *
```

