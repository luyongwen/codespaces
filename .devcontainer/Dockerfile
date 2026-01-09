# Use Ubuntu 22.04 as base image
FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Update package list and install dependencies
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    gnupg2 \
    software-properties-common \
    ca-certificates \
    apt-transport-https \
    lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Install Podman
RUN apt-get update && apt-get install -y \
    podman \
    && rm -rf /var/lib/apt/lists/*

# Install Buildah (container build tool)
RUN apt-get update && apt-get install -y \
    buildah \
    && rm -rf /var/lib/apt/lists/*

# Install Skopeo (container image management tool)
RUN apt-get update && apt-get install -y \
    skopeo \
    && rm -rf /var/lib/apt/lists/*

# Install additional useful tools
RUN apt-get update && apt-get install -y \
    podman-compose \
    crun \
    slirp4netns \
    fuse-overlayfs \
    && rm -rf /var/lib/apt/lists/*

# Configure podman for rootless operation
RUN mkdir -p /etc/containers && \
    echo 'unqualified-search-registries = ["docker.io"]' > /etc/containers/registries.conf

# Configure storage for podman
RUN mkdir -p /home/vscode/.config/containers && \
    chown -R vscode:vscode /home/vscode/.config

# Set up subuid and subgid for rootless containers
RUN echo "vscode:100000:65536" >> /etc/subuid && \
    echo "vscode:100000:65536" >> /etc/subgid

# Switch to vscode user
USER vscode

# Configure podman storage for vscode user
RUN mkdir -p /home/vscode/.config/containers && \
    echo '[storage]' > /home/vscode/.config/containers/storage.conf && \
    echo 'driver = "overlay"' >> /home/vscode/.config/containers/storage.conf && \
    echo 'runroot = "/tmp/containers-user-1000/storage"' >> /home/vscode/.config/containers/storage.conf && \
    echo 'graphroot = "/home/vscode/.local/share/containers/storage"' >> /home/vscode/.config/containers/storage.conf && \
    echo '[storage.options.overlay]' >> /home/vscode/.config/containers/storage.conf && \
    echo 'mount_program = "/usr/bin/fuse-overlayfs"' >> /home/vscode/.config/containers/storage.conf

# Switch back to root for any remaining setup
USER root

# Set the default command
CMD ["/bin/bash"]
