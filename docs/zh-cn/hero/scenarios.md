# 常见场景

这一页不按单个参数讲，而是按“实际测试任务”来组织。

## 场景 1：先确认环境能不能跑

优先执行：

```shell
./vdbench -t
./vdbench -tf
```

目的不是看性能，而是先确认：

- `Java` 环境正常
- `vdbench` 脚本可执行
- 结果目录能正常生成

## 场景 2：先用官方样例改出自己的第一份脚本

建议起点：

- 块设备：`example1`
- 文件系统：`example7`

做法很简单：

1. 只改被测对象
2. 只改最少量参数
3. 先跑通
4. 再加复杂参数

## 场景 3：做一次受控的限速测试

适合回答：

- 指定速率下时延如何
- 某设备在 `100/500/1000 IOPS` 下分别表现怎样

典型写法：

```shell
iorate=100
iorate=(100,200,300)
fwdrate=100
fwdrate=(100,200,300)
```

这类测试适合和 `totals.html`、`flatfile.html` 一起看。

## 场景 4：找峰值或画性能曲线

适合回答：

- 峰值大概在哪
- 到了多高的压力后，时延开始明显恶化

典型写法：

```shell
iorate=max
iorate=curve
fwdrate=max
fwdrate=curve
```

建议搭配：

- `histogram.html`
- `summary.html`
- `flatfile.html`

如果你已经知道自己的时延上限，可以把 `stopcurve=` 一起加上，让曲线在越过阈值后自动停止。

## 场景 5：安全地准备文件系统数据

如果你还没准备好文件结构，优先考虑：

```shell
format=yes
```

如果你只想先预埋数据，不想立刻执行正式测试，优先考虑：

```shell
format=only
```

如果你想保留已有结构，必须先确认它与当前 `FSD` 定义一致，再考虑：

```shell
format=restart
```

## 场景 6：共享目录或多主机压测

这类场景先别急着调速率，先确认结构和角色：

- 哪些主机参与发压
- `anchor` 是否共享
- 是否需要 `shared=yes`
- 文件结构由谁创建
- 是否需要 `format=restart`

如果是 `shared=yes` 的文件系统场景，优先按两步准备：

1. `format=(clean,only)`
2. `format=(restart,only)`

不要直接上 `format=yes`。

官方参考可先看：

- `examples/raw/multi_host`
- `examples/filesys/multi_host`

## 场景 7：改了很多 `forxxx`，不确定会怎么执行

优先用：

```shell
./vdbench -f parmfile -s
```

它不会真实执行工作负载，但能帮助你先确认脚本编排是否符合预期。

## 场景 8：结果很多，想做多轮对比

建议组合：

- `-o res+`
- `flatfile.html`
- `./vdbench parse`
- `./vdbench compare`

这样更适合做多轮测试留档，而不是只盯住单次控制台输出。

## 下一步

- 想直接拿一份脚本开改：看 [脚本模板](script-templates.md)
- 想回头补参数基础：看 [参数文件基础](parameter-file-basics.md)
