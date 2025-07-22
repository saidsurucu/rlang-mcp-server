# Python R-Server MCP

A comprehensive Model Context Protocol (MCP) server for R data visualization and analysis, built with Python and FastMCP.

## Overview

This project is inspired by [gdbelvin's rlang-mcp-server](https://github.com/gdbelvin/rlang-mcp-server) but is a complete Python reimplementation using the FastMCP framework. While the original Go version provided basic R visualization tools, this Python version extends the functionality with comprehensive file management capabilities and enhanced user experience.

## Features

### 🎨 **Visualization & Analysis**
- **ggplot2 Rendering**: Execute R code with ggplot2 commands and return publication-ready visualizations
- **R Script Execution**: Run any R script with smart file handling and return formatted output
- **Multiple Formats**: Support for PNG, JPEG, PDF, and SVG output formats
- **Customizable Output**: Control image dimensions, resolution, and quality

### 📁 **File Management** (New!)
- **File Upload**: Upload Excel, CSV, JSON, and text files to the R workspace
- **File Listing**: Browse and filter files in the workspace with detailed metadata
- **File Inspection**: Get detailed information about files including Excel sheet structure
- **Smart File Discovery**: Automatic file detection and suggestion for missing files

### 📦 **Package Management**
- **Package Installation**: Install R packages on-demand with version control
- **Package Listing**: Browse installed packages with filtering capabilities
- **Automatic Dependencies**: Smart package dependency resolution

### 🛡️ **Security & Isolation**
- **Docker Support**: Optional containerized execution for enhanced security
- **File Type Validation**: Whitelist-based file upload security
- **Size Limits**: Configurable file size restrictions
- **Path Sanitization**: Protection against directory traversal attacks

### 🚀 **Developer Experience**
- **FastMCP Framework**: Modern Python MCP implementation with excellent performance
- **uv Package Manager**: Lightning-fast dependency management and virtual environments
- **Comprehensive Testing**: Full test suite with integration and unit tests
- **Rich Documentation**: Detailed API documentation and usage examples

## Quick Start

### Prerequisites

Before installing the MCP server, you need to install the required dependencies on your system:

#### macOS

```bash
# Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install R
brew install r

# Install uv (Python package manager)
brew install uv

# Install Python 3.12+ (if not already installed)
brew install python@3.12

# Install required R packages
Rscript -e "install.packages(c('ggplot2', 'cowplot', 'readxl', 'writexl', 'dplyr', 'tidyr'), repos='https://cran.r-project.org')"

# Optional: Install Docker for containerized execution
brew install --cask docker
```

#### Windows

```powershell
# Install R from CRAN
# Download and install from: https://cran.r-project.org/bin/windows/base/

# Install Python 3.12+ from python.org
# Download from: https://www.python.org/downloads/windows/

# Install uv using pip
pip install uv

# Install required R packages (run in R console or RStudio)
install.packages(c('ggplot2', 'cowplot', 'readxl', 'writexl', 'dplyr', 'tidyr'), repos='https://cran.r-project.org')

# Optional: Install Docker Desktop
# Download from: https://www.docker.com/products/docker-desktop
```

#### Linux (Ubuntu/Debian)

```bash
# Update package list
sudo apt update

# Install R
sudo apt install r-base r-base-dev

# Install Python 3.12+
sudo apt install python3.12 python3.12-venv python3-pip

# Install uv
pip install uv

# Install required R packages
sudo Rscript -e "install.packages(c('ggplot2', 'cowplot', 'readxl', 'writexl', 'dplyr', 'tidyr'), repos='https://cran.r-project.org')"

# Optional: Install Docker
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### Installation

After installing the prerequisites, install the MCP server:

```bash
# Method 1: Install directly from GitHub (recommended)
uvx --from git+https://github.com/saidsurucu/rlang-mcp-python rlang-mcp-python

# Method 2: Clone and install locally
git clone https://github.com/saidsurucu/rlang-mcp-python.git
cd rlang-mcp-python
uv sync

# Method 3: Install with pip
pip install git+https://github.com/saidsurucu/rlang-mcp-python
```

### System Requirements

- **Python 3.12+**
- **R 4.0+** with packages: ggplot2, cowplot, readxl, writexl, dplyr, tidyr
- **uv** (recommended) or pip for package management
- **Docker** (optional, for containerized execution)

### Running the Server

```bash
# Using uvx (recommended)
uvx --from . r-server-mcp

# Or using uv run
uv run r-server-mcp

# Or using Python directly
python -m r_server
```

## Tools Available

This server provides **8 comprehensive tools**:

| Tool | Description | Category |
|------|-------------|----------|
| `mount_directory` | Mount a local directory for R operations | Directory Management |
| `upload_file` | Upload files to R workspace | File Management |
| `list_files` | List and filter workspace files | File Management |  
| `file_info` | Get detailed file information | File Management |
| `render_ggplot` | Generate ggplot2 visualizations | Visualization |
| `execute_r_script` | Execute R scripts with smart file handling | Execution |
| `install_r_package` | Install R packages on-demand | Package Management |
| `list_r_packages` | List and search installed packages | Package Management |

## MCP Integration

### Claude Desktop Configuration

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "r-server-python": {
      "command": "uvx",
      "args": [
        "--from", 
        "/path/to/rlang-mcp-python",
        "r-server-mcp"
      ],
      "disabled": false
    }
  }
}
```

### Usage Examples

**Mount directory and work with local files:**
```python
# Mount a local directory
mount_directory("/Users/you/Documents/r-projects/analysis")

# List available files
list_files(file_type="excel")

# Work with mounted files directly
execute_r_script("""
library(readxl)
# Files are accessible from mounted directory
data <- read_excel("data.xlsx")
summary(data)
""")
```

**Upload and analyze data:**
```python
# Upload an Excel file to workspace
upload_file(file_content="<base64_content>", filename="data.xlsx")

# Analyze the uploaded data
execute_r_script("""
library(readxl)
data <- read_excel("r_workspace/data.xlsx")
summary(data)
head(data)
""")
```

**Create visualizations:**
```python
# Generate a ggplot2 visualization
render_ggplot("""
library(ggplot2)
data(mtcars)
ggplot(mtcars, aes(x=wt, y=mpg, color=factor(cyl))) +
  geom_point(size=3) +
  geom_smooth(method="lm") +
  theme_minimal() +
  labs(title="Fuel Efficiency vs Weight", 
       color="Cylinders")
""", output_type="png", width=800, height=600)
```

## Docker Support

For enhanced security and isolation:

```bash
# Build Docker image
docker build -f Dockerfile.python -t r-server-mcp .

# Run with Docker Compose
docker-compose -f docker-compose.python.yml up
```

## Development

```bash
# Install development dependencies
uv sync --dev

# Run tests
uv run pytest

# Run linting
uv run ruff check
uv run black --check .

# Type checking
uv run mypy r_server.py
```

## Troubleshooting

### Common Issues

#### R not found
```bash
# macOS: Ensure R is in PATH
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Windows: Add R to system PATH
# Add C:\Program Files\R\R-x.x.x\bin to your PATH environment variable

# Linux: Install R development packages
sudo apt install r-base-dev
```

#### Python version issues
```bash
# Check Python version
python --version

# Use specific Python version with uv
uv python install 3.12
uv python pin 3.12
```

#### R package installation failures
```bash
# macOS: Install system dependencies
brew install harfbuzz fribidi
brew install --cask xquartz

# Ubuntu/Debian: Install system dependencies
sudo apt install libcurl4-openssl-dev libssl-dev libxml2-dev
sudo apt install libharfbuzz-dev libfribidi-dev

# Windows: Use binary packages
# In R console:
install.packages('ggplot2', type='binary')
```

#### uv command not found
```bash
# Install uv globally
pip install --user uv

# Or use curl on Unix systems
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows: Use pip or download from GitHub releases
```

#### Permission denied errors
```bash
# Linux/macOS: Fix R library permissions
sudo chmod -R 755 /usr/local/lib/R
sudo chown -R $USER /usr/local/lib/R/site-library

# Or install packages in user library
# In R console:
.libPaths()  # Check library paths
install.packages('ggplot2', lib=.libPaths()[1])
```

#### Docker issues
```bash
# Start Docker service (Linux)
sudo systemctl start docker

# Add user to docker group (Linux)
sudo usermod -aG docker $USER
# Logout and login again

# macOS/Windows: Ensure Docker Desktop is running
```

### Verification

Test your installation:

```bash
# Test R installation
Rscript -e "R.version.string"

# Test R packages
Rscript -e "library(ggplot2); library(readxl); cat('R packages OK\n')"

# Test Python/uv
uv --version
python --version

# Test MCP server
uvx --from git+https://github.com/saidsurucu/rlang-mcp-python rlang-mcp-python --help
```

## Comparison with Original

| Feature | Original (Go) | This Version (Python) |
|---------|---------------|----------------------|
| Core Tools | 2 | **8** |
| Directory Mounting | ❌ | ✅ |
| File Management | ❌ | ✅ |
| Package Management | ❌ | ✅ |
| File Upload | ❌ | ✅ |
| Smart File Handling | ❌ | ✅ |
| Modern Framework | ❌ | ✅ (FastMCP) |
| Package Manager | Go modules | **uv** |
| Testing Suite | Basic | **Comprehensive** |

## Contributing

Contributions are welcome! Please read our contributing guidelines and submit pull requests for any enhancements.

## License

Creative Commons Attribution-NonCommercial 4.0 International (CC-BY-NC 4.0)

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/).

## Acknowledgments

- Inspired by [gdbelvin's rlang-mcp-server](https://github.com/gdbelvin/rlang-mcp-server)
- Built with [FastMCP](https://github.com/jlowin/fastmcp)
- Powered by [uv](https://github.com/astral-sh/uv) for fast Python package management