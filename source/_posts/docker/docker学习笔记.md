---
title: docker学习笔记
tags:
  - Docker
categories:
  - Docker
abbrlink: 177bfdaa
date: 2024-10-05 16:20:26
---

docker学习笔记

<!-- more -->

# Docker概念

## docker的基本组成

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051638767.png)


**镜像**

镜像只是一个只读的模板，镜像可以用来创建Docker容器，一个镜像可以创建很多容器（最终的服务或项目是运行在容器中的）



**容器**

Docker利用容器独立运行的一个或一组应用。容器是用镜像创建的运行实例。每个容器都是相互隔离的，保证安全的平台。可以把容器看作是一个简易版的Linux环境(包括root用户权限，进程空间，用户空间和网络空间等)和运行在其中的应用程序。



**仓库**

仓库是集中存放镜像文件的场所。

docker仓库类似于github，有共有仓库和私有仓库。Docker Hub（国外）阿里云（国内）。



# Docker命令

## 镜像命令

**docker images**  查看本机所有的镜像

**docker search**  搜索镜像

```shell
howu@howu-ubuntu ~> docker search ubuntu

# 可添加过滤规则
--filter=STARS=3000
howu@howu-ubuntu ~> docker search ubuntu --filter=STARS=3000
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
ubuntu              Ubuntu is a Debian-based Linux operating sys…   11515               [OK]
```



**docker pull**  获取镜像

```shell
howu@howu-ubuntu ~> docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
6a5697faee43: Pull complete
ba13d3bc422b: Pull complete
a254829d9e55: Pull complete
Digest: sha256:fff16eea1a8ae92867721d90c59a75652ea66d29c05294e6e2f898704bdb8cf1
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```



**docker rmi**  删除镜像

```she
howu@howu-ubuntu ~> docker rmi db2b37ec6181
Untagged: mysql:latest
Untagged: mysql@sha256:8c17271df53ee3b843d6e16d46cff13f22c9c04d6982eb15a9a47bd5c9ac7e2d
Deleted: sha256:db2b37ec6181ee1f367363432f841bf3819d4a9f61d26e42ac16e5bd7ff2ec18
Deleted: sha256:4d2a8e0ab441c5c1170a783be98b47096324f531f84cc387f06523af79ee7cd5
Deleted: sha256:09e5f20e5b3289cedaf4824906f74057b5b9b8765a246d8bfaafded2562a901a
Deleted: sha256:c25bdaaaf298842706f214fd3d855ca6f215cc66d01345efb9fd7479f6ca7b5f
Deleted: sha256:64231a7235fe05862b419beb8884dec599d27cab1b1bb402f0f75e64bc4cbd19
Deleted: sha256:79bc33a7ecad0032cc5e218d8b246b645f4cfddbf639b5db8383f81d4cbb6c9b
Deleted: sha256:33134afe9e842a2898e36966f222fcddcdb2ab42e65dcdc581a4b38b2852c2e0
Deleted: sha256:dd053ec71039c3646324372061452778609b9ba42d1501f6a72c0272c94f8965
Deleted: sha256:2d4c647f875f6256f210459e5c401aad27ad223d0fa3bada7c4088a6040f8ba4
Deleted: sha256:4bded7e9aa769cb0560e60da97421e2314fa896c719179b926318640324d7932
Deleted: sha256:5fd9447ef700dfe5975c4cba51d3c9f2e757b34782fe145c1d7a12dfbee1da2f
Deleted: sha256:5ee7cbb203f3a7e90fe67330905c28e394e595ea2f4aa23b5d3a06534a960553
Deleted: sha256:d0fe97fa8b8cefdffcef1d62b65aba51a6c87b6679628a2b50fc6a7a579f764c
```





## 容器命令

### **新建容器并启动**

```shell
#参数说明
--name="Name"   指定容器的名字  
-d				后台方式运行
-it				使用交互模式运行
--rm			容器在终止后立即删除容器，不能与-d参数同时使用
-p
	-p ip:主机端口：容器端口
	-p 主机端口：容器端口
	-p 容器端口
-P				随机指定端口
howu@howu-ubuntu ~> docker run -it ubuntu /bin/bash
root@49c2189c70c8:/#
```

容器端口映射：

https://www.cnblogs.com/panwenbin-logs/p/11205614.html



**列出所有运行的容器**

```shell
howu@howu-ubuntu ~> docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
49c2189c70c8        ubuntu              "/bin/bash"         5 minutes ago       Up 5 minutes                            zealous_goldberg
howu@howu-ubuntu ~>
howu@howu-ubuntu ~>
howu@howu-ubuntu ~> docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                  PORTS               NAMES
49c2189c70c8        ubuntu              "/bin/bash"         5 minutes ago       Up 5 minutes                                zealous_goldberg
9e034b52cfe7        centos:latest       "/bin/bash"         2 days ago          Exited (0) 2 days ago                       objective_chaum
0088f5db4629        hello-world         "/hello"            3 days ago          Exited (0) 3 days ago                       determined_albattani
```



**退出容器**

```shell
exit     #容器停止并退出
Ctrl + P + Q    # 容器不停止退出
```



**删除容器**

```shell
docker rm 容器ID
docker rm -f $(docker ps -aq)   #删除所有的容器
```



**启动和停止容器**

```shell
docker start 容器ID
docker restart 容器ID
docker stop 容器ID
docker kill 容器ID
```



### 常用其它命令

#### **后台启动容器**

```shell
docker run -d 镜像名
# docker 容器使用后台运行，就必须要有一个前台的进程，docker发现没有应用，就会自动停止
```



#### **查看日志**

```shell
docker logs -tf --tail 10 容器ID
```



#### **查看容器中的进程信息**

```shell
docker top 容器ID
```



#### **查看容器属性**

```shell
docker inspect 容器ID
```



#### **进入当前正在运行的容器**

```shell
#方式一  进入容器后会开启一个新的终端，可以在里面操作
docker exec -it 容器ID bashShell

#方式二  进入容器正在执行的终端，不会启动新的进程
docker attach 容器ID
```



#### **容器拷贝文件**

```shell
# 往容器内拷贝
howu@howu-ubuntu ~/g/s/hello> docker cp hello 49c2189c70c8:/
# 往容器外拷贝
docker cp 49c2189c70c8:/ljt.txt ./
```



#### 查看端口映射

```shell
docker port 容器名 端口名
```





# 容器支撑技术-namespace

目前linux内核支持7种不同类型的namespace。

```shell
名称        宏定义             隔离内容
Cgroup      CLONE_NEWCGROUP   Cgroup root directory (since Linux 4.6)
IPC         CLONE_NEWIPC      System V IPC, POSIX message queues (since Linux 2.6.19)
Network     CLONE_NEWNET      Network devices, stacks, ports, etc. (since Linux 2.6.24)
Mount       CLONE_NEWNS       Mount points (since Linux 2.4.19)
PID         CLONE_NEWPID      Process IDs (since Linux 2.6.24)
User        CLONE_NEWUSER     User and group IDs (started in Linux 2.6.23 and completed in Linux 3.8)
UTS         CLONE_NEWUTS      Hostname and NIS domain name (since Linux 2.6.19)
```

> ps: Cgroup现在在docker中还未使用

## 容器与进程间关系

首先明确一点：容器不能脱离进程而存在，先有进程，后有容器。
然而，大家往往会说到“使用Docker创建Docker Container（容器），然后在容器内部运行进程”。对此，从通俗易懂的角度来讲，这完全可以理解，因为“容器”一词的存在，本身就较为抽象。如果需要更为准确的表述，那么可以是：使用Docker创建一个进程，为这个进程创建隔离的环境，这样的环境可以称为Docker Container（容器），然后再在容器内部运行用户应用进程。

## 查看进程namespace的方式

```shell
ls -l /proc/[pid]/ns
```

![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051637200.png)


Linux可以为docker提供的namespace种类包括文件系统mount、进程间通信、网络等

```c
#define _GUN_SOURCE
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <sched.h>
#include <signal.h>
#include <unistd.h>

/* 定义一个给 clone 用的栈，栈大小1M */
#define STACK_SIZE (1024 * 1024)
static char container_stack[STACK_SIZE];
char* const container_args[] = {
    "/bin/bash",
    NULL
};
int container_main(void* arg)
{
    printf("Container - inside the container!\n");
    execv(container_args[0], container_args); 
    printf("Something's wrong!\n");
    return 1;
}
int main()
{
    printf("Parent - start a container!\n");
    int container_pid = clone(container_main, container_stack+STACK_SIZE, SIGCHLD, NULL);
    /* 等待子进程结束 */
    waitpid(container_pid, NULL, 0);
    printf("Parent - container stopped!\n");
    return 0;
}
```



## UTS namespace

UTS(**UNIX Timesharing System**)命名空间是Linux内核Namespace（命名空间）的一个子系统，主要用来完成对容器HOSTNAME和domain的隔离，同时保存内核名称、版本、以及底层体系结构类型等信息。
![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051638438.png)

UTS命名空间是扁平化的结构，不同的命名空间之间没有层级关系。
UTS namespace没有嵌套关系，即不存在说一个namespace是另一个namespace的父namespace。

UTS命名空间的用来隔离系统的这些信息，使得用户在容器中查看到的信息是当前容器的系统、版本，不同于主机的，内核通过uts_namespace对当前系统中多个容器的这些信息进行统一管理，每一个容器对应有一个自己的uts_namespace，用来隔离容器的内核名称、版本等信息，不同容器查看到的都是属于自己的信息，相互间不能查看

### 示例

```c
int container_main(void* arg)
{
    printf("Container - inside the container!\n");
    sethostname("container", 10);
    execv(container_args[0], container_args);    #？
    printf("Something's wrong!\n");
    return 1;
}
int main()
{
    printf("Parent - start a container!\n");
    int container_pid = clone(container_main, container_stack+STACK_SIZE, CLONE_NEWUTS | SIGCHLD, NULL);
    /* 等待子进程结束 */
    waitpid(container_pid, NULL, 0);
    printf("Parent - container stopped!\n");
    return 0;
}
```

修改主机名成功

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051638120.png)



### 内核中的实现

在每一个任务对应的task结构体[struct task_struct](https://github.com/torvalds/linux/blob/master/include/linux/sched.h)中，有一个字段专门用来描述该任务所处的用户空间，该字段为[struct nsproxy](https://github.com/torvalds/linux/blob/master/include/linux/sched.h)，以下为关键代码：

```c
struct task_struct {
	/* Namespaces: */
	struct nsproxy			*nsproxy;
}

/*
 * A structure to contain pointers to all per-process
 * namespaces - fs (mount), uts, network, sysvipc, etc.
 *
 * The pid namespace is an exception -- it's accessed using
 * task_active_pid_ns.  The pid namespace here is the
 * namespace that children will use.
 *
 * 'count' is the number of tasks holding a reference.
 * The count for each namespace, then, will be the number
 * of nsproxies pointing to it, not the number of tasks.
 *
 * The nsproxy is shared by tasks which share all namespaces.
 * As soon as a single namespace is cloned or unshared, the
 * nsproxy is copied.
 */
struct nsproxy {
	atomic_t count;
	struct uts_namespace *uts_ns;
	struct ipc_namespace *ipc_ns;
	struct mnt_namespace *mnt_ns;
	struct pid_namespace *pid_ns_for_children;
	struct net 	     *net_ns;
	struct cgroup_namespace *cgroup_ns;
};
extern struct nsproxy init_nsproxy;

static inline struct new_utsname *utsname(void)
{
	return &current->nsproxy->uts_ns->name;
}

SYSCALL_DEFINE2(gethostname, char __user *, name, int, len)
{
	int i;
	struct new_utsname *u;
	char tmp[__NEW_UTS_LEN + 1];

	if (len < 0)
		return -EINVAL;
	down_read(&uts_sem);
	u = utsname();
	i = 1 + strlen(u->nodename);
	if (i > len)
		i = len;
	memcpy(tmp, u->nodename, i);
	up_read(&uts_sem);
	if (copy_to_user(name, tmp, i))
		return -EFAULT;
	return 0;
}


```


## IPC Namespace

IPC资源一般就是进程间通讯所使用的内核资源，常用的有以下几种：

* 管道
* 信号
* 共享存储映射
* socket
* 消息队列
  而IPC namespace就是用来隔离这些资源。

### 示例

```shell
#--------------------------第一个shell窗口----------------------
#记下默认的uts和ipc namespace number
dev@ubuntu:~$ readlink /proc/$$/ns/uts /proc/$$/ns/ipc
uts:[4026531838]
ipc:[4026531839]

#确认hostname
dev@ubuntu:~$ hostname
ubuntu

#查看现有的ipc Message Queues，默认情况下没有message queue
dev@ubuntu:~$ ipcs -q
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages

#创建一个message queue
dev@ubuntu:~$ ipcmk -Q
Message queue id: 0
dev@ubuntu:~$ ipcs -q
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages
0x12aa0de5 0          dev        644        0            0


#--------------------------第二个shell窗口----------------------
#重新打开一个shell窗口，确认和上面的shell是在同一个namespace，
#能看到上面创建的message queue
dev@ubuntu:~$ readlink /proc/$$/ns/uts /proc/$$/ns/ipc
uts:[4026531838]
ipc:[4026531839]
dev@ubuntu:~$ ipcs -q
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages
0x12aa0de5 0          dev        644        0            0

#运行unshare创建新的ipc和uts namespace，并且在新的namespace中启动bash
#这里-i表示启动新的ipc namespace，-u表示启动新的utsnamespace
dev@ubuntu:~$ sudo unshare -iu /bin/bash
root@ubuntu:~#

#确认新的bash已经属于新的ipc和uts namespace了
root@ubuntu:~# readlink /proc/$$/ns/uts /proc/$$/ns/ipc
uts:[4026532455]
ipc:[4026532456]

#设置新的hostname以便和第一个shell里面的bash做区分
root@ubuntu:~# hostname container001
root@ubuntu:~# hostname
container001

#当hostname改变后，bash不会自动修改它的命令行提示符
#所以运行exec bash重新加载bash
root@ubuntu:~# exec bash
root@container001:~#
root@container001:~# hostname
container001

#现在各个bash进程间的关系如下
#bash(24429)是shell窗口打开时的bash
#bash(27668)是运行sudo unshare创建的bash，和bash(24429)不在同一个namespace
root@container001:~# pstree -pl
├──sshd(24351)───sshd(24428)───bash(24429)───sudo(27667)───bash(27668)───pstree(27695)

#查看message queues，看不到原来namespace里面的消息，说明已经被隔离了
root@container001:~# ipcs -q
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages

#创建一条新的message queue
root@container001:~# ipcmk -Q
Message queue id: 0
root@container001:~# ipcs -q
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages
0x54b08fc2 0          root       644        0            0

#--------------------------第一个shell窗口----------------------
#回到第一个shell窗口，看看有没有受到新namespace的影响
dev@ubuntu:~$ ipcs -q
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages
0x12aa0de5 0          dev        644        0            0
#完全无影响，还是原来的信息

#试着加入第二个shell窗口里面bash的uts和ipc namespace
#-t后面跟pid用来指定加入哪个进程所在的namespace
#这里27668是第二个shell中正在运行的bash的pid
#加入成功后将运行/bin/bash
dev@ubuntu:~$ sudo nsenter -t 27668 -u -i /bin/bash

#加入成功，bash的提示符也自动变过来了
root@container001:~# readlink /proc/$$/ns/uts /proc/$$/ns/ipc
uts:[4026532455]
ipc:[4026532456]

#显示的是新namespace里的message queues
root@container001:~# ipcs -q
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages
0x54b08fc2 0          root       644        0            0
```

## PID Namespace

PID namespace用来隔离进程的id信息，使得不同pid namespace里的进程ID可以重复且相互之间不影响。

```shell
#查看当前pid namespace的ID
dev@ubuntu:~$ readlink /proc/self/ns/pid
pid:[4026531836]

#启动新的pid namespace
#这里同时也启动了新的uts和mount namespace
#新的uts是为了设置一个新的hostname，便于和老的namespace区分
#新的mount namespace是为了方便我们修改新namespace里面的mount信息，
#因为这样不会对老namespace造成影响
#这里--fork是为了让unshare进程fork一个新的进程出来，然后再用bash替换掉新的进程
#这是pid namespace本身的限制，进程所属的pid namespace在它创建的时候就确定了，不能更改，
#所以调用unshare和nsenter后，原来的进程还是属于老的namespace，
#而新fork出来的进程才属于新的namespace
dev@ubuntu:~$ sudo unshare --uts --pid --mount --fork /bin/bash
root@ubuntu:~# hostname container001
root@ubuntu:~# exec bash
root@container001:~#

#查看进程间关系，当前bash(31646)确实是unshare的子进程
root@container001:~# pstree -pl
├─sshd(955)─┬─sshd(17810)───sshd(17891)───bash(17892)───sudo(31644)──
─unshare(31645)───bash(31646)───pstree(31677)
#他们属于不同的pid namespace
root@container001:~# readlink /proc/31645/ns/pid
pid:[4026531836]
root@container001:~# readlink /proc/31646/ns/pid
pid:[4026532469]

#但为什么通过这种方式查看到的namespace还是老的呢？
root@container001:~# readlink /proc/$$/ns/pid
pid:[4026531836]

#由于我们实际上已经是在新的namespace里了，并且当前bash是当前namespace的第一个进程
#所以在新的namespace里看到的他的进程ID是1
root@container001:~# echo $$
1
#但由于我们新的namespace的挂载信息是从老的namespace拷贝过来的，
#所以这里看到的还是老namespace里面的进程号为1的信息
root@container001:~# readlink /proc/1/ns/pid
pid:[4026531836]
#ps命令依赖/proc目录，所以ps的输出还是老namespace的视图
root@container001:~# ps ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 7月07 ?       00:00:06 /sbin/init
root         2     0  0 7月07 ?       00:00:00 [kthreadd]
 ...
root     31644 17892  0 7月14 pts/0   00:00:00 sudo unshare --uts --pid --mount --fork /bin/bash
root     31645 31644  0 7月14 pts/0   00:00:00 unshare --uts --pid --mount --fork /bin/bash

#所以我们需要重新挂载我们的/proc目录
root@container001:~# mount -t proc proc /proc

#重新挂载后，能看到我们新的pid namespace ID了
root@container001:~# readlink /proc/$$/ns/pid
pid:[4026532469]
#ps的输出也正常了
root@container001:~# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 7月14 pts/0   00:00:00 bash
root        44     1  0 00:06 pts/0    00:00:00 ps -ef
```

PID namespace可以嵌套，也就是说有父子关系，在当前namespace里面创建的所有新的namespace都是当前namespace的子namespace。父namespace里面可以看到所有子孙后代namespace里的进程信息，而子namespace里看不到祖先或者兄弟namespace里的进程信息。

目前PID namespace最多可以嵌套32层，由内核中的宏MAX_PID_NS_LEVEL来定义。

调用unshare或者setns函数后，当前进程的namespace不会发生变化，不会加入到新的namespace，而它的子进程会加入到新的namespace。也就是说进程属于哪个namespace是在进程创建的时候决定的，并且以后再也无法更改。

在一个PID namespace里的进程，它的父进程可能不在当前namespace中，而是在外面的namespace里面（这里外面的namespace指当前namespace的祖先namespace），这类进程的ppid都是0。比如新namespace里面的第一个进程，他的父进程就在外面的namespace里。通过setns的方式加入到新namespace中的进程的父进程也在外面的namespace中。

可以在祖先namespace中看到子namespace的所有进程信息，且可以发信号给子namespace的进程，但进程在不同namespace中的PID是不一样的。

```shell
#--------------------------第一个shell窗口----------------------
#记下最外层的namespace ID
dev@ubuntu:~$ readlink /proc/$$/ns/pid
pid:[4026531836]

#创建新的pid namespace， 这里--mount-proc参数是让unshare自动重新mount /proc目录
dev@ubuntu:~$ sudo unshare --uts --pid --mount --fork --mount-proc /bin/bash
root@ubuntu:~# hostname container001
root@ubuntu:~# exec bash
root@container001:~# readlink /proc/$$/ns/pid
pid:[4026532469]

#再创建新的pid namespace
root@container001:~# unshare --uts --pid --mount --fork --mount-proc /bin/bash
root@container001:~# hostname container002
root@container001:~# exec bash
root@container002:~# readlink /proc/$$/ns/pid
pid:[4026532472]

#再创建新的pid namespace
root@container002:~# unshare --uts --pid --mount --fork --mount-proc /bin/bash
root@container002:~# hostname container003
root@container002:~# exec bash
root@container003:~# readlink /proc/$$/ns/pid
pid:[4026532475]

#目前namespace container003里面就一个bash进程
root@container003:~# pstree -p
bash(1)───pstree(22)
#这样我们就有了三层pid namespace，
#他们的父子关系为container001->container002->container003

#--------------------------第二个shell窗口----------------------
#在最外层的namespace中查看上面新创建的三个namespace中的bash进程
#从这里可以看出，这里显示的bash进程的PID和上面container003里看到的bash(1)不一样
dev@ubuntu:~$ pstree -pl|grep bash|grep unshare
|-sshd(955)-+-sshd(17810)---sshd(17891)---bash(17892)---sudo(31814)--
-unshare(31815)---bash(31816)---unshare(31842)---bash(31843)--
-unshare(31864)---bash(31865)
#各个unshare进程的子bash进程分别属于上面的三个pid namespace
dev@ubuntu:~$ sudo readlink /proc/31816/ns/pid
pid:[4026532469]
dev@ubuntu:~$ sudo readlink /proc/31843/ns/pid
pid:[4026532472]
dev@ubuntu:~$ sudo readlink /proc/31865/ns/pid
pid:[4026532475]

#PID在各个namespace里的映射关系可以通过/proc/[pid]/status查看到
#这里31865是在最外面namespace中看到的pid
#45，23，1分别是在container001，container002和container003中的pid
dev@ubuntu:~$ grep pid /proc/31865/status
NSpid:  31865   45     23      1

#创建一个新的bash并加入container002
dev@ubuntu:~$ sudo nsenter --uts --mount --pid -t 31843 /bin/bash
root@container002:/#

#这里bash（23）就是container003里面的pid 1对应的bash
root@container002:/# pstree -p
bash(1)───unshare(22)───bash(23)
#unshare(22)属于container002
root@container002:/# readlink /proc/22/ns/pid
pid:[4026532472]
#bash（23）属于container003
root@container002:/# readlink /proc/23/ns/pid
pid:[4026532475]

#为什么上面pstree的结果里面没看到nsenter加进来的bash呢？
#通过ps命令我们发现，我们新加进来的那个/bin/bash的ppid是0，难怪pstree里面显示不出来
#从这里可以看出，跟最外层namespace不一样的地方就是，这里可以有多个进程的ppid为0
#从这里的TTY也可以看出哪些命令是在哪些窗口执行的，
#pts/0对应第一个shell窗口，pts/1对应第二个shell窗口
root@container002:/# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 04:39 pts/0    00:00:00 bash
root        22     1  0 04:39 pts/0    00:00:00 unshare --uts --pid --mount --fork --mount-proc /bin/bash
root        23    22  0 04:39 pts/0    00:00:00 bash
root        46     0  0 04:52 pts/1    00:00:00 /bin/bash
root        59    46  0 04:53 pts/1    00:00:00 ps -ef

#--------------------------第三个shell窗口----------------------
#创建一个新的bash并加入container001
dev@ubuntu:~$ sudo nsenter --uts --mount --pid -t 31816 /bin/bash
root@container001:/#

#通过pstree和ps -ef我们可看到所有三个namespace中的进程及他们的关系
#bash(1)───unshare(22)属于container001
#bash(23)───unshare(44)属于container002
#bash(45)属于container003，而68和84两个进程分别是上面两次通过nsenter加进来的bash
#同上面ps的结果比较我们可以看出，同样的进程在不同的namespace里面拥有不同的PID
root@container001:/# pstree -pl
bash(1)───unshare(22)───bash(23)───unshare(44)───bash(45)
root@container001:/# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 04:37 pts/0    00:00:00 bash
root        22     1  0 04:39 pts/0    00:00:00 unshare --uts --pid --mount --fork --mount-proc /bin/bash
root        23    22  0 04:39 pts/0    00:00:00 bash
root        44    23  0 04:39 pts/0    00:00:00 unshare --uts --pid --mount --fork --mount-proc /bin/bash
root        45    44  0 04:39 pts/0    00:00:00 bash
root        68     0  0 04:52 pts/1    00:00:00 /bin/bash
root        84     0  0 05:00 pts/2    00:00:00 /bin/bash
root        95    84  0 05:00 pts/2    00:00:00 ps -ef

#发送信号给contain002中的bash
root@container001:/# kill 68

#--------------------------第二个shell窗口----------------------
#回到第二个窗口，发现bash已经被kill掉了，说明父namespace是可以发信号给子namespace中的进程的
root@container002:/# exit
dev@ubuntu:~$
```

## Mount Namespace

Mount namespace用来隔离文件系统的挂载点，使不同的namespace之间不会受影响，这样用户可以构建容器自己的文件系统。

当前进程所在mount namespace里的所有挂载信息可以在/proc/[pid]/mounts、/proc/[pid]/mountinfo和/proc/[pid]/mountstats里面找到。

Mount namespaces是第一个被加入Linux的namespace，由于当时没想到还会引入其它的namespace，所以取名为CLONE_NEWNS，而没有叫CLONE_NEWMOUNT。

每个mount namespace都拥有一份自己的挂载点列表，当用clone或者unshare函数创建新的mount namespace时，新创建的namespace将拷贝一份老namespace里的挂载点列表，但从这之后，他们就没有关系了，通过mount和umount增加和删除各自namespace里面的挂载点都不会相互影响。


## User Namespace

User namespace用来隔离user权限相关的Linux资源，包括user IDs and group IDs，keys , 和capabilities.

user namespace可以嵌套（目前内核控制最多32层），除了系统默认的user namespace外，所有的user namespace都有一个父user namespace，每个user namespace都可以有零到多个子user namespace。 当在一个进程中调用unshare或者clone创建新的user namespace时，当前进程原来所在的user namespace为父user namespace，新的user namespace为子user namespace。

在不同的user namespace中，同样一个用户的user ID 和group ID可以不一样，换句话说，一个用户可以在父user namespace中是普通用户，在子user namespace中是超级用户（超级用户只相对于子user namespace所拥有的资源，无法访问其他user namespace中需要超级用户才能访问资源）。

### 创建user namespace

```shell
#--------------------------第一个shell窗口----------------------
#先记录下目前的id，gid和user namespace
dev@ubuntu:~$ id
uid=1000(dev) gid=1000(dev) groups=1000(dev),4(adm),24(cdrom),27(sudo)
dev@ubuntu:~$ readlink /proc/$$/ns/user
user:[4026531837]

#创建新的user namespace
dev@ubuntu:~$ unshare --user /bin/bash
nobody@ubuntu:~$ readlink /proc/$$/ns/user
user:[4026532464]
nobody@ubuntu:~$ id
uid=65534(nobody) gid=65534(nogroup) groups=65534(nogroup)
```

很奇怪，为什么上面例子中显示的用户名是nobody，它的id和gid都是65534？

这是因为我们还没有映射父user namespace的user ID和group ID到子user namespace中来，这一步是必须的，因为这样系统才能控制一个user namespace里的用户在其他user namespace中的权限。（比如给其他user namespace中的进程发送信号，或者访问属于其他user namespace挂载的文件）

如果没有映射的话，当在新的user namespace中用getuid()和getgid()获取user id和group id时，系统将返回文件/proc/sys/kernel/overflowuid中定义的user ID以及proc/sys/kernel/overflowgid中定义的group ID，它们的默认值都是65534。也就是说如果没有指定映射关系的话，会默认映射到ID65534。

下面看看这个user能干些什么?

```shell
#--------------------------第一个shell窗口----------------------
#ls的结果显示/root目录属于nobody
nobody@ubuntu:~$ ls -l /|grep root
drwx------   3 nobody nogroup  4096 7月   8 18:39 root
#但是当前的nobody账号访问不了，说明这两个nobody不是一个ID，他们之间没有映射关系
nobody@ubuntu:~$ ls /root
ls: cannot open directory '/root': Permission denied

#这里显示/home/dev目录属于nobody
nobody@ubuntu:~$ ls -l /home/
drwxr-xr-x 11 nobody nogroup 4096 7月   8 18:40 dev
#touch成功，说明虽然没有显式的映射ID，但还是能访问父user namespace里dev账号拥有的资源
#说明他们背后还是有映射关系
nobody@ubuntu:~$ touch /home/dev/temp01
nobody@ubuntu:~$
```

### 映射user ID和group ID

通常情况下，创建新的user namespace后，第一件事就是映射user和group ID. 映射ID的方法是添加配置到/proc/PID/uid_map和/proc/PID/gid_map（这里的PID是新user namespace中的进程ID，刚开始时这两个文件都是空的）.
![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051638835.png)
这两个文件里面的配置格式如下（可以有多条)：

> ID-inside-ns   ID-outside-ns   length
> 举个例子, 0 1000 256这条配置就表示父user namespace中的1000~1256映射到新user namespace中的0~256。
> 系统默认的user namespace没有父user namespace，但为了保持一致，kernel提供了一个虚拟的uid和gid map文件，看起来是这样子的:
> dev@ubuntu:~$ cat /proc/$$/uid_map
> 0 0 4294967295

那么谁可以向这个文件中写配置呢？

/proc/PID/uid_map和/proc/PID/gid_map的拥有者是创建新user namespace的这个user，所以和这个user在一个user namespace的root账号可以写。但这个user自己有没有写map文件权限还要看它有没有CAP_SETUID和CAP_SETGID的capability。

> 注意：只能向map文件写一次数据，但可以一次写多条，并且最多只能5条

原来的Linux就分root和非root，很多操作只能root完成，比如修改一个文件的owner，后来Linux将root的一些权限分解了，变成了各种capability，只要拥有了相应的capability，就能做相应的操作，不需要root账户的权限。

下面我们来看看如何用dev账号映射uid和gid

```shell
#--------------------------第一个shell窗口----------------------
#获取当前bash的pid
nobody@ubuntu:~$ echo $$
24126

#--------------------------第二个shell窗口----------------------
#dev是map文件的owner
dev@ubuntu:~$ ls -l /proc/24126/uid_map /proc/24126/gid_map
-rw-r--r-- 1 dev dev 0 7月  24 23:11 /proc/24126/gid_map
-rw-r--r-- 1 dev dev 0 7月  24 23:11 /proc/24126/uid_map

#但还是没有权限写这个文件
dev@ubuntu:~$ echo '0 1000 100' > /proc/24126/uid_map
bash: echo: write error: Operation not permitted
dev@ubuntu:~$ echo '0 1000 100' > /proc/24126/gid_map
bash: echo: write error: Operation not permitted
#当前用户运行的bash进程没有CAP_SETUID和CAP_SETGID的权限
dev@ubuntu:~$ cat /proc/$$/status | egrep 'Cap(Inh|Prm|Eff)'
CapInh: 0000000000000000
CapPrm: 0000000000000000
CapEff: 0000000000000000

#为/binb/bash设置capability，
dev@ubuntu:~$ sudo setcap cap_setgid,cap_setuid+ep /bin/bash
#重新加载bash以后我们看到相应的capability已经有了
dev@ubuntu:~$ exec bash
dev@ubuntu:~$ cat /proc/$$/status | egrep 'Cap(Inh|Prm|Eff)'
CapInh: 0000000000000000
CapPrm: 00000000000000c0
CapEff: 00000000000000c0


#再试一次写map文件，成功了
dev@ubuntu:~$ echo '0 1000 100' > /proc/24126/uid_map
dev@ubuntu:~$ echo '0 1000 100' > /proc/24126/gid_map
dev@ubuntu:~$
#再写一次就失败了，因为这个文件只能写一次
dev@ubuntu:~$ echo '0 1000 100' > /proc/24126/uid_map
bash: echo: write error: Operation not permitted
dev@ubuntu:~$ echo '0 1000 100' > /proc/24126/gid_map
bash: echo: write error: Operation not permitted

#后续测试不需要CAP_SETUID了，将/bin/bash的capability恢复到原来的设置
dev@ubuntu:~$ sudo setcap cap_setgid,cap_setuid-ep /bin/bash
dev@ubuntu:~$ getcap /bin/bash
/bin/bash =

#--------------------------第一个shell窗口----------------------
#回到第一个窗口，id已经变成0了，说明映射成功
nobody@ubuntu:~$ id
uid=0(root) gid=0(root) groups=0(root),65534(nogroup)

#--------------------------第二个shell窗口----------------------
#回到第二个窗口，确认map文件的owner，这里24126是新user namespace中的bash
dev@ubuntu:~$ ls -l /proc/24126/
......
-rw-r--r-- 1 dev dev 0 7月  24 23:13 gid_map
dr-x--x--x 2 dev dev 0 7月  24 23:10 ns
-rw-r--r-- 1 dev dev 0 7月  24 23:13 uid_map
......

#--------------------------第一个shell窗口----------------------
#重新加载bash，提示有root权限了
nobody@ubuntu:~$ exec bash
root@ubuntu:~#

#0000003fffffffff表示当前运行的bash拥有所有的capability
root@ubuntu:~# cat /proc/$$/status | egrep 'Cap(Inh|Prm|Eff)'
CapInh: 0000000000000000
CapPrm: 0000003fffffffff
CapEff: 0000003fffffffff
#--------------------------第二个shell窗口----------------------
#回到第二个窗口，发现owner已经变了，变成了root
#目前还不清楚为什么有这样的机制
# 新版本内核并无该问题
dev@ubuntu:~$ ls -l /proc/24126/
......
-rw-r--r-- 1 root root 0 7月  24 23:13 gid_map
dr-x--x--x 2 root root 0 7月  24 23:10 ns
-rw-r--r-- 1 root root 0 7月  24 23:13 uid_map
......
#虽然不能看目录里有哪些文件，但是可以读里面文件的内容
# 新版本内核并无该问题
dev@ubuntu:~$ ls -l /proc/24126/ns
ls: cannot open directory '/proc/24126/ns': Permission denied
dev@ubuntu:~$ readlink /proc/24126/ns/user
user:[4026532464]


#--------------------------第一个shell窗口----------------------
#和第二个窗口一样的结果
# 新版本内核并无该问题
root@ubuntu:~# ls -l /proc/24126/ns
ls: cannot open directory '/proc/24126/ns': Permission denied
root@ubuntu:~# readlink /proc/24126/ns/user
user:[4026532464]

#仍然不能访问/root目录，因为他的拥有着是nobody
root@ubuntu:~# ls -l /|grep root
drwx------   3 nobody nogroup  4096 7月   8 18:39 root
root@ubuntu:~# ls /root
ls: cannot open directory '/root': Permission denied

#对于原来/home/dev下的内容，显示的owner已经映射过来了，由dev变成了新namespace中的root，
#当前root用户可以访问他里面的内容
root@ubuntu:~# ls -l /home
drwxr-xr-x 8 root root 4096 7月  21 18:35 dev
root@ubuntu:~# touch /home/dev/temp01
root@ubuntu:~#

#试试设置主机名称
root@ubuntu:~# hostname container001
hostname: you must be root to change the host name
#修改失败，说明这个新user namespace中的root账号在父user namespace里面不好使
#这也正是user namespace所期望达到的效果，当访问其他user namespace里的资源时，
#是以其他user namespace中的相应账号的权限来执行的，
#比如这里root对应父user namespace的账号是dev，所以改不了系统的hostname
```

那是不是把系统默认user namespace的root账号映射到新的user namespace中，新user namespace的root就可以修改默认user namespace中的hostname呢？

```shell
#--------------------------第三个shell窗口----------------------
#重新打开一个窗口
#这里不再手动映射uid和gid，而是利用unshare命令的-r参数来帮我们完成映射，
#指定-r参数后，unshare将会帮助我们将当前运行unshare的账号映射成新user namesapce的root账号
#这里用了sudo，目的是让root账号来运行unshare命令，
#这样就将外面的root账号映射成新user namespace的root账号
dev@ubuntu:~$ sudo unshare --user -r /bin/bash
root@ubuntu:~# id
uid=0(root) gid=0(root) groups=0(root)

#确认是用root映射root
root@ubuntu:~# echo $$
24283
root@ubuntu:~# cat /proc/24283/uid_map
         0          0          1
root@ubuntu:~# cat /proc/24283/gid_map
         0          0          1

#可以访问/root目录下的东西，但无法操作/home/dev/下的文件
root@ubuntu:~# ls -l / |grep root$
drwx------   6 root root  4096 8月  14 23:11 root
root@ubuntu:~# touch /root/temp01
root@ubuntu:~# ls -l /home
drwxr-xr-x 11 nobody nogroup 4096 8月  14 23:13 dev
root@ubuntu:~# touch /home/dev/temp01
touch: cannot touch '/home/dev/temp01': Permission denied

#尝试修改hostname，还是失败
root@ubuntu:~# hostname container001
hostname: you must be root to change the host name
```

上面的例子中虽然是将root账号映射到了新user namespace的root账号上，但修改hostname、访问/home/dev下的文件依然失败，那是因为不管怎么映射，当用子user namespace的账号访问父user namespace的资源的时候，它启动的进程的capability都为空，所以这里子user namespace的root账号到父namespace中就相当于一个普通的账号。







## Network Namespace

network namespace用来隔离网络设备, IP地址, 端口等. 每个namespace将会有自己独立的网络栈，路由表，防火墙规则，socket等。

每个新的network namespace默认有一个本地环回接口，除了lo接口外，所有的其他网络设备（物理/虚拟网络接口，网桥等）只能属于一个network namespace。每个socket也只能属于一个network namespace。

### 示例

同一个主机内的不同 namespace 之间的通信，以及 namespace 和主机的通信，需要借助网桥和 veth-pair 实现。
![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639300.png)

* 创建命名空间

```shell
ip netns add ns1
ip netns add ns2
```

* 创建网桥

```shell
ip link add name bridge0 type bridge
```

* 创建 veth-pair

```shell
ip link add veth1 type veth peer name vethb1
ip link add veth2 type veth peer name vethb2
```

* 将 veth 的一端放在网桥上

```shell
ip link set vethb1 master bridge0
ip link set vethb2 master bridge0
```

* 将 veth 另一端放在 namespace

```shell
ip link set veth1 netns ns1
ip link set veth2 netns ns2
```

* 给网络设备配置 ip 地址

```shell
ip netns exec ns1 ip addr add 10.1.1.2/24 dev veth1
ip netns exec ns2 ip addr add 10.1.1.3/24 dev veth2
ip addr add 10.1.1.1/24 dev bridge0
```

* 启动网络设备

```shell
ip link set bridge0 up
ip link set vethb1  up
ip link set vethb2  up
ip netns exec ns1 ip link set veth1 up
ip netns exec ns2 ip link set veth2 up
```

相互间可以ping通
![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639442.png)


# 容器支撑技术-cgroup

![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639021.png)

## cgroup的作用

cgroup代表control group，group这个集合的主要目的在于控制资源的使用：多个进程作为一个整体享有资源的配额（quota），同时接受资源的限制。
cgroup提供了对一组进程及将来的子进程的资源限制的能力。

## cgroup组成

Cgroups主要由task,cgroup,subsystem及hierarchy构成。下面分别介绍下各自的概念。

* Task : 在Cgroups中，task就是系统的一个进程。
* Cgroup : Cgroups中的资源控制都以cgroup为单位实现的。cgroup表示按照某种资源控制标准划分而成的任务组，包含一个或多个Subsystems。一个任务可以加入某个cgroup，也可以从某个cgroup迁移到另外一个cgroup。
* Subsystem : Cgroups中的subsystem就是一个资源调度控制器（Resource Controller）。比如CPU子系统可以控制CPU时间分配，内存子系统可以限制cgroup内存使用量。
* Hierarchy : hierarchy由一系列cgroup以一个树状结构排列而成，每个hierarchy通过绑定对应的subsystem进行资源调度。hierarchy中的cgroup节点可以包含零或多个子节点，子节点继承父节点的属性。整个系统可以有多个hierarchy。

## cgroup规则

  主要介绍Subsystems, Hierarchies,Control Group和Tasks之间组织结构和规则：

**规则一：**
       同一个hierarchy能够附加一个或多个subsystem。如 cpu 和 memory subsystems(或者任意多个subsystems)附加到同一个hierarchy。

**规则二：**
       一个 subsystem 可以附加到多个 hierarchy，当且仅当这些 hierarchy 只有这唯一一个 subsystem。即某个hierarchy（hierarchy A）中的subsystem（如CPU）不能附加到已经附加了其他subsystem的hierarchy（如hierarchy B）中。也就是说已经附加在某个 hierarchy 上的 subsystem 不能附加到其他含有别的 subsystem 的 hierarchy 上。

**规则三：**
       系统每次新建一个hierarchy时，该系统上的所有task默认构成了这个新建的hierarchy的初始化cgroup，这个cgroup也称为root cgroup。对于你创建的每个hierarchy，task只能存在于其中一个cgroup中，即一个task不能存在于同一个hierarchy的不同cgroup中，但是一个task可以存在在不同hierarchy中的多个cgroup中。如果操作时把一个task添加到同一个hierarchy中的另一个cgroup中，则会从第一个cgroup中移除
如：
cpu 和 memory subsystem被附加到 cpu_mem_cg 的hierarchy。而 net_cls subsystem被附加到 net_cls hierarchy。并且httpd进程被同时加到了 cpu_mem_cg hierarchy的 cg1 cgroup中和 net hierarchy的 cg2 cgroup中。并通过两个hierarchy的subsystem分别对httpd进程进行cpu,memory及网络带宽的限制。

**规则四：**
       进程（task）在 fork 自身时创建的子任务（child task）默认与原 task 在同一个 cgroup 中，但是 child task 允许被移动到不同的 cgroup 中。即 fork 完成后，父子进程间是完全独立的。




## cgroup子系统

目前Linux支持下面12种subsystem

* cpu (since Linux 2.6.24; CONFIG_CGROUP_SCHED)
  用来限制cgroup的CPU使用率。

* cpuacct (since Linux 2.6.24; CONFIG_CGROUP_CPUACCT)
  统计cgroup的CPU的使用率。

* cpuset (since Linux 2.6.24; CONFIG_CPUSETS)
  绑定cgroup到指定CPUs和NUMA节点。

* memory (since Linux 2.6.25; CONFIG_MEMCG)
  统计和限制cgroup的内存的使用率，包括process memory, kernel memory, 和swap。

* devices (since Linux 2.6.26; CONFIG_CGROUP_DEVICE)
  限制cgroup创建(mknod)和访问设备的权限。

* freezer (since Linux 2.6.28; CONFIG_CGROUP_FREEZER)
  suspend和restore一个cgroup中的所有进程。

* net_cls (since Linux 2.6.29; CONFIG_CGROUP_NET_CLASSID)
  将一个cgroup中进程创建的所有网络包加上一个classid标记，用于tc和iptables。 只对发出去的网络包生效，对收到的网络包不起作用。

* blkio (since Linux 2.6.33; CONFIG_BLK_CGROUP)
  限制cgroup访问块设备的IO速度。

* perf_event (since Linux 2.6.39; CONFIG_CGROUP_PERF)
  对cgroup进行性能监控

* net_prio (since Linux 3.3; CONFIG_CGROUP_NET_PRIO)
  针对每个网络接口设置cgroup的访问优先级。

* hugetlb (since Linux 3.5; CONFIG_CGROUP_HUGETLB)
  限制cgroup的huge pages的使用量。

* pids (since Linux 4.3; CONFIG_CGROUP_PIDS)
  限制一个cgroup及其子孙cgroup中的总进程数。



## 示例

systemd已经帮我们将memory绑定到了/sys/fs/cgroup/memory，如下：

```shell
dev@dev:~$ mount|grep memory
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
```

### 创建子cgroup

在/sys/fs/cgroup/memory下创建一个子目录即创建了一个子cgroup

```shell
#--------------------------第一个shell窗口----------------------
dev@dev:~$ cd /sys/fs/cgroup/memory
dev@dev:/sys/fs/cgroup/memory$ sudo mkdir test
dev@dev:/sys/fs/cgroup/memory$ ls test
cgroup.clone_children  memory.kmem.failcnt             memory.kmem.tcp.limit_in_bytes      memory.max_usage_in_bytes        memory.soft_limit_in_bytes  notify_on_release
cgroup.event_control   memory.kmem.limit_in_bytes      memory.kmem.tcp.max_usage_in_bytes  memory.move_charge_at_immigrate  memory.stat                 tasks
cgroup.procs           memory.kmem.max_usage_in_bytes  memory.kmem.tcp.usage_in_bytes      memory.numa_stat                 memory.swappiness
memory.failcnt         memory.kmem.slabinfo            memory.kmem.usage_in_bytes          memory.oom_control               memory.usage_in_bytes
memory.force_empty     memory.kmem.tcp.failcnt         memory.limit_in_bytes               memory.pressure_level            memory.use_hierarchy
```

从上面ls的输出可以看出，除了每个cgroup都有的那几个文件外，和memory相关的文件还不少，每个文件的作用如下：=

```shell
 cgroup.event_control       #用于eventfd的接口
 memory.usage_in_bytes      #显示当前已用的内存
 memory.limit_in_bytes      #设置/显示当前限制的内存额度
 memory.failcnt             #显示内存使用量达到限制值的次数
 memory.max_usage_in_bytes  #历史内存最大使用量
 memory.soft_limit_in_bytes #设置/显示当前限制的内存软额度
 memory.stat                #显示当前cgroup的内存使用情况
 memory.use_hierarchy       #设置/显示是否将子cgroup的内存使用情况统计到当前cgroup里面
 memory.force_empty         #触发系统立即尽可能的回收当前cgroup中可以回收的内存
 memory.pressure_level      #设置内存压力的通知事件，配合cgroup.event_control一起使用
 memory.swappiness          #设置和显示当前的swappiness
 memory.move_charge_at_immigrate #设置当进程移动到其他cgroup中时，它所占用的内存是否也随着移动过去
 memory.oom_control         #设置/显示oom controls相关的配置
 memory.numa_stat           #显示numa相关的内存
```

### 添加进程

```shell
#--------------------------第二个shell窗口----------------------
#重新打开一个shell窗口，避免相互影响
dev@dev:~$ cd /sys/fs/cgroup/memory/test/
dev@dev:/sys/fs/cgroup/memory/test$ echo $$
4589
dev@dev:/sys/fs/cgroup/memory/test$ sudo sh -c "echo $$ >> cgroup.procs"
#运行top命令，这样这个cgroup消耗的内存会多点，便于观察
dev@dev:/sys/fs/cgroup/memory/test$ top
#后续操作不再在这个窗口进行，避免在这个bash中运行进程影响cgropu里面的进程数及相关统计
```

### 设置限额

设置限额很简单，写文件memory.limit_in_bytes就可以了，请仔细看示例

```shell
#--------------------------第一个shell窗口----------------------
#回到第一个shell窗口
dev@dev:/sys/fs/cgroup/memory$ cd test
#这里两个进程id分别时第二个窗口的bash和top进程
dev@dev:/sys/fs/cgroup/memory/test$ cat cgroup.procs
4589
4664
#开始设置之前，看看当前使用的内存数量，这里的单位是字节
dev@dev:/sys/fs/cgroup/memory/test$ cat memory.usage_in_bytes
835584

#设置1M的限额
dev@dev:/sys/fs/cgroup/memory/test$ sudo sh -c "echo 1M > memory.limit_in_bytes"
#设置完之后记得要查看一下这个文件，因为内核要考虑页对齐, 所以生效的数量不一定完全等于设置的数量
dev@dev:/sys/fs/cgroup/memory/test$ cat memory.limit_in_bytes
1048576

#如果不再需要限制这个cgroup，写-1到文件memory.limit_in_bytes即可
dev@dev:/sys/fs/cgroup/memory/test$ sudo sh -c "echo -1 > memory.limit_in_bytes"
#这时可以看到limit被设置成了一个很大的数字
dev@dev:/sys/fs/cgroup/memory/test$ cat memory.limit_in_bytes
9223372036854771712
```

如果设置的限额比当前已经使用的内存少呢？如上面显示当前bash用了800多k，如果我设置limit为400K会怎么样？

```shell
#--------------------------第一个shell窗口----------------------
#先用free看下当前swap被用了多少
dev@dev:/sys/fs/cgroup/memory/test$ free
              total        used        free      shared  buff/cache   available
Mem:         500192       45000       34200        2644      420992      424020
Swap:        524284          16      524268
#设置内存限额为400K
dev@dev:/sys/fs/cgroup/memory/test$ sudo sh -c "echo 400K > memory.limit_in_bytes"

#再看当前cgroup的内存使用情况
#发现内存占用少了很多，刚好在400K以内，原来用的那些内存都去哪了呢？
dev@dev:/sys/fs/cgroup/memory/test$ cat memory.usage_in_bytes
401408

#再看swap空间的占用情况，和刚开始比，多了500-16=384K，说明内存中的数据被移到了swap上
dev@dev:/sys/fs/cgroup/memory/test$ free
              total        used        free      shared  buff/cache   available
Mem:         500192       43324       35132        2644      421736      425688
Swap:        524284         500      523784

#这个时候再来看failcnt，发现有453次之多(隔几秒再看这个文件，发现次数在增长)
dev@dev:/sys/fs/cgroup/memory/test$ cat memory.failcnt
453

#再看看memory.stat（这里只显示部分内容），发现物理内存用了400K，
#但有很多pgmajfault以及pgpgin和pgpgout，说明发生了很多的swap in和swap out
dev@dev:/sys/fs/cgroup/memory/test$ cat memory.stat
rss 409600
total_pgpgin 4166
total_pgpgout 4066
total_pgfault 7558
total_pgmajfault 419

#从上面的结果可以看出，当物理内存不够时，就会触发memory.failcnt里面的数量加1，
#但进程不会被kill掉，那是因为内核会尝试将物理内存中的数据移动到swap空间中，从而让内存分配成功
```

如果设置的限额过小，就算swap out部分内存后还是不够会怎么样？

```shell
#--------------------------第一个shell窗口----------------------
dev@dev:/sys/fs/cgroup/memory/test$ sudo sh -c "echo 1K > memory.limit_in_bytes"
#进程已经不在了（第二个窗口已经挂掉了）
dev@dev:/sys/fs/cgroup/memory/test$ cat cgroup.procs
dev@dev:/sys/fs/cgroup/memory/test$ cat memory.usage_in_bytes
0
#从这里的结果可以看出，第二个窗口的bash和top都被kill掉了
```

从上面的这些测试可以看出，一旦设置了内存限制，将立即生效，并且当物理内存使用量达到limit的时候，memory.failcnt的内容会加1，但这时进程不一定就会被kill掉，内核会尽量将物理内存中的数据移到swap空间上去，如果实在是没办法移动了（设置的limit过小，或者swap空间不足），默认情况下，就会kill掉cgroup里面继续申请内存的进程。

## 参考资料

[红帽cgroup资料](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/ch-using_control_groups)


# docker支撑技术-Union FS 

```shell
# mount -t aufs -o br=./Branch-0:./Branch-1:./Branch-2 none ./MountPoint
```

* -t aufs： 指定挂载类型为aufs
* -o br=./Branch-0:./Branch-1:./Branch-2： 表示将当前目录下的Branch-0，Branch-1，Branch-2三个文件夹联合到一起
* none：aufs不需要设备，只依赖于-o br指定的文件夹，所以这里填none即可
* /MountPoint：表示将最后联合的结果挂载到当前的MountPoint目录下，然后我们就可以往这个目录里面读写文件了
  假设Branch-0里面有文件001.txt、003.txt，Branch-1里面有文件001.txt、003.txt、004.txt，Branch-2里面有文件002.txt、003.txt。

mount完成后，得到的结果将会如下图所示

```shell
              /*001.txt(b0)表示Branch-0的001.txt文件，其它的以此类推*/
           +-------------+-------------+-------------+-------------+
MountPoint | 001.txt(b0) | 002.txt(b2) | 003.txt(b0) | 004.txt(b1) |    
           +-------------+-------------+-------------+-------------+
                  ↑             ↑             ↑             ↑
                  |             |             |             |
           +-------------+-------------+-------------+-------------+
Branch-0   |   001.txt   |             |   003.txt   |             |
           +-------------+-------------+-------------+-------------+
Branch-1   |   001.txt   |             |   003.txt   |   004.txt   |
           +-------------+-------------+-------------+-------------+
Branch-2   |             |   002.txt   |   003.txt   |             |
           +-------------+-------------+-------------+-------------+
```

联合之后，在MountPoint下将会看到四个文件，分别是Branch-0下的001.txt、003.txt，Branch-1下的04.txt，以及Branch-2下的002.txt。

# Docker镜像

## 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时的库、环境变量和配置文件。

## Docker镜像加载原理

> Unions （联合文件系统）

Unions （联合文件系统）：Unions 是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。

联合文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639291.png)


**特性：**一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。  

## 镜像分层

> 镜像为什么会产生分层的结构

最大的好处，我觉得莫过于资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享 。

所有的 Docker镜像都起始于一个基础镜像层，当进行修改或添加新的内容时，就会在当前镜像层之上，创建新的镜像层。
举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像当前已经包含3个镜像层，如下图所示。

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639986.png)
在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了
一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。  
![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639430.png)
上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件
下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版  
![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639232.png)
这种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中。
Docker通过存储引擎来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。
Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的文件系统或者块设备技术，并且每种存储引擎都有其独有的性能特点。

下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图。  

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051639385.png)
Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！
这一层就是我们通常说的容器层，容器之下的都叫镜像层！  

## commit镜像

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640263.png)

# docker镜像管理

* registry

镜像仓库，用户可以构建自己的私有仓库

* repository

repository是一个镜像集合，其中包含多个版本的镜像，使用标签可以进行版本区分。

* image

image用来存储镜像相关的元数据，主要包括镜像的架构、镜像默认配置信息、rootfs等

* layer

镜像层元数据，保存着每一层镜像的实际内容，单个layer可被多个镜像共享



## 镜像本地管理

```shell
root@ubuntu /v/l/d/i/overlay2# pwd
/var/lib/docker/image/overlay2
root@ubuntu /v/l/d/i/overlay2# ls
distribution/  imagedb/  layerdb/  repositories.json
```

docker pull所下载的镜像都存储在这个目录下（以overlay2为例）。

### repositories.json

repositories.json中记录了和本地image相关的repository信息，主要是name和image id的对应关系，当image从registry上被pull下来后，就会更新该文件：

```shell
root@ubuntu /v/l/d/i/overlay2# cat repositories.json | python -m json.tool
{
    "Repositories": {
        "busybox": {  # 镜像的名字
            "busybox:latest": "sha256:beae173ccac6ad749f76713cf4440fe3d21d1043fe616dfbe30775815d1d0f6a",
            "busybox@sha256:5acba83a746c7608ed544dc1533b87c737a0b0fb730301639a0179f9344b1678": "sha256:beae173ccac6ad749f76713cf4440fe3d21d1043fe616dfbe30775815d1d0f6a"
        },

```

每一个repository里不同版本对应的镜像id。找到镜像id后，就可以查看镜像内容，如下所示：

```shell
root@ubuntu /v/l/d/i/o/i/c/sha256# cat beae173ccac6ad749f76713cf4440fe3d21d1043fe616dfbe30775815d1d0f6a | python -m json.tool
{
    "architecture": "amd64",
    "config": {
        "Hostname": "",
        "Domainname": "",
        "User": "",
        "AttachStdin": false,
        "AttachStdout": false,
        "AttachStderr": false,
        "Tty": false,
        "OpenStdin": false,
        "StdinOnce": false,
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
        ],
        "Cmd": [
            "sh"
        ],
        "Image": "sha256:da658412c37aa24e561eb7e16c61bc82a9711340d8fb5cf1a8f39d8e96d7f723",
        "Volumes": null,
        "WorkingDir": "",
        "Entrypoint": null,
        "OnBuild": null,
        "Labels": null
    },
    "container": "a0007fa726185ffbcb68e90f8edabedd79a08949f32f4f0bcc6e5fed713a72c8",
    "container_config": {
        "Hostname": "a0007fa72618",
        "Domainname": "",
        "User": "",
        "AttachStdin": false,
        "AttachStdout": false,
        "AttachStderr": false,
        "Tty": false,
        "OpenStdin": false,
        "StdinOnce": false,
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
        ],
        "Cmd": [
            "/bin/sh",
            "-c",
            "#(nop) ",
            "CMD [\"sh\"]"
        ],
        "Image": "sha256:da658412c37aa24e561eb7e16c61bc82a9711340d8fb5cf1a8f39d8e96d7f723",
        "Volumes": null,
        "WorkingDir": "",
        "Entrypoint": null,
        "OnBuild": null,
        "Labels": {}
    },
    "created": "2021-12-30T19:19:41.006954958Z",
    "docker_version": "20.10.7",
    "history": [
        {
            "created": "2021-12-30T19:19:40.833034683Z",
            "created_by": "/bin/sh -c #(nop) ADD file:6db446a57cbd2b7f4cfde1f280177b458390ed5a6d1b54c6169522bc2c4d838e in / "
        },
        {
            "created": "2021-12-30T19:19:41.006954958Z",
            "created_by": "/bin/sh -c #(nop)  CMD [\"sh\"]",
            "empty_layer": true
        }
    ],
    "os": "linux",
    "rootfs": {
        "type": "layers",
        "diff_ids": [
            "sha256:01fd6df81c8ec7dd24bbbd72342671f41813f992999a3471b9d9cbc44ad88374"
        ]
    }
}
```

其中我们可以看到上面提及的镜像架构、镜像配置等，其中我们可以通过rootfs找到镜像对应的layer。busybody镜像只有一层layer。

通过diff-id我们就可以找到cache-id（内容寻址），如下所示：

```shell
root@ubuntu /v/l/d/i/o/l/s/01fd6df81c8ec7dd24bbbd72342671f41813f992999a3471b9d9cbc44ad88374# cat cache-id
0ea32e289a2f802c5b3abce02d8a4fe10dfff3049c4693276153c02da6f2e149⏎
```

找到cache-id，就可以知道这一层镜像实际存放的内容：

```shell
root@ubuntu /v/l/d/o/0ea32e289a2f802c5b3abce02d8a4fe10dfff3049c4693276153c02da6f2e149# ls diff/
bin/  dev/  etc/  home/  root/  tmp/  usr/  var/
root@ubuntu /v/l/d/o/0ea32e289a2f802c5b3abce02d8a4fe10dfff3049c4693276153c02da6f2e149# cat link
RBOMEOEKWMWD37H4SKJRUNYWK2⏎                                                                                                                                                                                   root@ubuntu /v/l/d/o/0ea32e289a2f802c5b3abce02d8a4fe10dfff3049c4693276153c02da6f2e149# pwd
/var/lib/docker/overlay2/0ea32e289a2f802c5b3abce02d8a4fe10dfff3049c4693276153c02da6f2e149
```

以上示例是只有一层镜像的情况，对于多层镜像需要通过计算chain-id，得到cache-id。

计算chainid时，用到了所有祖先layer的信息，从而能保证根据chainid得到的rootfs是唯一的。比如我在debian和ubuntu的image基础上都添加了一个同样的文件，那么commit之后新增加的这两个layer具有相同的内容，相同的diffid，但由于他们的父layer不一样，所以他们的chainid会不一样，从而根据chainid能找到唯一的rootfs。

`chainID 需通过公式 chainID(n) = SHA256(chain(n-1) diffID(n)) 计算得到`

```shell
[root@k8s-master-node-1 layerdb]# echo -n "sha256:ccdbb80308cc5ef43b605ac28fac29c6a597f89f5a169bbedbb8dec29c987439 sha256:63c99163f47292f80f9d24c5b475751dbad6dc795596e935c5c7f1c73dc08107" | sha256sum -
8d8dceacec7085abcab1f93ac1128765bc6cf0caac334c821e01546bd96eb741  -

[root@k8s-master-node-1 8d8dceacec7085abcab1f93ac1128765bc6cf0caac334c821e01546bd96eb741]# ls
cache-id  diff  parent  size  tar-split.json.gz
[root@k8s-master-node-1 8d8dceacec7085abcab1f93ac1128765bc6cf0caac334c821e01546bd96eb741]# cat cache-id
4d615a437c68f0853db7749bf3d7d268efaebbe045a2af4d8b8e1148fc1acd91
```



## docker容器与镜像

```shell
root@ubuntu /v/l/d/o/0ea32e289a2f802c5b3abce02d8a4fe10dfff3049c4693276153c02da6f2e149# mount | grep overlay
overlay on /var/lib/docker/overlay2/c982ec89c32ca002642cee045d28afcea7659f4bf0ef6eb0ef73ac00800c0b07/merged type overlay (rw,relatime,lowerdir=/var/lib/docker/overlay2/l/6MZ24T7INBRKWZP6KQNKLY2HJL:/var/lib/docker/overlay2/l/RBOMEOEKWMWD37H4SKJRUNYWK2,upperdir=/var/lib/docker/overlay2/c982ec89c32ca002642cee045d28afcea7659f4bf0ef6eb0ef73ac00800c0b07/diff,workdir=/var/lib/docker/overlay2/c982ec89c32ca002642cee045d28afcea7659f4bf0ef6eb0ef73ac00800c0b07/work)
```

可以看到，启动容器会 mount 一个 overlay 的联合文件系统到容器内。这个文件系统由三层组成：

- lowerdir：只读层，即为镜像的镜像层，为layer的短id
- upperdir：读写层，该层是容器的读写层，对容器的读写操作将反映在读写层。
- workdir： overlayfs 的内部层，用于实现从只读层到读写层的 copy_up 操作。
- merge：容器内作为同一视图联合挂载点的目录。




# Docker容器卷

## 什么是容器卷

如果数据都在容器中，那么我们容器删除，数据就会丢失！需求：数据可以持久化  

容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！
这就是卷技术！目录的挂载，将我们容器内的目录，挂载到Linux上面！  



## 使用容器卷

* 直接使用命令挂载 -v

  ```shel
  docker run -it -v 主机目录:容器内目录
  
  # 通过 docker inspect 容器ID 查看挂载信息
  ```

* 容器卷挂载方式

```shell
# 匿名挂载
docker run -d --name niming -v /home/ljt ubuntu

# 具名挂载
docker run -d --name juming -v JuMing:/home/juming ubuntu

# 指定路径挂载， 这种挂载方式docker volume ls是找不到的
docker run -d --name lujing -v /home/docker/volume:/home/juming ubuntu

# 查看卷信息
howu@howu-ubuntu:~/sambaDoc$ docker volume ls
DRIVER              VOLUME NAME
local               JuMing
local               f0e64f8aba5f9a68765b8a4dc2ba9576d04a83d94b1f3b6ee4ce7684284f2c8e

howu@howu-ubuntu:~/sambaDoc$ docker volume inspect JuMing
[
    {
        "CreatedAt": "2020-11-29T18:04:59+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/JuMing/_data",
        "Name": "JuMing",
        "Options": null,
        "Scope": "local"
    }
]
```

* 注意事项

```shell
# 通过 -v 容器内路径： ro rw 改变读写权限
ro #readonly 只读
rw #readwrite 可读可写
docker run -d -P --name nginx05 -v juming:/etc/nginx:ro nginx
docker run -d -P --name nginx05 -v juming:/etc/nginx:rw nginx
# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部是无法操作！
```



## 初识Dockerfile

DockerFile用来构建镜像的构建文件，是一个命令脚本，通过这个脚本可以生成镜像。

**实验：**通过DockerFile自己创建一个镜像，实现数据卷挂载

```shell
#dockerfile脚本
FROM centos

VOLUME ["volume01","volume01"]

CMD echo "----end----"
CMD /bin/bash
```

执行后的结果：

```shell
howu@howu-ubuntu ~/s/d/dockerfile> docker build -f /home/howu/sambaDoc/docker/dockerfile/dockerfile_myubuntu  -t ljt/ubuntu:1.0 .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 0d120b6ccaa8
Step 2/4 : VOLUME ["volume01","volume01"]
 ---> Running in 6a8eb0a835e1
Removing intermediate container 6a8eb0a835e1
 ---> 08f163e4eebd
Step 3/4 : CMD echo "----end----"
 ---> Running in aaf30891f017
Removing intermediate container aaf30891f017
 ---> 13d730977a6d
Step 4/4 : CMD /bin/bash
 ---> Running in 29b3539587b9
Removing intermediate container 29b3539587b9
 ---> c8894d001c35
Successfully built c8894d001c35
Successfully tagged ljt/ubuntu:1.0
howu@howu-ubuntu ~/s/d/dockerfile> docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ljt/ubuntu          1.0                 c8894d001c35        22 seconds ago      215MB
ubuntu-vim          latest              c1bcb16c3615        7 days ago          167MB
ubuntu              latest              f643c72bc252        10 days ago         72.9MB
centos              latest              0d120b6ccaa8        3 months ago        215MB
hello-world         latest              bf756fb1ae65        11 months ago       13.3kB
```

运行自己创建的镜像：

```shell
howu@howu-ubuntu:~$ docker run -it --name docker1 c8894d001c35
[root@cb10ce51c04a /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume01
```


## 容器间数据卷

**实验：**容器间数据共享  `--volumes-from`

```shell
howu@howu-ubuntu ~/s/d/dockerfile> docker run -it --name docker2 --volumes-from docker1 c8894d001c35
[root@e279f4fdab7e /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume01
[root@e279f4fdab7e /]# cd volume01/
[root@e279f4fdab7e volume01]# ls
ljt.txt
[root@e279f4fdab7e volume01]#
```

`ps`删除其中一个容器，其他容器的数据不会丢失。



# DockerFile

## DockerFile介绍

DockerFile是一个文本格式的配置文件（命令参数脚本），用户可以使用DockerFile来快速创建自定义的镜像。

**构建步骤：**

1、编写一个DockerFile文件

2、docker build构建一个镜像

3、docker run运行镜像

4、docker push发布镜像（DockerHub、阿里云镜像）

很多官方镜像包都是基础包，我们可以通过DockerFile构建自己的镜像。



## DockerFile语法

1、每个保留关键字(指令）都是必须是大写字母
2、执行从上到下顺序
3、#表示注释
4、每一个指令都会创建提交一个新的镜像曾，并提交！  


```shell
# DockerFile常用指令
FROM 				# 基础镜像，一切从这里开始构建
MAINTAINER 			# 镜像是谁写的， 姓名+邮箱
RUN 				# 镜像构建的时候需要运行的命令
ADD 				# 步骤，tomcat镜像，这个tomcat压缩包！添加内容 添加同目录
WORKDIR 			# 镜像的工作目录
VOLUME			 	# 挂载的目录
EXPOSE 				# 保留端口配置
CMD 				# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代。
ENTRYPOINT 			# 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD 			# 当构建一个被继承 DockerFile 这个时候就会运行ONBUILD的指令，触发指令。
COPY 				# 类似ADD，将我们文件拷贝到镜像中
ENV 				# 构建的时候设置环境变量！
```



## 编写DockerFile

```shell
FROM centos
MAINTAINER laijiatao<496272917@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```


> 运行：通过DockerFile文件构建镜像

```shell
# 命令 docker build -f 文件路径 -t 镜像名:[tag] .
docker build -f mydockerfile-centos -t mycentos:0.1 .
```



> CMD与ENTRYPOINT区别

```shell
CMD # 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代。
ENTRYPOINT # 指定这个容器启动的时候要运行的命令，可以追加命令
```

## 小结

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640095.png)


# Docker网络

## 端口映射

在启动容器时，通过加入-p -P参数来指定端口映射，实现容器外部网络访问容器内的网络应用和服务。



## 容器互联 --link

```shell
# 创建两个容器
docker run -d --name tomcat01 tomcat
docker run -d --name tomcat02 tomcat

# 两个容器间ping不通
howu@howu-ubuntu:~$ docker exec -it tomcat02 ping tomcat01
ping: tomcat01: No address associated with hostname

# 使用--link使两个容器互联
howu@howu-ubuntu:~$ docker run -d -P --name tomcat03 --link tomcat02 tomcat
7d3cd10676bf6e43383f47e690650d8f305314d2022919b65b12e2505077d3aa
howu@howu-ubuntu:~$ docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=8.74 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.050 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.045 ms
^C
--- tomcat02 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 16ms
rtt min/avg/max/mdev = 0.045/2.944/8.739/4.097 ms
# 但是用tomcat02 ping不通 tomcat03
```

> 使用docker inspect 容器查看信息

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640250.png)

查看tomcat03里面的/etc/hosts发现有tomcat02的配置
![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640955.png)



> --link的本质就是在hosts配置中添加映射



## 自定义网络

> 使用docker network ls查看docker网络
> ![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640057.png)

**网络模式**

bridge ：桥接 docker（默认，自己创建也是用bridge模式）

none ：不配置网络，一般不用

host ：和所主机共享网络

```shell
# 当直接启动一个容器时，默认是使用bridge，就是我们得docker0
# 自定义一个网络
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
```

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640131.png)


```shell
 docker network inspect mynet
```

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640592.png)


> 使用自定义网络启动两个容器

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640521.png)


> 再次查看mynet网络

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640288.png)


此时两个容器间可以相互ping通。

## 添加容器到自定义网路

```she
howu@howu-ubuntu:~$ docker network connect --help

Usage:  docker network connect [OPTIONS] NETWORK CONTAINER

Connect a container to a network

Options:
      --alias strings           Add network-scoped alias for the container
      --driver-opt strings      driver options for the network
      --ip string               IPv4 address (e.g., 172.30.100.104)
      --ip6 string              IPv6 address (e.g., 2001:db8::33)
      --link list               Add link to another container
      --link-local-ip strings   Add a link-local address for the container
```

```shel
howu@howu-ubuntu:~$ docker exec tomact-mtnet-01 ping tomcat04
PING tomcat04 (192.168.0.4) 56(84) bytes of data.
64 bytes from tomcat04.mynet (192.168.0.4): icmp_seq=1 ttl=64 time=0.184 ms
64 bytes from tomcat04.mynet (192.168.0.4): icmp_seq=2 ttl=64 time=0.052 ms
64 bytes from tomcat04.mynet (192.168.0.4): icmp_seq=3 ttl=64 time=0.081 ms
^C
howu@howu-ubuntu:~$ docker exec tomact-mtnet-02 ping tomcat04
PING tomcat04 (192.168.0.4) 56(84) bytes of data.
64 bytes from tomcat04.mynet (192.168.0.4): icmp_seq=1 ttl=64 time=0.074 ms
64 bytes from tomcat04.mynet (192.168.0.4): icmp_seq=2 ttl=64 time=0.060 ms
^C
howu@howu-ubuntu:~$ docker exec tomcat04 ping tomact-mtnet-02
PING tomact-mtnet-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomact-mtnet-02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.065 ms
64 bytes from tomact-mtnet-02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.055 ms
^C

```

# Docker 运行容器概述

## docker进程关系图

![img](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051640246.png)
以运行hello-world容器为例：

```shell
                              +------------+
                              |            |
                              | Docker Hub |
                              |            |
                              +------------+
                                    ↑
                                    |
                                  2 | REST
                                    |
                                    ↓
                               +---------+
+--------+       REST          |         |    grpc      +-------------------+
| docker |<------------------->| dockerd |<------------>| docker-containerd |
+--------+         1           |         |      3       +-------------------+
                               +---------+                       ↑
                                                                 |
                                                                 | 4
                                                                 ↓
                                                      +------------------------+  5   +-------------+
                                                      | docker-containerd-shim |<---->| docker-runc |
                                                      +------------------------+      +-------------+
                                                                                             ↑
                                                                                             | 6
                                                                                             ↓
                                                                                         +-------+
                                                                                         | hello |
                                                                                         +-------+

```

用`docker run`命令直接创建并运行一个容器，它的背后其实包含独立的两步，一步是`docker create`创建容器，另一步是`docker start`启动容器

## docker creat

### 为容器设置读写层

以容器id为名字创建容器层。

```go
	if err := idtools.MkdirAs(path.Join(dir, "diff"), 0755, rootUID, rootGID); err != nil {
		return err
	}

	lid := generateID(idLength)
	if err := os.Symlink(path.Join("..", id, "diff"), path.Join(d.home, linkDir, lid)); err != nil {
		return err
	}

	// Write link id to link file
	if err := ioutil.WriteFile(path.Join(dir, "link"), []byte(lid), 0644); err != nil {
		return err
	}

	// if no parent directory, done
	if parent == "" {
		return nil
	}

	if err := idtools.MkdirAs(path.Join(dir, "work"), 0700, rootUID, rootGID); err != nil {
		return err
	}
	if err := idtools.MkdirAs(path.Join(dir, "merged"), 0700, rootUID, rootGID); err != nil {
		return err
	}
```





### 创建容器配置文件

docker将用户指定的参数和image配置文件中的部分参数进行合并，然后将合并后生成的容器的配置文件放在/var/lib/docker/containers/下面，目录名称就是容器的ID，主要有两个配置文件：

* 容器相关配置文件config.v2.json

容器id、容器镜像、容器环境变量、容器执行命令、容器网络设置等

* 主机相关配置文件hostconfig.json

网络模式、容器能力、内存大小、cpu配额、ulimit等



## docker start

### 准备rootfs

```go
dir, err := container.RWLayer.Mount(container.GetMountLabel())
```

容器在create阶段，已经准备好所有的layer，在容器start阶段，一开始，就通过mount的方式将所有的layer合并起来

```go
// 合并的层
opts = fmt.Sprintf("lowerdir=%s,upperdir=%s,workdir=%s", string(lowers), path.Join(id, "diff"), path.Join(id, "work"))
// mount
if err := mount("overlay", mountTarget, "overlay", 0, mountData)
```

合并的内容：

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051641040.png)

![请添加图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051641647.png)




### 初始化网络

容器分为4种网络模式：

* host

```go
			//将宿主机的hostname赋值给新建container
			container.Config.Hostname, err = os.Hostname()
```



* container

```go
		//获取所要共享网络的container的hosts文件
		nc, err := daemon.getNetworkedContainer(container.ID, container.HostConfig.NetworkMode.ConnectedContainer())
		if err != nil {
			return err
		}
		//将所要共享网络的container的HostnamePath、HostsPath和ResolvConfPath赋值给新container
		initializeNetworkingPaths(container, nc)
		//将所要共享网络的container的Hostname、Domainname赋值给新container
		container.Config.Hostname = nc.Config.Hostname
		container.Config.Domainname = nc.Config.Domainname
```



* null
* bridge（default）

(1) 若网络模式为container模式，则说明容器与其他容器共用网络。首先找到被引用的容器对象，然后让新容器使用被引用容器的hostname、hosts、resolv.conf文件，并和被引用主机同一个主机名和域名。
(2) 若网络模式为host模式，则将容器的主机名和域名设置为与主机相同。首先调用os. Hostname()获取宿主机的主机名，分离出主机名和域名分别填写到容器Config对应的域中，然后继续执行下一步。
(3) host模式或其他情况则执行allocateNetwork函数，然后创建hostname文件并填入域名和主机名。allocateNetwork函数主要为容器清理遗留的Sandbox，更新NetworkSettings属性，并对每一个容器加入的网络调用connectToNetwork函数。connectToNetwork函数会调用libnetwork. network.CreateEndpoint和libnetwork.controller.NewSandbox为容器当前网络创建Endpoint和Sandbox（Sandbox对应一个容器，仅创建一次），将Endpoint加入到该Sandbox中，以及为容器更新NetworkSettings属性。

![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051641845.png)



### 生成bundle配置文件

```go
f, err := os.Create(filepath.Join(container.dir, configFilename))
```

config.json文件就是Container Configuration file，这个是容器运行时所必须的文件。关于该文件的具体说明，可以参考[容器运行时规范](https://github.com/opencontainers/runtime-spec/blob/main/config.md#configuration-schema-example)

路径：/run/containerd/io.containerd.runtime.v1.linux/moby/



### 容器监控

待容器启动之后，containerd还需要监听容器的OOM事件和容器退出事件。



### shim

shim进程被containerd启动之后，第一步是设置子孙进程成为孤儿进程后由shim进程接管，即shim将变成孤儿进程的父进程，这样就保证容器里的第一个进程不会因为runc进程的退出而被init进程接管。



### runc

runc会被调用两次，第一次是shim调用`runc create`创建容器，第二次是containerd调用`runc start`启动容器。

#### 创建容器

runc会根据参数中传入的bundle目录名称以及容器ID，创建容器.

创建容器就是启动进程`/proc/self/exe init`，由于/proc/self/exe指向的是自己，所以相当于fork了一个新进程，并且新进程启动的参数是init，相当于运行了`runc init`，`runc init`会根据配置创建好相应的namespace，同时创建一个叫exec.fifo的临时文件，等待其它进程打开这个文件，如果有其它进程打开这个文件，则启动容器。

#### 启动容器

启动容器就是运行`runc start`，它会打开并读一下文件exec.fifo，这样就会触发`runc init`进程启动容器，如果`runc start`读取该文件没有异常，将会删掉文件exec.fifo，所以一般情况下我们看不到文件exec.fifo。



1. 运行`runc create`时，后台生成该命令的进程，我们称该进程为parent；
2. parent进程中运行`runc init`，我们称`runc init`进程为child进程；
3. child进程开始准备用户进程的运行环境，此时parent和child进程通过pipe进行通信；
4. child进程准备好用户进程的运行环境后，通知parent退出，自己则被exec.fifo阻塞；
5. 由于parent退出(即`runc create`退出)，child成孤独进程，进而被1进程接收；
6. child进程一直被exec.fifo阻塞；
7. 运行`runc start`时，会打开exec.fifo，使child的阻塞消除，`runc start`退出；
8. 由于阻塞消除，child进程继续往下执行；
9. child进程使用用户定义的命令替换`runc init`，从而child进程成为容器内的主进程；
10. 容器启动完成。





# other

## container-shim进程参数

```shell
root      34919   1524  0 6月15 ?       00:00:09 containerd-shim -namespace moby -workdir /var/lib/containerd/io.containerd.runtime.v1.linux/moby/99c136f8c75c6f94b38c92b22a15a5beea4de1059540a2ac2b95856488a4987e -address /run/containerd/containerd.sock -containerd-binary /usr/bin/containerd -runtime-root /var/run/docker/runtime-runc

```

## io文件

/run/docker/containerd/
文件占用情况
![在这里插入图片描述](https://raw.githubusercontent.com/howu911/picx-images-hosting/master/docker_stu202410051641709.png)

## 运行状态文件

	/run/docker/runtime-runc/moby/

# 参考资料

[oci](https://opencontainers.org/about/overview/)









