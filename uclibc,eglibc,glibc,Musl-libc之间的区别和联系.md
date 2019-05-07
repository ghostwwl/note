## uclibc,eglibc,glibc,Musl-libc之间的区别和联系 


### 1. glibc 
GNU C Library 是GNU项（GNU Project）目，所实现的 C语言标准库（C standard library）。   
目前，常见的桌面和服务器中的GNU/Linux类的系统中，都是用的这套C语言标准库。   
其实现了常见的C库的函数，支持很多种系统平台，功能很全，但是也相对比较臃肿和庞大。  

### 2. uclibc
[uclibc](https://uclibc.org/)一个小型的C语言标准库，主要用于嵌入式。   
其最开始设计用于uClinux（注：uClinux不支持MMU），因此比较适用于微处理器中。   
对应的，此处的u意思是μ，Micro，微小的意思。   

**uClibc的特点：**  
(1)uClibc比glibc要小很多。   
(2)uClibc是独立的，为了应用于嵌入式系统中，完全重新实现出来的。和glibc在源码结构和二进制上，都不兼容。  

### 3. eglibc
[EGLIBC](http://www.eglibc.org/home) = Embedded GLIBC EGLIBC是，（后来）glibc的原创作组织FSF所（新）推出的，glibc的一种变体，目的在于将glibc用于嵌入式系统。   
**EGLIBC的目标是：**  
(1)保持源码和二进制级别的兼容于Glibc 源代码架构和ABI层面兼容 如果真正实现了这个目标，那意味着，你之前用glibc编译的程序，可以直接用eglibc替换，而不需要重新编译。 这样就可以复用之前的很多的程序了。   
(2)降低(内存)资源占用/消耗   
(3)使更多的模块为可配置的（以实现按需裁剪不需要的模块）   
(4)提高对于交叉编译(cross-compilation)和交叉测试(cross-testing)的支持 Eglibc的最主要特点就是可配置，这样对于嵌入式系统中，你所不需要的模块，比如NIS，locale等，就可以裁剪掉，不把其编译到库中，使得降低生成的库的大小了。  

更多特点，可以去看：Eglibc的特点 【glibc, uClibc, Elibc的渊源/历史/区别/联系】 

1. 写程序，需要用到很多c语言的库函数。所有的库函数加起来，就是对应的C语言（标准）函数库。 
2. 目前在普通GNU/Linux系统中所用的C语言标准库，叫做glibc。其功能很全，函数很多，但是代码太多，编译出来的函数库的大小也很大，即资源占用也很多。 
3. 而嵌入式系统中，也需要C语言写代码实现特定功能，也需要用到C语言函数库，但是由于嵌入式系统中，一般资源比较有限，所以不适合直接使用（太占用资源的）gLibc。 
4. 所以有人就又（没有参考glibc，而是从头开始，）重新实现了一个用于嵌入式系统中的，代码量不是很大的，资源占用相对较少的，C语言函数库，叫做uClibc。并且，uClibc不支持MMU（内存管理单元）。 
5. 而后来，glibc的开发者，又推出个Embedded glibc，简称eglibc，其主要目的也是将glibc用于嵌入式领域。 相应最大的改动就在于，把更多的库函数，改为可配置的，这样，如果你的嵌入式系统中不需要某些函数，就可以裁剪掉，不把该函数编译到你的eglibc库中，使得最终生成的eglibc库的大小变小，最终符合你的嵌入式系统的要求（不能超过一定的大小），这样，就实现了，把glibc引用于嵌入式系统中的目的了。 

可以简单的理解为： glibc，uClibc，eglibc都是C语言函数库： 
1. uClibc是嵌入式系统中用的，glibc是桌面系统用的 
2. eglibc也是嵌入式系统中用的，是glibc的嵌入式版本，和glibc在源码和二进制上兼容。

### 4.musl-libc
C语言标准库Musl-libc项目发布了1.0版。[musl](http://www.musl-libc.org)轻量级的C标准库，设计作为GNU C library (glibc)、 uClibc或Android Bionic的替代用于嵌入式操作系统和移动设备。它遵循POSIX 2008规格和 C99 标准，采用MIT许可证授权，使用Musl的Linux发行版和项目包括sabotage，bootstrap-linux，LightCube OS等等。

目前openwrt LEDE默认使用Musl-libc了。。。CC分支还是uclibc
