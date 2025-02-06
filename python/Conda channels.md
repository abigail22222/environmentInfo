管理 Conda Channels
-----------

Conda Channels是存储软件包的位置。它们用于托管和管理软件包。Conda 软件包从远程Channels下载，这些Channels是包含 Conda 软件包的目录的 URL。`conda` 命令会搜索一组默认Channels，并从[默认Channels](https://repo.anaconda.com/pkgs/)自动下载和更新软件包。可以阅读更多关于 [Conda Channels](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/channels.html) 以及其使用条款的信息。

### 处理Channels冲突

不同的Channels可能包含相同的软件包，因此 Conda 需要处理这些Channels冲突。

*   如果你只使用默认Channels，则不会发生Channels冲突。 
*   如果你使用的所有Channels只包含独有的软件包，而这些软件包在其他Channels中不存在，则也不会发生冲突。
*   当多个Channels都包含相同的软件包时，Conda 需要解析这些冲突。

### Channels优先级

默认情况下，Conda 优先选择较高优先级Channels的软件包，而不会考虑较低优先级Channels的版本。因此，你可以将非默认Channels放在Channels列表的底部，以提供额外的软件包，同时确保它们不会覆盖核心软件包集。

Conda 处理多个Channels的软件包时，会按照以下规则排序：

1.  按Channels优先级从高到低排序软件包。
2.  在相同Channels优先级的情况下，按版本号从高到低排序。例如，如果 `channelA` 包含 NumPy 1.12.0 和 1.13.1，则 NumPy 1.13.1 的优先级更高。
3.  若版本号相同，则按构建编号从高到低排序。例如，如果 `channelA` 包含 NumPy 1.12.0 的构建版本 1 和 2，则构建版本 2 优先级更高。同样，`channelB` 中的软件包优先级低于 `channelA`。
4.  安装排序后列表中的第一个符合安装要求的软件包。

最终排序示例：

```
channelA::numpy-1.13_1 > channelA::numpy-1.12.1_1 > channelA::numpy-1.12.1_0 > channelB::numpy-1.13_1
```

**注意：** 如果启用了严格的Channels优先级，则 `channelB::numpy-1.13_1` 不会被包含在排序列表中。

### 使 Conda 安装所有Channels中最新的软件包

如果希望 Conda 从所有列出的Channels中安装最新版本的软件包，可以：

*   在 `.condarc` 文件中添加：
    
    ```
    channel_priority: disabled
    ```
    
    **或者**
*   运行以下命令：
    
    ```
    conda config --set channel_priority disabled
    ```
    

此时，Conda 会按照以下规则排序：

1.  按版本号从高到低排序。
2.  在版本号相同的情况下，按Channels优先级从高到低排序。
3.  若Channels优先级相同，则按构建编号从高到低排序。

由于不同Channels的构建编号不可比，因此构建编号仍然排在Channels优先级之后。

### 修改Channels优先级

*   将 `new_channel` 添加到Channels列表顶部，使其具有最高优先级：
    
    ```
    conda config --add channels new_channel
    ```
    
*   另一种等效的命令：
    
    ```
    conda config --prepend channels new_channel
    ```
    
*   将 `new_channel` 添加到Channels列表底部，使其具有最低优先级：
    
    ```
    conda config --append channels new_channel
    ```
    

严格的Channels优先级
--------

自 Conda 4.6.0 版本起，Conda 引入了严格的Channels优先级功能。启用严格的Channels优先级可以显著加快 Conda 的操作速度，并减少软件包不兼容问题。因此，建议在可能的情况下将Channels优先级设置为 **strict**。

可以使用以下命令查看该功能的详细信息：

```
conda config --describe channel_priority
```

可选的 `channel_priority` 值包括：

*   `strict`：如果高优先级Channels中存在相同名称的软件包，则不会考虑低优先级Channels的软件包。
*   `flexible`（默认值）：如果需要满足依赖关系，解析器可能会从低优先级Channels获取软件包，而不会立即报错。
*   `disabled`：优先选择最高版本的软件包，仅在版本相同时才参考Channels优先级。

在 Conda 早期版本中，该参数仅能设置为 `True` 或 `False`，其中 `True` 现在等同于 `flexible`。

当前的默认设置：

```
channel_priority: flexible
```
