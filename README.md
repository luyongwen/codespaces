# Codespaces 开发环境

用于创建包含 Podman 相关工具的 GitHub Codespaces 开发环境。

## 功能特性

本 Codespaces 配置提供以下工具和功能：

- **Podman**: 无守护进程的容器引擎，提供与 Docker 兼容的命令行界面
- **Buildah**: 用于构建 OCI 容器镜像的工具
- **Skopeo**: 用于管理容器镜像和镜像仓库的工具
- **Podman Compose**: Podman 的 docker-compose 实现
- **其他工具**: crun (OCI 运行时)、slirp4netns (用户态网络)、fuse-overlayfs (文件系统)

## 快速开始

### 1. 使用 GitHub Codespaces

1. 在 GitHub 仓库页面点击 **Code** 按钮
2. 选择 **Codespaces** 选项卡
3. 点击 **Create codespace on main** 创建新的 Codespace
4. 等待环境初始化完成

### 2. 验证安装

创建 Codespace 后，可以运行以下命令验证工具是否正确安装：

```bash
# 检查 Podman 版本
podman --version

# 检查 Buildah 版本
buildah --version

# 检查 Skopeo 版本
skopeo --version
```

## 使用示例

### Podman 基本操作

```bash
# 运行一个容器
podman run -it ubuntu:latest /bin/bash

# 列出运行中的容器
podman ps

# 列出所有容器
podman ps -a

# 拉取镜像
podman pull docker.io/library/nginx:latest

# 列出本地镜像
podman images
```

### Buildah 构建镜像

```bash
# 从 Dockerfile 构建镜像
buildah bud -t myimage:latest .

# 创建一个新的工作容器
buildah from ubuntu:latest

# 提交容器为镜像
buildah commit working-container myimage:latest
```

### Skopeo 镜像管理

```bash
# 检查远程镜像
skopeo inspect docker://docker.io/library/alpine:latest

# 复制镜像
skopeo copy docker://alpine:latest docker://myregistry.com/alpine:latest

# 删除远程镜像
skopeo delete docker://myregistry.com/alpine:latest
```

## 技术说明

### 无根容器 (Rootless Containers)

本环境配置支持无根容器运行，提供更好的安全性：

- 使用 `vscode` 用户运行容器
- 配置了 subuid 和 subgid 映射
- 使用 fuse-overlayfs 作为存储驱动
- 支持用户态网络 (slirp4netns)

### 存储配置

Podman 存储配置位于：`~/.config/containers/storage.conf`

容器数据存储在：`~/.local/share/containers/storage`

## 常见问题

### 权限问题

如果遇到权限相关的错误，请确保：
1. 使用 `vscode` 用户运行命令
2. 检查 subuid/subgid 配置是否正确

### 网络问题

Podman 在 rootless 模式下使用 slirp4netns 提供网络功能，这可能比特权容器稍慢，但更安全。

## 贡献

欢迎提交 Issues 和 Pull Requests 来改进这个开发环境配置。

## 许可证

本项目采用 MIT 许可证。
