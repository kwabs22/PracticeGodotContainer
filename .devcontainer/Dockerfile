# Base your image on an official distribution (e.g., Ubuntu 22.04)
FROM ubuntu:22.04

# Avoid user interaction during package installs
ENV DEBIAN_FRONTEND=noninteractive

# Update packages and install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    unzip \
    ca-certificates \
    libglu-dev \
    # Add other packages you need, e.g., for your Godot scripts or CI tasks
    && rm -rf /var/lib/apt/lists/*

# Download Godot (4.0.3-stable as an example) - Adjust if needed
RUN wget https://downloads.tuxfamily.org/godotengine/4.0.3/Godot_v4.0.3-stable_linux.x86_64.zip -O /tmp/godot.zip \
    && unzip /tmp/godot.zip -d /tmp \
    && mv /tmp/Godot_v4.0.3-stable_linux.x86_64 /usr/local/bin/godot \
    && chmod +x /usr/local/bin/godot \
    && rm /tmp/godot.zip

# Create a non-root user "vscode" with sudo access so Codespaces can attach
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update && apt-get install -y sudo \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/99_$USERNAME \
    && rm -rf /var/lib/apt/lists/*

# Switch to the vscode user
USER $USERNAME

# By default, this container will open in /workspaces/<repo>
WORKDIR /workspaces

# For convenience, set the entrypoint to bash. In Codespaces,
# it’ll override with its own settings, but this is a safe default.
CMD [ "/bin/bash" ]
