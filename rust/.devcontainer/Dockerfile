FROM cgr.dev/chainguard/rust:latest-dev

USER root

RUN <<EOT

    apk update && apk add posix-libc-utils && ldconfig
    apk add zsh zsh-vcs curl wget git vim make jq docker nodejs npm coreutils
    apk add sudo-rs shadow && echo "nonroot ALL = (ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers && echo y | pwck -q || true

    # Create docker group and add nonroot user to it
    sudo addgroup docker
    sudo addgroup nonroot docker

    curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o ~/awscliv2.zip
    unzip ~/awscliv2.zip
    sudo ./aws/install --bin-dir /usr/bin --install-dir /usr/aws-cli --update

    npm install -g @anthropic-ai/claude-code
    
EOT

USER nonroot

RUN <<EOT

    pip install --upgrade pip

    curl -LsSf https://astral.sh/uv/install.sh | sh

    command -v zsh | sudo tee -a /etc/shells
    chsh nonroot --shell /usr/bin/zsh
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

EOT

# Create entrypoint script
RUN <<EOT

cat <<'EOF' > /home/nonroot/entrypoint.sh && chmod +x /home/nonroot/entrypoint.sh
#!/bin/sh
set -e

# Setup docker socket if it exists
if [ -S /var/run/docker.sock ]; then
    sudo chown root:docker /var/run/docker.sock
fi

# Run command or default to sleep infinity
if [ $# -gt 0 ]; then
    exec "$@"
else
    exec sleep infinity
fi

EOT

COPY .devcontainer/dotfiles/* /home/nonroot

ENTRYPOINT ["/home/nonroot/entrypoint.sh"]
