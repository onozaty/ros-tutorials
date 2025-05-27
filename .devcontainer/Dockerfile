# 公式ドキュメントに従ったROS 2 Jazzy インストール
# https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html

FROM ubuntu:24.04

# System setup - Set locale
RUN apt update && apt install -y locales && \
    locale-gen en_US en_US.UTF-8 && \
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8

# Enable required repositories
RUN apt update && apt install -y \
    software-properties-common \
    curl && \
    add-apt-repository universe

# Add ROS 2 GPG key
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

# Add repository to sources list
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Install development tools
RUN apt update && apt install -y ros-dev-tools

# Update repository caches and upgrade system
RUN apt update && apt upgrade -y

# Install ROS 2 (Desktop Install - Recommended)
RUN apt install -y ros-jazzy-desktop

# Initialize rosdep
# https://docs.ros.org/en/jazzy/Installation/Alternatives/Ubuntu-Install-Binary.html#linux-install-binary-install-missing-dependencies
RUN rosdep init

# Install additional development tools
RUN apt install -y \
    # GUI関連 (X11フォワーディング用)
    x11-apps

# ユーザー切り替え
USER ubuntu

# Setup environment - ROS 2環境の設定
RUN echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc && \
    echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc

# Update rosdep
# https://docs.ros.org/en/jazzy/Installation/Alternatives/Ubuntu-Install-Binary.html#linux-install-binary-install-missing-dependencies
RUN rosdep update

# エントリーポイント
CMD ["/bin/bash"]
