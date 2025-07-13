FROM cgr.dev/chainguard/node:latest-dev

USER root

RUN <<EOT

    apk update && apk add posix-libc-utils && ldconfig
    apk add zsh zsh-vcs curl wget git vim make jq docker coreutils
    apk add sudo-rs shadow && echo "node ALL = (ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers && echo y | pwck -q || true

    # Create docker group and add node user to it
    sudo addgroup docker
    sudo addgroup node docker

    curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o ~/awscliv2.zip
    unzip ~/awscliv2.zip
    sudo ./aws/install --bin-dir /usr/bin --install-dir /usr/aws-cli --update

    npm install -g @anthropic-ai/claude-code
    npm install -g @google/gemini-cli

EOT

USER node

RUN <<EOT

    # Install default shell
    command -v zsh | sudo tee -a /etc/shells
    chsh node --shell /usr/bin/zsh
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

EOT

# Create entrypoint script
RUN <<EOT

cat <<'EOF' > /home/node/entrypoint.sh && chmod +x /home/node/entrypoint.sh
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

COPY .devcontainer/dotfiles/* /home/node

ENTRYPOINT ["/home/node/entrypoint.sh"]
