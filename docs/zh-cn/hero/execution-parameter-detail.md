# 命令行参数详解

这一页只保留最常用、最值得先掌握的执行参数。

## `-f`

指定参数文件。

```shell
./vdbench -f parmfile
./vdbench -f basefile fsd fwd rd
```

如果同时传入多个文件，最终顺序仍然必须满足参数文件顺序规则。

## `-o`

指定结果输出目录。

```shell
./vdbench -f parmfile -o output
./vdbench -f parmfile -o res+
./vdbench -f parmfile -o res.tod
```

常见形式：

- `output`
  - 固定目录
- `res+`
  - 自动递增目录名，如 `res001`
- `res.tod`
  - 自动带时间戳

## `-t` 与 `-tf`

最适合环境自检。

- `-t`
  - 执行内置的 Raw I/O 快测
- `-tf`
  - 执行内置的 File System 快测

建议第一次安装完成后先执行它们，而不是一上来就写复杂脚本。

## `-e` / `-i` / `-w`

这三个参数适合临时覆盖 `RD` 里的时间相关配置：

- `-e`
  - 覆盖 `elapsed=`
- `-i`
  - 覆盖 `interval=`
- `-w`
  - 覆盖 `warmup=`

适合在长测前先做一轮短测验证。

## `-m`

控制 `JVM/Slave` 数量相关行为。

在高 `IOPS` 或多设备场景下，如果单个 `JVM` 压力过大，可以结合：

- `-m nn`
- `hd=default,jvms=nn`

来增加并发执行单元。

## `-v`

开启数据验证。

常见形式：

- `-v`
  - 基础数据验证
- `-vr`
  - 写后立即读回验证
- `-vw`
  - 再次写入前不预读
- `-vt`
  - 记录时间戳，内存开销更大

如果你主要关心性能，而不是数据校验，不要默认开启它。

## `-j`

开启日志化的数据验证。

常见形式：

- `-j`
- `-jr`
- `-jn`
- `-jm`

这一组更偏高级恢复与校验场景。除非你已经明确需要 journal 文件，否则先把基础性能测试跑稳，再碰它。

## `-s`

模拟执行，不真实发压。

```shell
./vdbench -f parmfile -s
```

最适合：

- 校验复杂 `forxxx`
- 校验组合 `include=`
- 在真正执行前确认参数展开是否符合预期

## `-c`

与文件系统预埋或清理有关。

常见形式：

- `-c`
- `-co`
- `-cy`
- `-cn`

如果你还没有完全搞清楚当前 `format=` 与目录结构的关系，请不要把它当常规参数使用。

## `-p`

指定 socket 端口。

```shell
./vdbench -f parmfile -p 5570
```

主要在多主机或端口冲突排查时使用。

## `-l`

循环执行所有 `RD`。

```shell
./vdbench -f parmfile -l
./vdbench -f parmfile -l 24h
```

注意：

- `-l`
  - 执行层面的循环
- `loop=`
  - 参数文件里的循环

两者名字像，但不完全是一回事。实际使用时，优先避免同时把两者写得很复杂。

## `-r`

从指定 `RD` 开始执行。

```shell
./vdbench -f parmfile -r rd2
```

适合多 `RD` 脚本中的断点继续或局部重跑。

## 命令行变量替换

你可以在参数文件里写变量，并在执行时赋值：

```shell
sd=sd1,lun=$lun
rd=rd1,wd=wd1,threads=$threads
```

执行时：

```shell
./vdbench -f parmfile lun=/dev/sdb threads=8
```

这是组织可复用脚本时最常见的技巧之一。

## 推荐掌握顺序

新手建议先掌握下面这些：

1. `-f`
2. `-o`
3. `-t` / `-tf`
4. `-s`
5. `-e` / `-i` / `-w`
6. `-r`
7. `-l`

把这几项用熟之后，再去碰 `-v`、`-j`、`-c` 这类更容易引入副作用的参数。

## 下一步

- 想直接改一份能跑的脚本：看 [脚本模板](script-templates.md)
- 想把官方示例和自己的场景对上：看 [官方样例索引](example-map.md)
