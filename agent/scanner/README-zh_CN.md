[English](README.md) | 简体中文
## 关于 Yara Scanner 插件
Yara Scanner 使用 [yara 规则](https://yara.readthedocs.io/)对系统进程和敏感目录进行周期扫描，并使用fanotify监控敏感目录文件变动，以发现可疑静态文件（UPX/挖矿二进制/挖矿脚本/可疑脚本文件/...）。

## 平台兼容性
与[Elkeid Agent](../README-zh_CN.md#平台兼容性)相同。


## 配置
在[config.rs](./src/config.rs)中,有下面一些常量，可根据实际情况进行配置（出于性能考虑，除规则外，建议保持默认）。

### 插件配置
```
const SOCKET_PATH:&str = ../../plugin.sock";
const NAME:&str = "scanner";
const VERSION:&str = "0.0.0.0";
```
这些常量可以根据需要进行修改，但是要注意：他们需要与[Agent参数](../README-zh_CN.md#参数和选项)以及[Agent的配置文件](../README-zh_CN.md#配置文件)保持一致。

### 扫描配置
* `SCAN_DIR_CONFIG` 定义扫描目录，以及递归深度
* `SCAN_DIR_FILTER` 定义过滤目录，按照前缀匹配过滤扫描白名单

### 性能限制配置
* `LOAD_MMAP_MAX_SIZE` 定义扫描的文件的最大文件大小，跳过大文件
* `WAIT_INTERVAL_DAILY` 定义周期扫描每轮的间隔时间
* `WAIT_INTERVAL_DIR_SCAN` 定义周期扫描目录的间隔时间
* `WAIT_INTERVAL_PROC_SCAN` 定义周期扫描proc进程的间隔时间


### [Yara 扫描规则](https://yara.readthedocs.io/en/stable/writingrules.html)
配置`RULES_SET`定义Yara扫描规则
目前提供参考的规则:
* UPX 带壳elf
* 存在挖矿通信协议的elf/脚本
* 可疑的脚本文件


更多Yara规则可[参考](https://github.com/InQuest/awesome-yara)
* https://github.com/godaddy/yara-rules
* https://github.com/Yara-Rules/rules
* https://github.com/fireeye/red_team_tool_countermeasures
* https://github.com/x64dbg/yarasigs
...


## 需要的编译环境
* Rust 1.48.0

快速安装 [rust](https://www.rust-lang.org/tools/install) 环境：
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# add build target x86_64-unknown-linux-gnu
rustup target add x86_64-unknown-linux-gnu

```

## 编译
执行以下命令:
```
chmod +x build.sh && ./build.sh
```
或者：
```
RUSTFLAGS='-C target-feature=+crt-static' cargo build --release --target x86_64-unknown-linux-gnu
```
你将会在`target/x86_64-unknown-linux-gnu/release/`下面找到`scanner`二进制文件。静态链接的二进制文件(更易于分发)。

## 已知问题
* Creation time / birth_time is not available for some filesystems
```bash
error: "creation time is not available for the filesystem
```

## License
Yara Scanner plugin is distributed under the Apache-2.0 license.
