# deven-rust
Rust Development Environment Template

A containerized Rust development environment template that can be easily integrated into any Rust project. This template provides a complete development setup with VS Code dev container support, essential tools, and optimized configurations for modern Rust development.

## Features

- **🦀 Rust Development**: Based on Chainguard's security-hardened Rust image
- **📦 Dev Container Ready**: Complete VS Code dev container integration
- **🛠️ Essential Tools**: Pre-configured with:
  - Latest Rust toolchain (rustc, cargo, rustfmt, clippy)
  - Rust Analyzer for VS Code
  - Docker-in-Docker support
  - AWS CLI v2
  - Claude Code AI assistant
  - Google Gemini CLI
  - Oh My Zsh with Apple theme
- **🔒 Security-First**: Built on Chainguard's distroless base for minimal attack surface
- **🌐 Multi-Platform**: Supports both x64 and ARM64 architectures
- **⚡ Fast Setup**: One-command installation into existing projects

## Installation

### Option 1: Add to Existing Project (Recommended)

Add this development environment to your existing Rust project:

```bash
# Navigate to your Rust project root
cd your-rust-project

# Install the development environment
rm -rf .devcontainer
git clone --depth=1 https://github.com/rennochj/deven-rust.git .devcontainer
rm -rf .devcontainer/.git
```

### Option 2: Start New Project

Clone this template to start a new Rust project:

```bash
git clone https://github.com/rennochj/deven-rust.git my-rust-project
cd my-rust-project
rm -rf .git
git init
```

### Prerequisites

- **Docker**: Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- **VS Code**: Install [Visual Studio Code](https://code.visualstudio.com/)
- **Dev Containers Extension**: Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## Quick Start

1. **Open Project**: Open your project folder in VS Code
2. **Reopen in Container**: 
   - VS Code will detect the dev container and prompt you to "Reopen in Container"
   - Or use Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`) → "Dev Containers: Reopen in Container"
3. **Wait for Build**: The container will build automatically (first time takes a few minutes)
4. **Start Coding**: Your Rust development environment is ready!

## Project Structure

```
.devcontainer/
├── Dockerfile              # Multi-stage Rust development container
├── devcontainer.json       # VS Code dev container configuration  
├── docker-compose.yml      # Container orchestration
├── dotfiles/
│   └── .zshrc             # Shell configuration with Oh My Zsh
└── README.md              # This documentation
```

## VS Code Extensions Included

- **rust-lang.rust-analyzer**: Advanced Rust language support with IntelliSense
- **tamasfe.even-better-toml**: Enhanced TOML editing for Cargo.toml files
- **ms-azuretools.vscode-docker**: Docker integration and management
- **shd101wyy.markdown-preview-enhanced**: Advanced Markdown editing
- **yzhang.markdown-all-in-one**: Complete Markdown toolkit
- **hediet.vscode-drawio**: Diagram creation and editing
- **streetsidesoftware.code-spell-checker**: Spell checking for code and docs
- **amazonwebservices.aws-toolkit-vscode**: AWS development tools
- **mechatroner.rainbow-csv**: CSV file editing and visualization

## Container Features

- **🔐 Security**: Non-root user (`nonroot`) with sudo access
- **🐚 Shell**: Zsh with Oh My Zsh and Apple theme
- **☁️ AWS Ready**: AWS CLI v2 with credential mounting (`~/.aws`)
- **🐳 Docker Support**: Full Docker-in-Docker capability
- **📦 Package Management**: Cargo for Rust dependencies
- **🕒 Timezone**: Configurable timezone (default: America/Chicago)

## Getting Started with Rust

Once your container is running, you can immediately start developing:

```bash
# Check Rust installation
cargo --version
rustc --version

# Create a new Rust project (if not existing)
cargo new hello-world
cd hello-world

# Build and run
cargo run

# Run tests
cargo test

# Format code
cargo fmt

# Lint code with Clippy
cargo clippy

# Add dependencies
cargo add serde tokio

# Build for release
cargo build --release
```

## AI-Powered Development

The environment includes AI coding assistants:

```bash
# Use Claude Code (Anthropic)
claude-code

# Use Gemini CLI (Google)
gemini-cli
```

## Docker Integration

Develop with Docker inside the container:

```bash
# Build Docker images
docker build -t my-rust-app .

# Run containers
docker run --rm -it my-rust-app

# Docker Compose
docker-compose up -d
```

## AWS Development

AWS CLI is pre-installed and configured:

```bash
# Configure AWS (credentials mounted from ~/.aws)
aws configure

# Use AWS services
aws s3 ls
aws lambda list-functions
```

## Customization

### Modify Shell Configuration
Edit `.devcontainer/dotfiles/.zshrc` to customize your shell:

```bash
# Add custom aliases
alias ll='ls -la'
alias gc='git commit'

# Add custom environment variables
export RUST_LOG=debug
```

### Add VS Code Extensions
Edit `.devcontainer/devcontainer.json`:

```json
{
  "customizations": {
    "vscode": {
      "extensions": [
        "rust-lang.rust-analyzer",
        "your-extension-id"
      ]
    }
  }
}
```

### Install Additional Tools
Modify `.devcontainer/Dockerfile`:

```dockerfile
# Add your tools after line 21
RUN apk add your-package-name
```

## Advanced Configuration

### Environment Variables
Configure in `.devcontainer/devcontainer.json`:

```json
{
  "containerEnv": {
    "TZ": "America/Chicago",
    "RUST_LOG": "info",
    "CARGO_TERM_COLOR": "always"
  }
}
```

### Volume Mounts
Add persistent volumes in `.devcontainer/devcontainer.json`:

```json
{
  "mounts": [
    "source=rust-cargo-cache,target=/usr/local/cargo/registry,type=volume"
  ]
}
```

### Post-Creation Commands
Uncomment and modify in `.devcontainer/devcontainer.json`:

```json
{
  "postCreateCommand": "cargo --version && rustc --version"
}
```

## Troubleshooting

### Container Won't Start
```bash
# Rebuild container
Ctrl+Shift+P → "Dev Containers: Rebuild Container"

# Clear Docker cache
docker system prune -a
```

### Slow Performance
```bash
# Check Docker resources in Docker Desktop settings
# Increase memory allocation to 4GB+ for Rust compilation
```

### Permission Issues
```bash
# Fix file permissions
sudo chown -R nonroot:nonroot /workspaces
```

### AWS Credentials
```bash
# Ensure ~/.aws directory exists on host
mkdir -p ~/.aws
```

## Contributing

1. Fork this repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add some amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Chainguard](https://chainguard.dev/) for the secure base images
- [Microsoft](https://github.com/microsoft/vscode-dev-containers) for VS Code Dev Containers
- [Rust Foundation](https://foundation.rust-lang.org/) for the amazing Rust ecosystem