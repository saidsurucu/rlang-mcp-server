# Python R-Server MCP

R veri görselleştirmesi ve analizi için Python ve FastMCP ile geliştirilmiş kapsamlı bir Model Context Protocol (MCP) sunucusu.

*[English](README.md) | **Türkçe***

## Genel Bakış

Bu proje [gdbelvin'in rlang-mcp-server](https://github.com/gdbelvin/rlang-mcp-server) projesinden ilham alınmıştır ancak FastMCP framework kullanılarak tamamen Python'da yeniden yazılmıştır. Orijinal Go versiyonu temel R görselleştirme araçları sağlarken, bu Python versiyonu kapsamlı dosya yönetimi yetenekleri ve gelişmiş kullanıcı deneyimi ile işlevselliği genişletmiştir.

## Özellikler

### 🎨 **Görselleştirme ve Analiz**
- **ggplot2 Render**: ggplot2 komutları ile R kodu çalıştırarak yayın kalitesinde görselleştirmeler oluşturun
- **R Script Çalıştırma**: Akıllı dosya işleme ile herhangi bir R scripti çalıştırın ve formatlanmış çıktı alın
- **Çoklu Format**: PNG, JPEG, PDF ve SVG çıktı formatları desteği
- **Özelleştirilebilir Çıktı**: Görüntü boyutları, çözünürlük ve kalite kontrolü

### 📁 **Dosya Yönetimi** (Yeni!)
- **Dosya Yükleme**: Excel, CSV, JSON ve metin dosyalarını R çalışma alanına yükleyin
- **Dosya Listeleme**: Çalışma alanındaki dosyaları detaylı metadata ile tarayın ve filtreleyin
- **Dosya İnceleme**: Excel sayfa yapısı dahil dosyalar hakkında detaylı bilgi alın
- **Akıllı Dosya Keşfi**: Otomatik dosya tespit ve eksik dosyalar için öneri

### 📂 **Dizin Yönetimi** (Yeni!)
- **Dinamik Mount**: Herhangi bir yerel dizini R işlemleri için monte edin
- **Güvenli Erişim**: Absolute path kontrolü ve permission doğrulaması
- **Otomatik Workspace**: r_workspace alt dizini otomatik olarak oluşturulur

### 📦 **Paket Yönetimi**
- **Paket Kurulumu**: Versiyon kontrolü ile isteğe bağlı R paketi kurulumu
- **Paket Listeleme**: Filtreleme yetenekleri ile kurulu paketleri tarayın
- **Otomatik Bağımlılık**: Akıllı paket bağımlılık çözümü

### 🛡️ **Güvenlik ve İzolasyon**
- **Docker Desteği**: Gelişmiş güvenlik için zorunlu konteyner çalıştırma
- **Dosya Tipi Doğrulama**: Whitelist tabanlı dosya yükleme güvenliği
- **Boyut Limitleri**: Yapılandırılabilir dosya boyutu kısıtlamaları
- **Path Sanitization**: Directory traversal saldırılarına karşı koruma

### 🚀 **Geliştirici Deneyimi**
- **FastMCP Framework**: Mükemmel performans ile modern Python MCP implementasyonu
- **uv Paket Yöneticisi**: Yıldırım hızında bağımlılık yönetimi ve sanal ortamlar
- **Kapsamlı Test**: Entegrasyon ve birim testleri ile tam test paketi
- **Zengin Dokümantasyon**: Detaylı API dokümantasyonu ve kullanım örnekleri

## Hızlı Başlangıç

### Ön Koşullar

MCP sunucusunu kurmadan önce, sisteminize gerekli bağımlılıkları kurmanız gerekir:

#### macOS

```bash
# Homebrew kurulu değilse kurun
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# R'yi kurun
brew install r

# uv (Python paket yöneticisi) kurun
brew install uv

# Python 3.12+ kurun (kurulu değilse)
brew install python@3.12

# Gerekli R paketlerini kurun
Rscript -e "install.packages(c('ggplot2', 'cowplot', 'readxl', 'writexl', 'dplyr', 'tidyr'), repos='https://cran.r-project.org')"

# Docker kurun (güvenli çalıştırma için gerekli)
brew install --cask docker
```

#### Windows

```powershell
# R'yi CRAN'den kurun
# Şuradan indirip kurun: https://cran.r-project.org/bin/windows/base/

# Python 3.12+'ı python.org'dan kurun
# Şuradan indirin: https://www.python.org/downloads/windows/

# uv'yi pip ile kurun
pip install uv

# Gerekli R paketlerini kurun (R konsolu veya RStudio'da çalıştırın)
install.packages(c('ggplot2', 'cowplot', 'readxl', 'writexl', 'dplyr', 'tidyr'), repos='https://cran.r-project.org')

# Docker Desktop kurun (güvenli çalıştırma için gerekli)
# Şuradan indirin: https://www.docker.com/products/docker-desktop
```

#### Linux (Ubuntu/Debian)

```bash
# Paket listesini güncelleyin
sudo apt update

# R'yi kurun
sudo apt install r-base r-base-dev

# Python 3.12+ kurun
sudo apt install python3.12 python3.12-venv python3-pip

# uv kurun
pip install uv

# Gerekli R paketlerini kurun
sudo Rscript -e "install.packages(c('ggplot2', 'cowplot', 'readxl', 'writexl', 'dplyr', 'tidyr'), repos='https://cran.r-project.org')"

# Docker kurun (güvenli çalıştırma için gerekli)
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### Kurulum

Ön koşulları kurduktan sonra MCP sunucusunu kurun:

```bash
# Yöntem 1: Doğrudan GitHub'dan kurulum (önerilen)
uvx --from git+https://github.com/saidsurucu/rlang-mcp-python rlang-mcp-python

# Yöntem 2: Clone edip yerel kurulum
git clone https://github.com/saidsurucu/rlang-mcp-python.git
cd rlang-mcp-python
uv sync

# Yöntem 3: pip ile kurulum
pip install git+https://github.com/saidsurucu/rlang-mcp-python
```

### Sistem Gereksinimleri

- **Python 3.12+**
- **R 4.0+** ve paketler: ggplot2, cowplot, readxl, writexl, dplyr, tidyr
- **uv** (önerilen) veya pip paket yönetimi için
- **Docker** (güvenli konteyner çalıştırma için gerekli)

### Sunucuyu Çalıştırma

```bash
# uvx kullanarak (önerilen)
uvx --from . r-server-mcp

# Veya uv run kullanarak
uv run r-server-mcp

# Veya doğrudan Python ile
python -m r_server
```

## Mevcut Araçlar

Bu sunucu **8 kapsamlı araç** sunar:

| Araç | Açıklama | Kategori |
|------|----------|----------|
| `mount_directory` | R işlemleri için yerel dizin monte et | Dizin Yönetimi |
| `upload_file` | R çalışma alanına dosya yükle | Dosya Yönetimi |
| `list_files` | Çalışma alanı dosyalarını listele ve filtrele | Dosya Yönetimi |  
| `file_info` | Detaylı dosya bilgisi al | Dosya Yönetimi |
| `render_ggplot` | ggplot2 görselleştirmeleri oluştur | Görselleştirme |
| `execute_r_script` | Akıllı dosya işleme ile R scriptleri çalıştır | Çalıştırma |
| `install_r_package` | İsteğe bağlı R paketi kur | Paket Yönetimi |
| `list_r_packages` | Kurulu paketleri listele ve ara | Paket Yönetimi |

## MCP Entegrasyonu

### Claude Desktop Konfigürasyonu

`claude_desktop_config.json` dosyanıza ekleyin:

```json
{
  "mcpServers": {
    "r-server-python": {
      "command": "uvx",
      "args": [
        "--from", 
        "git+https://github.com/saidsurucu/rlang-mcp-python",
        "rlang-mcp-python"
      ],
      "disabled": false
    }
  }
}
```

### Kullanım Örnekleri

**Dizin monte et ve yerel dosyalarla çalış:**
```python
# Yerel bir dizini monte et
mount_directory("/Users/you/Documents/r-projects/analysis")

# Mevcut dosyaları listele
list_files(file_type="excel")

# Monte edilen dosyalarla doğrudan çalış
execute_r_script("""
library(readxl)
# Dosyalar monte edilen dizinden erişilebilir
data <- read_excel("data.xlsx")
summary(data)
""")
```

**Dosya yükle ve analiz et:**
```python
# Çalışma alanına Excel dosyası yükle
upload_file(file_content="<base64_content>", filename="data.xlsx")

# Yüklenen veriyi analiz et
execute_r_script("""
library(readxl)
data <- read_excel("r_workspace/data.xlsx")
summary(data)
head(data)
""")
```

**Görselleştirme oluştur:**
```python
# ggplot2 görselleştirmesi oluştur
render_ggplot("""
library(ggplot2)
data(mtcars)
ggplot(mtcars, aes(x=wt, y=mpg, color=factor(cyl))) +
  geom_point(size=3) +
  geom_smooth(method="lm") +
  theme_minimal() +
  labs(title="Yakıt Verimliliği vs Ağırlık", 
       color="Silindir")
""", output_type="png", width=800, height=600)
```

## Docker Desteği

Güvenli çalıştırma için Docker gereklidir:

```bash
# Docker imajını oluştur
docker build -f Dockerfile.python -t r-server-mcp .

# Docker Compose ile çalıştır
docker-compose -f docker-compose.python.yml up
```

## Geliştirme

```bash
# Geliştirme bağımlılıklarını kur
uv sync --dev

# Testleri çalıştır
uv run pytest

# Linting çalıştır
uv run ruff check
uv run black --check .

# Tip kontrolü
uv run mypy r_server.py
```

## Sorun Giderme

### Yaygın Sorunlar

#### R bulunamıyor
```bash
# macOS: R'nin PATH'te olduğundan emin olun
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Windows: R'yi sistem PATH'ine ekleyin
# C:\Program Files\R\R-x.x.x\bin PATH environment variable'ına ekleyin

# Linux: R development paketlerini kurun
sudo apt install r-base-dev
```

#### Python versiyon sorunları
```bash
# Python versiyonunu kontrol edin
python --version

# uv ile belirli Python versiyonu kullanın
uv python install 3.12
uv python pin 3.12
```

#### R paket kurulum hataları
```bash
# macOS: Sistem bağımlılıklarını kurun
brew install harfbuzz fribidi
brew install --cask xquartz

# Ubuntu/Debian: Sistem bağımlılıklarını kurun
sudo apt install libcurl4-openssl-dev libssl-dev libxml2-dev
sudo apt install libharfbuzz-dev libfribidi-dev

# Windows: Binary paketler kullanın
# R konsolunda:
install.packages('ggplot2', type='binary')
```

#### uv komutu bulunamıyor
```bash
# uv'yi global olarak kurun
pip install --user uv

# Veya Unix sistemlerde curl kullanın
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows: pip kullanın veya GitHub releases'den indirin
```

### Doğrulama

Kurulumunuzu test edin:

```bash
# R kurulumunu test edin
Rscript -e "R.version.string"

# R paketlerini test edin
Rscript -e "library(ggplot2); library(readxl); cat('R paketleri OK\n')"

# Python/uv test edin
uv --version
python --version

# MCP sunucusunu test edin
uvx --from git+https://github.com/saidsurucu/rlang-mcp-python rlang-mcp-python --help
```

## Orijinal ile Karşılaştırma

| Özellik | Orijinal (Go) | Bu Versiyon (Python) |
|---------|---------------|----------------------|
| Temel Araçlar | 2 | **8** |
| Dizin Montajı | ❌ | ✅ |
| Dosya Yönetimi | ❌ | ✅ |
| Paket Yönetimi | ❌ | ✅ |
| Dosya Yükleme | ❌ | ✅ |
| Akıllı Dosya İşleme | ❌ | ✅ |
| Modern Framework | ❌ | ✅ (FastMCP) |
| Paket Yöneticisi | Go modules | **uv** |
| Test Paketi | Temel | **Kapsamlı** |

## Katkıda Bulunma

Katkılarınızı memnuniyetle karşılıyoruz! Lütfen katkı rehberlerimizi okuyun ve geliştirmeler için pull request gönderin.

## Lisans

Creative Commons Attribution-NonCommercial 4.0 International (CC-BY-NC 4.0)

Bu eser [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/) altında lisanslanmıştır.

## Teşekkürler

- [gdbelvin'in rlang-mcp-server](https://github.com/gdbelvin/rlang-mcp-server)'ından ilham alınmıştır
- [FastMCP](https://github.com/jlowin/fastmcp) ile geliştirilmiştir
- Hızlı Python paket yönetimi için [uv](https://github.com/astral-sh/uv) kullanılmıştır