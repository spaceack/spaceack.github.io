# 第一次攒机记录-1-基本配置


## 配置明细：

|组件|选型|选型分析|价格|
|-|-|-|-|
|CPU|盒装AMD Ryzen 7 3700X|4.4GHz Max Boost, 3.6GGz Base 7nm 8核16线程65w,性能强劲，性价比高|2660.15（含主板）|
|主板|MSI B450M PRO-VDH MAX|集成声卡/网卡，最大内存容量64GB， 有较大扩展性||
|内存|金士顿HX430C5PB3/16 SP||549|
|显卡|盈通 RX550 2G||369|
|SSD|Samsung SSD 970 EVO Plus 250G|用做系统盘，秒启的秘密|422.82|
|硬盘|WD 蓝盘 20EZAZ RZ 2T|数据盘|340.03
|电源|先马金牌500W全模组||338|
|机箱|爱国者YOGO M2|白色机箱很好看ヾ(o◕∀◕)ﾉヾ|165.62|
||||4844.62|

### CPU参数
`lscpu`
```
架构：           x86_64
CPU 运行模式：   32-bit, 64-bit
字节序：         Little Endian
CPU:             16
在线 CPU 列表：  0-15
每个核的线程数： 2
每个座的核数：   8
座：             1
NUMA 节点：      1
厂商 ID：        AuthenticAMD
CPU 系列：       23
型号：           113
型号名称：       AMD Ryzen 7 3700X 8-Core Processor
步进：           0
CPU MHz：        2199.572
CPU 最大 MHz：   3600.0000
CPU 最小 MHz：   2200.0000
BogoMIPS：       7200.06
虚拟化：         AMD-V
L1d 缓存：       32K
L1i 缓存：       32K
L2 缓存：        512K
L3 缓存：        16384K
NUMA 节点0 CPU： 0-15
标记：           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid extd_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt tce topoext perfctr_core perfctr_nb bpext perfctr_llc mwaitx cpb cat_l3 cdp_l3 hw_pstate sme ssbd mba sev ibpb stibp vmmcall fsgsbase bmi1 avx2 smep bmi2 cqm rdt_a rdseed adx smap clflushopt clwb sha_ni xsaveopt xsavec xgetbv1 xsaves cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local clzero irperf xsaveerptr wbnoinvd arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold avic v_vmsave_vmload vgif umip rdpid overflow_recov succor smca

```
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-8.jpg)

### 内存
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-4.jpg)

### 主板
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-9.jpg)

### SSD 
#### 术语解析：

- NVM Express （Non-Volatile Memory express， `NVMe`）非易失性内存主机控制器接口规范是一个逻辑设备接口规范， 用于访问通过PCI-Express（`PCIe`）总线附加的非易失性内存介质。

 > 此规范目的在于充分利用PCI-E通道的低延时以及并行性，还有当代处理器、平台与应用的并行性，在可控制的存储成本下，极大的提升固态硬盘的读写性能，降低由于AHCI接口带来的高延时，彻底解放SATA时代固态硬盘的极致性能。

 - PCIe: PCI-Express, 高速串行计算机扩展总线标准。

#### 使用`hdparm`初步测试硬盘性能
```bash
apt-get install hdparm  
// 使用 fdisk -l 查看设备信息
/dev/nvme0n1p2 
/dev/sda2

// 测试SSD读取效率
hdparm -tT /dev/nvme0n1p2
 Timing cached reads:   23364 MB in  2.00 seconds = 11693.62 MB/sec
 Timing buffered disk reads: 8170 MB in  3.00 seconds = 2723.12 MB/sec

// 测试机械硬盘读取效率
hdparm -tT /dev/sda2
 Timing cached reads:   23728 MB in  2.00 seconds = 11875.69 MB/sec
 Timing buffered disk reads: 462 MB in  3.01 seconds = 153.42 MB/sec
```
![](http://qiniu.aimiter.com/myblog/IMG_20191117_201108-1.jpg)
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-6.jpg)


### 显卡
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-2.jpg)
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-3.jpg)

### 电源
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-1.jpg)
![](http://qiniu.aimiter.com/myblog/IMG_20191121_184641-0.jpg)

