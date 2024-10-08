## Linux 系统优化配置文件

### 文件简介

这个 `sysctl.conf` 文件包含了一系列经过优化的内核参数配置，专门针对 32GB 内存的 Linux 系统，目的是提升网络性能、系统 I/O 性能以及整体的系统稳定性。该配置适用于日常使用，如编写代码、浏览网页、观看视频等场景。

### 优化目标

- **稳定地提升性能**：不激进的优化，旨在没有副作用地提升性能。
- **提升网络性能**：通过启用 BBR 拥塞控制算法和 CAKE 队列调度算法，实现更高的带宽利用率、更低的网络延迟以及更稳定的网络传输。
- **减少磁盘 I/O 频率**：调整内存中的脏数据处理机制，减少磁盘写入操作的频率，从而优化磁盘性能。
- **优化缓存管理**：增加 VFS 缓存的保持时间，减少不必要的缓存回收，提升文件系统的访问速度。
- **增强系统的连接处理能力**：调整系统允许的最大并发连接数，适合高并发的网络应用。

### 配置文件的特性

1. **网络性能优化**
   - **启用 BBR 拥塞控制算法**：BBR 提供更好的带宽管理和延迟优化，特别适合高带宽、高延迟网络。
   - **启用 CAKE 队列调度算法**：CAKE 确保多用户环境中网络流量的公平分配，优化网络接口的延迟和队列管理。

2. **I/O 性能优化**
   - **调整脏数据处理**：设置 `vm.dirty_bytes` 和 `vm.dirty_background_bytes`，确保系统在有足够的脏数据时才进行磁盘写入，减少频繁的小规模写入操作。
   - **调整写回时间间隔**：延长内核写回脏数据的时间间隔，减少后台线程频繁写入磁盘操作的次数。

3. **文件系统性能优化**
   - **VFS 缓存压力调整**：通过降低 `vm.vfs_cache_pressure`，减少对文件系统缓存的回收倾向，提升文件系统的访问速度。

4. **提高网络并发能力**
   - **增加最大连接数**：将 `net.core.somaxconn` 设置为 8192，允许更多并发连接，适合高负载的网络应用环境。
   - **调整 TCP/UDP 缓冲区**：增大 TCP 和 UDP 的发送和接收缓冲区，提升网络吞吐量，减少丢包率。

### 使用方法

1. 加载bbr模块
```bash
sudo modprobe tcp_bbr
```

2. 输入命令，检查可用的拥堵控制算法。如果输出显示了bbr,就可以进行下一步。
```
sysctl net.ipv4.tcp_available_congestion_control
```

3. 使用 `sudo` 和 `nano` 创建并编辑一个新的 `sysctl` 配置文件。例如，可以使用 `99-sysctl.conf` 这个文件名：
```bash
sudo nano /etc/sysctl.d/99-sysctl.conf
```

4. 编辑文件。复制[99-sysctl.conf.conf 文件](./99-sysctl.conf.conf)里的代码内容，并按住ctrl+shift+v粘贴。然后按下ctrl+o保存文件，ctrl+x退出文件编辑。

5. 应用这些设置，然后重启系统。
```bash
sudo sysctl --system
sudo reboot
```




<!---
wxmup/wxmup is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
