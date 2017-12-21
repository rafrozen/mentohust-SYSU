## 交叉编译
## 交叉编译

下载交叉编译的编译器

网址为 https://downloads.openwrt.org/chaos_calmer/15.05/

找寻相应的编译器的版本,比如 `cpu` 为 `mt7621`为

https://downloads.openwrt.org/chaos_calmer/15.05/ramips/mt7621/

下载后解压到相应的文件夹

设置环境变量

修改`the_path/to_openwrt/OpenWrt-SDK-xxx`

```
export PATH=$PATH:/the_path/to_openwrt/OpenWrt-SDK-xxx/staging_dir/toolchain-mipsel_24kec+dsp_gcc-4.8-linaro_uClibc-0.9.33.2/bin

export STAGING_DIR=/the_path/to_openwrt/OpenWrt-SDK-xxx/staging_dir
```

### 编译

先要编译 `libpcap`

```
cd the_path_to_libpcap
./configure --host=mipsel-openwrt-linux-uclibc --with-pcap=linux
make -j4
```

此时在当前文件夹已经生成了`libpcap.a`

接着编译`mentohust`

由于中山大学的锐捷,所以下载`mentohust-sysu`版本的

https://github.com/Placya/mentohust-SYSU

```
cd the_path_mentohust
sh ./autogen.sh
./configure --host=mipsel-openwrt-linux-uclibc
make
```


此时应该可以在`src`文件夹下发现`mentohust`

有些版本的编译需要改一些参数

```
./configure --host=mipsel-openwrt-linux-uclibc --disable-encodepass --disable-notify --with-pcap=/path_to_libpcap/libpcap.a
make
```
在编译静态连接可以减少库相关,

在链接的时候会报错 `_Unwind_Resume' 和`__gxx_personality_v0` 只是由于在有些项目加入了多线程,而多线程用的是`cpp`导致编译失败

所以进入`src`文件夹,编辑`makefile`在`LDADD`增加`-lgcc_eh`即可

下面给出`mentohust`的一个代理版本和不是代理版本的`sysu`下载连接,并给静态和动态连接的下载连接

代理版本,在`github`可以搜到,不给链接了,动态要`libpthread.so.0,libgcc_s.so.1,libc.so.0`相应的库

[代理版本静态](http://ow3kig4i4.bkt.clouddn.com/github/mentohust/mentohust-proxy-s)

[代理版本动态](http://ow3kig4i4.bkt.clouddn.com/github/mentohust/mentohust-proxy)

[sysu静态](http://ow3kig4i4.bkt.clouddn.com/github/mentohust/mentohust-s)

[sysu动态](http://ow3kig4i4.bkt.clouddn.com/github/mentohust/mentohust)

并给一个关于东校区的`h3c`的路由器拨号的博客,以及我忘记在那里下载的一个`h3c`的程序

https://blog.terrychan.me/2016/setup-inode-and-ipv6-in-sysu-east-campus


[h3c](http://ow3kig4i4.bkt.clouddn.com/github/mentohust/h3c)