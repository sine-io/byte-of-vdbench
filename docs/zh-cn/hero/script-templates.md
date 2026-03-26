# 脚本模板

这一页的目标不是覆盖所有参数，而是给你一组“可以直接复制后改变量”的起点。

建议使用方式：

1. 先选一个最接近自己场景的模板
2. 先只改对象、目录、线程数、时长
3. 先用 `-s` 模拟
4. 再真实执行

## 使用模板前先改什么

无论你复制哪份模板，至少都要先确认：

- 被测对象是不是测试环境
- 输出目录是不是单独的结果目录
- 线程数和运行时长是不是符合当前环境
- 目录或文件是否会覆盖已有数据

## 模板 1：最小 Raw I/O 随机读

适合：

- 第一次改 `example1`
- 先验证块设备或大文件能不能正常打压

```shell
messagescan=no

sd=sd1,lun=$lun,openflags=o_direct
wd=wd1,sd=sd1,xfersize=4k,rdpct=100,seekpct=random
rd=rd1,wd=wd1,iorate=100,elapsed=30,interval=5
```

执行示例：

```shell
./vdbench -f raw-read lun=/dev/sdb -o res+
```

## 模板 2：最小 Raw I/O 顺序写

适合：

- 测试顺序写吞吐
- 先做一个最容易解释的写入场景

```shell
messagescan=no

sd=sd1,lun=$lun,openflags=o_direct
wd=wd1,sd=sd1,xfersize=1m,rdpct=0,seekpct=seq
rd=rd1,wd=wd1,iorate=max,elapsed=30,interval=5,threads=$threads
```

执行示例：

```shell
./vdbench -f raw-seq-write lun=/dev/sdb threads=4 -o res+
```

## 模板 3：Raw I/O 曲线压测

适合：

- 评估块设备的峰值和拐点
- 想给业务找一个更稳妥的工作点

```shell
messagescan=no

sd=sd1,lun=$lun,openflags=o_direct
wd=wd1,sd=sd1,xfersize=8k,rdpct=100,seekpct=random
rd=rd1,wd=wd1,iorate=curve,curve=(10-100,10),stopcurve=$stopcurve,elapsed=20,interval=5,threads=$threads
```

执行示例：

```shell
./vdbench -f raw-curve lun=/dev/sdb threads=4 stopcurve=1.0 -o res+
```

## 模板 4：最小 File System 随机读

适合：

- 第一次改 `example7`
- 先验证文件系统目录结构和读取路径

```shell
create_anchors=yes
messagescan=no

fsd=fsd1,anchor=$anchor,depth=1,width=1,files=100,size=1m
fwd=fwd1,fsd=fsd1,operation=read,xfersize=4k,fileio=random,fileselect=random,threads=$threads
rd=rd1,fwd=fwd1,fwdrate=100,format=yes,elapsed=20,interval=5
```

执行示例：

```shell
./vdbench -f fs-rand-read anchor=/mnt/testfs/vdbench threads=8 -o res+
```

## 模板 5：File System 随机混合读写

适合：

- 模拟更接近业务的文件随机读写
- 同时看 `read-rate` 和 `write-rate`

```shell
create_anchors=yes
messagescan=no

fsd=fsd1,anchor=$anchor,depth=1,width=1,files=100,size=5m
fwd=fwd1,fsd=fsd1,operation=write,rdpct=70,xfersizes=8k,fileio=random,fileselect=random,threads=$threads
rd=rd1,fwd=fwd1,fwdrate=max,format=yes,elapsed=30,interval=5
```

执行示例：

```shell
./vdbench -f fs-rand-rw anchor=/mnt/testfs/vdbench threads=16 -o res+
```

## 模板 6：文件系统只预埋，不正式发压

适合：

- 先把目录树和文件结构准备好
- 把“预埋”和“正式测试”拆成两步

```shell
create_anchors=yes
messagescan=no

fsd=fsd1,anchor=$anchor,depth=2,width=2,files=100,size=1m
fwd=fwd1,fsd=fsd1,operation=write,fileio=random,fileselect=random,threads=$threads,xfersizes=1m
rd=rd1,fwd=fwd1,fwdrate=max,format=only,elapsed=30,interval=5
```

## 模板 7：共享目录两步格式化

适合：

- 多主机
- `shared=yes`
- 共享目录文件系统压测

```shell
create_anchors=yes
messagescan=no

hd=default,shell=ssh,user=$user,vdbench=$vdbench
hd=one,system=$host1
hd=two,system=$host2

fsd=default,depth=2,width=2,files=100,size=8k,shared=yes
fsd=fsd1,anchor=$anchor

fwd=fwd1,fsd=fsd1,operation=read,fileio=random,fileselect=random,threads=$threads

rd=default,fwd=fwd1,fwdrate=100,elapsed=20,interval=5
rd=step1,format=(clean,only)
rd=step2,format=(restart,only)
rd=run1
```

!!! warning
    这个模板的重点不是速率，而是共享目录准备方式。第一次尝试时，优先先把 `step1` 和 `step2` 跑通，再决定要不要加正式负载。

## 模板 8：多变量可复用骨架

适合：

- 反复替换目录、设备、线程数
- 同一份脚本多轮复用

```shell
messagescan=no

sd=default,openflags=o_direct
sd=sd1,lun=$lun,size=$size

wd=wd1,sd=sd1,xfersize=$xfersize,rdpct=$rdpct,seekpct=$seekpct
rd=rd1,wd=wd1,iorate=$iorate,elapsed=$elapsed,interval=$interval,threads=$threads
```

执行示例：

```shell
./vdbench -f reusable \
  lun=/tmp/vdb.raw \
  size=10g \
  xfersize=8k \
  rdpct=100 \
  seekpct=random \
  iorate=100 \
  elapsed=30 \
  interval=5 \
  threads=4 \
  -o res+
```

## 执行前的固定动作

建议形成固定习惯：

### 1. 先模拟

```shell
./vdbench -f parmfile -s
```

### 2. 再短测

```shell
./vdbench -f parmfile -e 10 -i 1 -o res+
```

### 3. 跑完先看这几个文件

- `summary.html`
- `totals.html`
- `parmfile.html`
- `parmscan.html`

## 下一步

- 想先理解参数规则：看 [参数文件基础](parameter-file-basics.md)
- 想继续看块设备参数：看 [Raw I/O 参数详解](raw-parameter-detail.md)
- 想继续看文件系统参数：看 [File System 参数详解](filesystem-parameter-detail.md)
- 想知道官方样例该怎么对照阅读：看 [官方样例索引](example-map.md)
- 想先学怎么看结果：看 [结果分析](../zero/results-analysis.md)
