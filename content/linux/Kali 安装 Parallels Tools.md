---
title: "Kali 安装 Parallels Tools"
---


### 0.安装过程遇到的主要问题：

- 1.Parallels Tools 安装时的 /media/cdrom0权限问题
- 2.Kali 的 apt-get源
- 3.Parallels Tools 自动安装依赖无法安装 linux-headers
- 4.makefile编译失败
- 5.安装 Parallels Tools 后 Kali 登陆后白屏

### 1.Parallels Tools 安装时 /media/cdrom0权限问题

点击安装`parallels tools`的时候，会有提示框，提示权限问题，如果直接运行`install`脚本，提示权限不够，官方推荐的做法：

- 先卸载`# umount /media/cdrom0`
- 再挂载`# mount -o exec /media/cdrom0`
  按以上操作，依旧提示`# mount: /media/cdrom0: WARNING: device write-protected, mounted read-only.`

解决方案：
很简单，直接把文件复制到出来，然后`chmod 777 -R .`赋权即可~

### 2.Kali 的 apt-get 源

以下可用源填入`/etc/apt/sources.list`即可

```
#中科大
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
#阿里云
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
#清华大学
deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
#官方源
deb http://http.kali.org/kali kali-rolling main non-free contrib
deb-src http://http.kali.org/kali kali-rolling main non-free contrib
```

更新完依次执行

```
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get clean
```

### 3.Parallels Tools 自动安装依赖无法安装 linux-headers

接下来的错误都是要查看日志文件了

```
# cat /var/log/parallels-tools-install.log
```

如果是无法安装`linux-headers`的话，就要手动安装。
先查看内核版本

```
# uname -a
```

然后来这里<http://http.kali.org/kali/pool/main/l/linux/>下载三个对应内核版本的安装包手动安装

- linux-kbuild: linux-kbuild-xxxx_amd64.deb
- linux-header-common: linux-headers-xxxx-common_xxxx_amd64.deb
- linux-compiler-gcc: linux-compiler-gcc-xxx-amd64.deb
- linux-headers: linux-headers-xxxx_amd64.deb
  下载完成后，用dpkg命令安装deb包。

<!---
title: "Kali 安装 Parallels Tools"
---
->

```
# tar -xzf kmods/prl_mod.tar.gz
# rm prl_mod.tar.gz
```

- 修改`prl_eth/pvmnet/pvmnet.c`

<!---
title: "Kali 安装 Parallels Tools"
---
->

```
# vi prl_tg/Toolgate/Guest/Linux/prl_tg/prltg.c
# 编辑第1535行，同样是将“Parallels”替换为“GPL”
```

- 修改`prl_fs_freeze/Snapshot/Guest/Linux/prl_freeze/prl_fs_freeze.c`

<!---->

```
//第一步：增加函数
//第212行
void thaw_timer_fn(unsigned long data)
{
   struct work_struct *work = (struct work_struct *)data;
   schedule_work(work);
}
//后面增加以下函数
void thaw_timer_fn_new_kernel(struct timer_list *data)
{
   struct work_struct *work = data->expires;
   schedule_work(work);
}
//第二步：修改宏
//刚刚的位置往下两行的
DEFINE_TIMER(thaw_timer, thaw_timer_fn, 0, (unsigned long)&(thaw_work));
//改为
#if LINUX_VERSION_CODE >= KERNEL_VERSION(4, 15, 0)
DEFINE_TIMER(thaw_timer, thaw_timer_fn_new_kernel);
#else
DEFINE_TIMER(thaw_timer, thaw_timer_fn, 0, (unsigned long)&(thaw_work));
#endif
```

- 重新打包`prl_mod.tar.gz`

```shell
tar -zcvf prl_mod.tar.gz . dkms.conf Makefile.kmods
```

### 5.安装 Parallels Tools 后 Kali 登陆后白屏

![Untitled](/assets/kali-parallels-tools-1.png)
