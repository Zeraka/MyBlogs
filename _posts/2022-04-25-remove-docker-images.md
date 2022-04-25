---
layout: post
title: Shell脚本编程——快速删除doker images
subtitle: version 1.0
tags: [linux, shell, automation]
comments: true
---

# Shell脚本编程——快速删除doker images(v1)

## 按照image id删除匿名镜像

由于docker images在build过程中可能会分好多层，不断的build可能会导致遗留下许多的匿名images,  因为一个匿名的image可能基于其上构建了多个container,所以清理它们是一个麻烦事。故编写了一个快速清理docker images的shell脚本。

删除匿名docker images，REP和TAG均为NONE的镜像如下

```shell
[root@localhost ~]# docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
<none>                                     <none>    2499b8088873   13 days ago    225MB
qatnginx                                   v1        5aebae2dddb5   13 days ago    713MB
registry.fi.intel.com/ipuustin/envoy-qat   1.20      27771ff49d79   6 weeks ago    1.46GB
redhat/ubi8                                8.4-213   b1e63aaae5cf   6 months ago   225MB
hello-world                                latest    feb5d9fea6a5   7 months ago   13.3kB
centos                                     latest    5d0da3dc9764   7 months ago   231MB
```

按照image id删除，执行后出现：

```shell
[root@localhost ~]# docker rmi 2499b8088873
Error response from daemon: conflict: unable to delete 2499b8088873 (must be forced) - image is being used by stopped container f82da70de24a
```

这表明仍有基于该镜像启动的container,故首先要删除所有基于该image启动的container。

如何自动识别这些container的ID？

container id 是16进制数组成，1-f, 报错的log里，第一个数是所要删除的 image id。

**首先提取报错结果。**

在shell中使用

```shell
tmp=`docker rmi $id`
```

无法提取出该结果到变量tmp中，把该结果重定向至文件中也不行。

直接在终端上操作，仍无济于事：

```
[root@localhost test_qat_nginx_docker]# a=`docker rmi 2499b8088873`
Error response from daemon: conflict: unable to delete 2499b8088873 (must be forced) - image is being used by stopped container f82da70de24a
```

**这说明执行的报错应该不是正常输出，而是err输出。需要进行输出重定向**

[shell输入输出重定向](https://www.runoob.com/linux/linux-shell-io-redirections.html)

把docker rmi 的标准输出和标准错误输出都重定向至一个变量:

```
#将所有结果重定向至tmp文件
#docker rmi $id > tmp 2>&1

#将所有结果重定向至tmp变量
tmp=`docker rmi $id  2>&1`
```

提取第二个ID，推荐使用正则表达式，因为报错结果格式可能改变，但是报错结果中一定有container id.

**运用正则表达式匹配到container id**，

```
# 由于Error结果是一行字符串，因此用grep筛选出来的仍是一行
#tmp=`docker rmi $id 2>&1 | grep [0-9a-f]`
```

**shell查找单词**

**[shell脚本：删除文本中的字母、找单词、算数字](https://blog.csdn.net/Powerful_Fy/article/details/103264199)**

先将这一行按照空格分隔，并且计算有多少个单词。再去遍历每一个单词，如果这个单词符合正则匹配，则输出它。

shell实现for循环：[shellfor循环](https://www.jb51.net/article/186134.htm)

于是写了这段话：

shell可以写类C格式的for循环

```shell
if [[ $1 == -id ]];then
number=`docker rmi $image_id  2>&1 | awk '{print NF}'`
for ((i=1;i<=$number;i++)) # 双括号，不用分隔
do
        j=$i
        tmp=`docker rmi $image_id 2>&1 | awk '{print $j}'`
         echo $tmp
done
fi
```

但是打印的结果却是这样：

```
Error response from daemon: conflict: unable to delete 2499b8088873 (must be forced) - image is being used by stopped container f82da70de24a
Error response from daemon: conflict: unable to delete 2499b8088873 (must be forced) - image is being used by stopped container f82da70de24a
Error response from daemon: conflict: unable to delete 2499b8088873 (must be forced) - image is being used by stopped container f82da70de24a
Error response from daemon: conflict: unable to delete 2499b8088873 (must be forced) - image is being used by stopped container f82da70de24a
```

原因在于`print $j`, 在awk的{}中的print是无法直接打印shell中的变量的，因为awk并没有直接和当前的shell共享同一个环境，因此{print $j}的$j与前面的变量j并无关系，因此{}中的$j便是一个空值， 原句变成了{print}, 从而打印了整行。

**所以，必须把前文的j变量传入awk环境中，使用-v参数。并且，awk中的变量，不需要加$前缀！**

```shell
if [[ $1 == -id ]];then
number=`docker rmi $image_id  2>&1 | awk '{print NF}'`
for ((i=1;i<=$number;i++)) # 双括号，不用分隔
do
        tmp=`docker rmi $image_id 2>&1 | awk -v j=$i '{print $j}'`
         echo $tmp
done
fi
```

从而打印出如下结果,实现化行为列，将一句话变为单词

```
[root@localhost test_qat_nginx_docker]# ./remove_images.sh -id
Error
response
from
daemon:
conflict:
unable
to
delete
2499b8088873
(must
be
forced)
-
image
is
being
used
by
stopped
container
f82da70de24a
```

只要在原来的基础上对每个单词进行正则判断，即可选择出 container id。

可以用管道grep进行选择。

[grep正则表达式](https://blog.csdn.net/hdyebd/article/details/83096612)

```shell
echo xxx | grep -E "[0-9a-f]*"
```

grep用于在文件中查找字符串。所以如果这样用`grep "xxx" bbb`,则是说在bbb文件中查找符合xxx字符串的行。

于是写下如下代码：

```shell
if [[ $1 == -id ]];then
number=`docker rmi $image_id  2>&1 | awk '{print NF}'`
for ((i=1;i<=$number;i++))
do
        tmp=`docker rmi $image_id 2>&1 | awk -v j=$i '{print $j}'`
        echo $tmp | grep -E "[0-9a-f]*"

done

fi
```

得到如下结果：

```
Error
response
from
daemon:
conflict:
unable
to
delete
2499b8088873
(must
be
forced)
-
image
is
being
used
by
stopped
container
f82da70de24a
```

仍然出错，这是因为正则表达式写错了，"[0-9a-f]*"指的是任何包含字母和小写数字的字符串，且数量从0至无穷多，无疑这个正则表达式显得毫无意义。

正确的正则表达式该如何写？如何表达16进制数？如果这个16进制数不以0x开头且位数不固定，则没有解。但是可以发现，上面的结果中，16进制数都是12位。也就是说可以通过固定位数来表示：

```
"[0-9a-fA-F]{28}"
```

故，代码为:

```shell
if [[ $1 == -id ]];then
number=`docker rmi $image_id  2>&1 | awk '{print NF}'`
for ((i=1;i<=$number;i++))
do
        tmp=`docker rmi $image_id 2>&1 | awk -v j=$i '{print $j}'`
        id=`echo $tmp | grep -E "[0-9a-f]{12}"`
        echo $id
done
fi
```

顺利打印出两个符合要求的id, 但是其中一个id是镜像id, 需要排除。因此可用字符串判断。

```shell
if [[ $1 == -id ]];then
number=`docker rmi $image_id  2>&1 | awk '{print NF}'`
for ((i=1;i<=$number;i++))
do
        tmp=`docker rmi $image_id 2>&1 | awk -v j=$i '{print $j}'`
        id=`echo $tmp | grep -E "[0-9a-f]{12}"`
        if [ ${#id} -ne 0 ]&&[[ $id != $image_id ]];then # 判断字符串等式时用[[]], 而用-n这类参数用[]，如果前者使用[],就会报 [: !=: unary operator expected
        docker rm $id
fi
done
fi
```

判断字符串长度是否为0，如果不是0且id不同于image id， 则必然符合要求。

由于一个docker image可能创建多个docker container, 因此再执行docker rmi时仍然需要判断是否需要删除。

使用while循环，直到完全删除该镜像。

最好使用while [1] 加 break判断, 可以直接写while true

```shell
#!/bin/bash
if [[ $1 == -id ]];then
        while true
        do
                ret=`docker rmi $2  2>&1`
                flag=`echo $ret | grep -i error` 
                if [[ ${#flag} -ne 0 ]];then # 如果出现报错，则说明需要删除container id
                        number=`echo $ret | awk '{print NF}'`
                        for ((i=1;i<=$number;i++))
                        do
                                tmp=`docker rmi $2 2>&1 | awk -v j=$i '{print $j}'`
                                id=`echo $tmp | grep -E "[0-9a-f]{12}"`
                                if [  ${#id} -ne 0  ]&&[[ $id != $2 ]];then
                                        docker rm $id
                                fi
                        done
                else # 如果不再出现报错，说明image已经被删除
                        echo finish delete the image $2
                        break
                fi
        done
fi
```

[shell while循环](https://blog.csdn.net/wdz306ling/article/details/79602739)

[shell while循环2](http://c.biancheng.net/view/1006.html)

执行该脚本`./remove_docker_image -id 1ff262e568c9`, 表示删除id为1ff262e568c9的docker镜像.

## 注意

如果该镜像**存在正在运行的container**,会报出和匿名镜像不同的错误：

```shell
Error response from daemon: You cannot remove a running container 9929ed991b68f780e04c1abcb2fac7ef1c76cfceaa3c43cad492ca30e1c65b95. Stop the container before attempting removal or force remove
```

如果docker ps 中有正在运行的container , 则必须先用docker stop关闭它，再去用docker rm删除它。

关闭所有的正在运行的container代码思路如下:

```
[root@localhost test_qat_nginx_docker]# a=`docker ps -a | awk '{print $1}'`
[root@localhost test_qat_nginx_docker]# echo $a
CONTAINER fe7f7ed97db9 eca9f5aed918 3be2c77a9c27 9ded5a399602 6564ed60f848 66943fd04fa8 739f0ef6a484 5925d60ebc43 ce4ef2fdcd91 bf1cf3e39022 13dc44f1daf9 f3283e1a4774 f87227d569b6 f8d971c6a030 cd48065a9c41 19b2071512b7 a98af68b14c3 4ba00bc28d02 ebb0a3d3cd4a 68ae5d3d0998 174d827148eb 41d8c70200d5 a31198354fcc 981047c562b3 f0d579ea61c4 1cfefedf69f2 74cf30a0db0c 5a8be9d09c4e f904567639f0 cb17a3c086ac d5eebf550134 c46c9c225e55 47904f156b60 83126e499a20 b9389fe60ba7 07db48c20be4 312f68146321 a3c9e06f6d3e 8de98c3cb044 26e9ca00a477 0b448058e0f8 debe2f0ced24
```

因为变量a实际上生成的是分隔符为空格的字符串，可以将a按照单词拆分，遍历，从第二个开始遍历id:

```shell
#!/bin/bash

container_id=`docker ps | awk '{print $1}'`
id_numbers= `echo $container_id | awk '{print NF}'`
for((i=2;i<=id_numbers;i++))
do
	id=`echo $container_id | awk -v j=$i '{print $j}'`
	docker stop $id
done
```



## 优化

### 删除所有匿名镜像以及函数化

匿名镜像是指rep:tag为none的镜像，这些镜像一般是build过程中的中间层。为了简化操作逻辑，只要是REP为NONE的就被识别为匿名镜像。当$1参数为-rep且$2为none时，删除所有匿名镜像。

可以考虑使用sed+awk, 首先过滤`docker images`结果中所有的匿名镜像的id, 再用上面的脚本依次删除它们。

```shell
docker images | grep none | awk '{print $3}'
```

将会得到一列符合要求的id。

由于要反复调用通过id删除镜像的功能，前文的代码应该被**封装为函数**：

```shell
fuction delete_image_id()
{
	while true
        do
                ret=`docker rmi $1  2>&1`
                flag=`echo $ret | grep -i error` 
                if [[ ${#flag} -ne 0 ]];then # 如果出现报错，则说明需要删除container id
                        number=`echo $ret | awk '{print NF}'`
                        for ((i=1;i<=$number;i++))
                        do
                                tmp=`docker rmi $1 2>&1 | awk -v j=$i '{print $j}'`
                                id=`echo $tmp | grep -E "[0-9a-f]{12}"`
                                if [  ${#id} -ne 0  ]&&[[ $id != $1 ]];then
                                        docker rm $id
                                fi
                        done
                else # 如果不再出现报错，说明image已经被删除
                        echo finish delete the image $1
                        break
                fi
        done
}
```

[shell脚本及传参](https://blog.csdn.net/happyhorizion/article/details/80431327)

该函数的使用方法为`delete_image_id $id`, 注意，函数内部也有$0,$1,$2这样的默认参数，$0是函数名。



