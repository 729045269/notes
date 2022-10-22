### aws磁盘扩容

改文章转载自：`https://zhuanlan.zhihu.com/p/377278873`

#### AWS EC2 控制面板修改磁盘大小

- 从`AWS Console`进行`Elastic Block Store`的卷中
- 选择需要扩容的磁盘，点击`操作`按钮
- 点击`修改`卷
- 卷类型选择`通用型SSD(gp2)`（可以根据自己的需求选择其他类型）
- 大小输入`16`（按需输入）
- 点击确认修改

![image-20221020231543348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020231543348.png)

![image-20221020231813487](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020231813487.png)

## 调整卷大小后扩展 Linux 文件系统

- 使用`df -h`命令验证挂载在"/"下的根分区是否已满（100%）

![image-20221020231846438](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020231846438.png)

- 运行以下命令`lsblk`以收集有关附加块设备和根"/"挂载点的详细信息。

![image-20221020231900385](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020231900385.png)

- 为避免块设备上没有剩余空间错误，请将临时文件系统tmpfs挂载到/tmp挂载点。这会创建一个挂载到/tmp的 10 M tmpfs

```text
sudo mount -o size=10M,rw,nodev,nosuid -t tmpfs tmpfs /tmp
```

![image-20221020231925095](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020231925095.png)

\- 运行`growpart`命令以增大根分区或分区1 的大小。将`/dev/nvme0n1`替换为你的根分区

```text
sudo growpart /dev/nvme0n1 1
```

- 运行`lsblk`命令以验证分区 1 是否已扩展到 16 GiB

![image-20221020231947243](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020231947243.png)

- 扩展文件系统。执行`lsblk -f`确认磁盘类型

![image-20221020232021838](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020232021838.png)

- 如果想要扩展`XFS`类型的文件系统，执行下面的命令

```text
sudo xfs_growfs -d /
```

![image-20221020232036061](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020232036061.png)

- 如果想要扩展`EXT2/EXT3/EXT4`文件系统，执行

```text
sudo resize2fs /dev/nvme0n1p1
```

- 扩展文件系统后，使用`df -h`命令验证操作系统是否可以看到额外的空间

![image-20221020232118692](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221020232118692.png)

- 使用unmount命令卸载tmpfs文件系统

```text
$ sudo umount /tmp
```

## FAQ

## FAQ

- FAILED: failed to dump sfdisk info for /dev/nvme0n1
  为避免块设备上没有剩余空间错误，请将临时文件系统tmpfs挂载到/tmp挂载点。这会创建一个挂载到/tmp的 10 M tmpfs

```text
sudo mount -o size=10M,rw,nodev,nosuid -t tmpfs tmpfs /tmp
```

- FAILED: failed: sfdisk --list /dev/nvme0n1
  为避免块设备上没有剩余空间错误，请将临时文件系统tmpfs挂载到/tmp挂载点。这会创建一个挂载到/tmp的 10 M tmpfs

```text
sudo mount -o size=10M,rw,nodev,nosuid -t tmpfs tmpfs /tmp
```