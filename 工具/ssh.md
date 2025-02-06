Ubuntu 24.04 默认情况下**不会自动安装SSH服务器**（即 `openssh-server`），但通常会预装SSH客户端（`openssh-client`）。因此，如果你需要通过SSH连接到其他主机，客户端工具已经可用，但如果你希望其他设备通过SSH连接到这台Ubuntu 24.04主机，则需要手动安装和配置SSH服务器。

以下是更详细的说明和步骤：

---

### 1. 检查是否已安装SSH服务器
在Ubuntu 24.04上，可以通过以下命令检查是否已安装SSH服务器：
```bash
dpkg -l | grep openssh-server
```
- 如果输出中包含 `openssh-server`，说明已经安装。
- 如果没有输出，则需要手动安装。

---

### 2. 安装SSH服务器
如果未安装SSH服务器，可以通过以下命令安装：
```bash
sudo apt update
sudo apt install openssh-server
```

---

### 3. 启动和启用SSH服务
安装完成后，SSH服务会自动启动。如果没有启动，可以使用以下命令手动启动：
```bash
sudo systemctl enable ssh
```
设置SSH服务开机自启：
```bash
sudo systemctl start ssh
```

---

### 4. 检查SSH服务状态
确认SSH服务是否正常运行：
```bash
sudo systemctl status ssh
```
如果看到 `active (running)`，说明服务已成功启动。

---

### 5. 获取Ubuntu主机的IP地址
使用以下命令查看Ubuntu主机的IP地址：
```bash
ip a
```
找到 `inet` 后面的IP地址（通常是 `192.168.x.x` 或 `10.x.x.x`）。

---

### 6. 在Windows 11上连接
在Windows 11上，可以通过以下方式连接：

#### 方法 1：使用Windows内置的OpenSSH客户端密码连接
1. 打开命令提示符或PowerShell。

2. 输入以下命令：
   ```bash
   ssh username@ip_address
   ```
   - 替换 `username` 为Ubuntu主机的用户名。
   - 替换 `ip_address` 为Ubuntu主机的IP地址。

3. **首次连接确认**：
   首次连接时，系统会提示确认主机指纹，输入`yes`继续。会在known_hosts文件中添加记录

   ![image-20250206175328405](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206175328405.png)

4. **输入密码**：
   输入Ubuntu用户的密码，成功后会进入Ubuntu的命令行界面。

   ![image-20250206165308753](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206165308753.png)

5. config

   在本地的.ssh目录下，创建config文件像下图这样配置，每次用ssh指令连接时我们就不用把完整的username@ip_address输入进去，输入 `ssh ubuntu2404` 再输入密码即可连接：

   ![image-20250206183550941](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206183550941.png)

   <img src="https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206183840189.png" alt="image-20250206183840189" style="zoom:67%;" />

   





#### 方法 2：使用PuTTY/final shell
1. 下载并安装PuTTY（[官网](https://www.putty.org/)）。
2. 打开PuTTY，输入Ubuntu主机的IP地址。
3. 在“Connection” -> “SSH” -> “Auth”中，选择私钥文件（如果使用密钥认证）;输入用户名和密码（使用密码认证）。
4. 点击“Open”，输入用户名和密码。



#### 方法 3： 使用SSH密钥认证（推荐）

在远程主机上规定的位置放置公钥，在本地规定的地方保存私钥，只要远程主机的ssh服务是开启状态，就可以通过ssh进行安全的连接。

1. **在Windows 11上生成SSH密钥**：
   打开终端，运行：

   ```bash
   ssh-keygen [option]
   ```

   -t 后面跟的是算法名，推荐使用 ed25519 ；-C 后面跟的是注释；

   首先让你输入保存密钥对的路径和文件名，可以自己指定（若不指定新的文件名，会覆盖掉原来已经生成过的同名的文件），下图由于我的终端已经位于.ssh目录下，直接输入文件名即可，不需要指定绝对路径；建议都在.ssh目录下输入命令

   还会提示你输入 passphrase，相当于给你的密钥又上了一层锁，不想设置直接按回车即可

   ![image-20250206182040039](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206182040039.png)

2. **将公钥复制到Ubuntu**：
   使用以下命令将公钥复制到Ubuntu主机，替换`username`和`ip_address`：

   ```bash
   ssh-copy-id -i <对应的公钥文件路径> username@ip_address
   ```

   windows不自带 `ssh-copy-id` ，可以手动把对应的公钥粘贴到远程主机的~/.ssh/authorized_keys文件中

   ![image-20250206183933110](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206183933110.png)

   <img src="https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206183243068.png" alt="image-20250206183243068" style="zoom:50%;" />

3. **测试无密码登录**：
   再次使用SSH连接，应无需输入密码 (除非在生成密钥对的时候输入了passphrase)。

   输入：

   ```
   ssh -i <对应私钥文件的路径> username@ip_address
   ```

   如果不想每次都输入-i <对应私钥文件的路径>、完整的username@ip_address，修改config文件如下图即可 `ssh ubuntu2404` 免密登陆：`IdentityFile` 可以指定使用的私钥文件
   
   ![image-20250206204200461](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206204200461.png)

---

### 7. 禁止密码登陆（选做）

首先连接到远程，然后打开远程 /etc/ssh/ 目录下的 `sshd_config` 文件，修改框出的值来禁用密码验证 ！！！修改时、或者最后不成功千万不要断开连接！！！！：

![image-20250206202430516](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250206202430516.png)

最后再查看以下 `sshd_config.d` 这个文件夹下的文件中还有没有 `PasswordAuthentication yes` 存在，如果有你要把 yes 改成 `no` (购置云服务器时设置了用密码进行连接可能就会出现这种情况)



### 8. 防火墙设置（如有）

如果Ubuntu主机启用了防火墙（如 `ufw`），确保允许SSH端口（默认22或自定义端口）：
```bash
sudo ufw allow ssh
```
如果修改了SSH端口，需要明确允许该端口，例如：
```bash
sudo ufw allow 2222
```

---

