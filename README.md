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

### Installation

```bash
# Clone the repository
git clone https://github.com/saidsurucu/rlang-mcp-python.git
cd rlang-mcp-python

# Install with uv (recommended)
uv sync

# Or install with pip
pip install -e .
```

### Requirements

- **Python 3.12+**
- **R 4.0+** with required packages
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