# Bon-suv
## Avatarify
Bon-suv is a project designed to For full avatarify installation, managing it package 
# Avatarify Python

![](docs/mona.gif)

**Photorealistic avatars for video-conferencing**

Avatarify Python is a cutting-edge deep learning application that replaces your face with a photorealistic avatar in real-time during video conferences. It harnesses state-of-the-art computer vision and generative models to create immersive virtual avatars that track your facial movements and expressions naturally.

---

## Table of Contents

- [Project Definition](#project-definition)
- [Key Features](#key-features)
- [System Requirements](#system-requirements)
- [Installation](#installation)
  - [Linux Setup](#linux-setup)
  - [macOS Setup](#macos-setup)
  - [Windows Setup](#windows-setup)
  - [Docker Setup](#docker-setup)
  - [Google Colab Setup](#google-colab-setup)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Advanced Features](#advanced-features)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Resources](#resources)

---

## Project Definition

### Overview

Avatarify Python is a Python-based implementation of photorealistic avatar generation technology, designed to enhance video conferencing experiences. It allows users to replace their physical appearance with a virtual avatar while maintaining natural facial expressions and head movements.

### Technology Stack

- **AI Model**: Based on [First Order Motion Model](https://github.com/AliaksandrSiarohin/first-order-model) for efficient pose-guided image animation
- **Framework**: PyTorch for deep learning inference
- **Language**: Python 3.6+
- **Platforms**: Linux, macOS, Windows
- **Deployment Options**: Local, Docker, Google Colab, Remote GPU

### Architecture

```
Avatarify Python Architecture
├── Video Input (Webcam)
├── Face Detection & Keypoint Extraction
├── Motion Model Inference
├── Avatar Rendering
└── Video Output (Virtual Webcam)
```

### Core Components

- **afy/**: Core library with model inference and utilities
- **avatars/**: Pre-trained avatar models and StyleGAN-generated avatars
- **scripts/**: Utility scripts for model downloading and setup
- **run.sh / run_mac.sh / run_windows.bat**: Platform-specific execution scripts

---

## Key Features

✨ **Real-time Avatar Generation**
- Process video stream with minimal latency
- Support for multiple avatar styles and characters
- Smooth facial expression transfer

🎭 **Multiple Avatar Options**
- Pre-trained celebrity and famous person avatars
- StyleGAN-generated synthetic faces (press `Q` to randomize)
- Custom avatar support

📱 **Cross-Platform Support**
- Linux with GPU acceleration
- macOS native support
- Windows with CUDA support
- Docker containerization for consistent environments

🔧 **Flexible Deployment**
- Local GPU processing
- CPU-only mode for testing
- Remote GPU support for distributed computing
- Google Colab integration (no GPU required)

🎥 **Video Conference Integration**
- Works with Zoom, Skype, Teams, Slack, and other platforms
- Virtual camera output on Linux and Windows
- Screen sharing compatible

📦 **Production Ready**
- Optimized inference pipeline
- Memory-efficient processing
- Real-time performance (30+ FPS on modern GPUs)

---

## System Requirements

### Minimum Requirements

| Component | Specification |
|-----------|---------------|
| **OS** | Linux, macOS 10.14+, or Windows 10+ |
| **Python** | 3.6 - 3.9 |
| **RAM** | 8 GB |
| **Storage** | 4 GB free space |

### Recommended Requirements

| Component | Specification |
|-----------|---------------|
| **OS** | Linux (Ubuntu 18.04+) |
| **GPU** | NVIDIA with CUDA Compute Capability 6.0+ (RTX 2060 or better) |
| **CUDA** | 11.0+ |
| **cuDNN** | 8.0+ |
| **RAM** | 16 GB |
| **Storage** | SSD with 5+ GB space |

### GPU Support

**NVIDIA CUDA**: Recommended for best performance
- RTX 2060 or better recommended
- ~30+ FPS real-time performance
- ~1 GB VRAM usage

**CPU Mode**: For testing and environments without GPU
- ~5-10 FPS performance
- Higher latency acceptable for non-real-time use

---

## Installation

### Prerequisites

1. **Install Python**
   ```bash
   # Verify Python 3.6+ is installed
   python --version
   ```

2. **Install Git**
   ```bash
   # Clone the repository
   git clone https://github.com/alievk/avatarify-python.git
   cd avatarify-python
   ```

3. **Install conda** (Recommended)
   - Download from [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
   - Provides isolated Python environments

### Linux Setup

#### Ubuntu 18.04 / 20.04 (GPU)

```bash
# Create conda environment
conda create -n avatarify python=3.8 -y

# Activate environment
conda activate avatarify

# Install dependencies
pip install -r requirements.txt

# For GPU support, install PyTorch with CUDA
conda install pytorch::pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia

# Download pre-trained models
cd scripts && bash download.sh && cd ..

# Run Avatarify
bash run.sh
```

#### Ubuntu 18.04 / 20.04 (CPU Only)

```bash
# Create conda environment
conda create -n avatarify python=3.8 -y
conda activate avatarify

# Install CPU-only PyTorch
conda install pytorch::pytorch torchvision torchaudio pytorch-cpu -c pytorch

# Install dependencies
pip install -r requirements_client.txt

# Download models
cd scripts && bash download.sh && cd ..

# Run Avatarify (CPU mode - slower performance)
bash run.sh
```

#### Install Virtual Camera (Linux)

```bash
# Install v4l2loopback for virtual camera support
sudo apt-get install v4l2loopback-dkms

# Load the kernel module
sudo modprobe v4l2loopback

# Verify installation
v4l2-ctl --list-devices
```

### macOS Setup

#### Intel Mac

```bash
# Create conda environment
conda create -n avatarify python=3.8 -y
conda activate avatarify

# Install PyTorch for macOS
conda install pytorch::pytorch torchvision torchaudio -c pytorch

# Install dependencies
pip install -r requirements.txt

# Download models
cd scripts && bash download.sh && cd ..

# Run Avatarify
bash run_mac.sh
```

#### Apple Silicon (M1/M2)

```bash
# Create conda environment with arm64 architecture
CONDA_SUBDIR=osx-arm64 conda create -n avatarify python=3.8 -y
conda activate avatarify

# Install PyTorch for ARM
conda install pytorch::pytorch torchvision torchaudio -c pytorch

# Install dependencies
pip install -r requirements.txt

# Download models
cd scripts && bash download.sh && cd ..

# Run with optimized settings
bash run_mac.sh
```

### Windows Setup

#### Windows 10/11 (GPU - NVIDIA)

1. **Install Visual C++ Build Tools**
   - Download from [Microsoft Visual C++ 14.0](https://visualstudio.microsoft.com/visual-cpp-build-tools/)

2. **Install CUDA and cuDNN**
   ```powershell
   # Download and install CUDA 11.8
   # https://developer.nvidia.com/cuda-11-8-0-download-wizard
   
   # Download cuDNN and extract to CUDA directory
   # https://developer.nvidia.com/cudnn
   ```

3. **Setup Python Environment**
   ```powershell
   # Create conda environment
   conda create -n avatarify python=3.8 -y
   conda activate avatarify
   
   # Install PyTorch with CUDA
   conda install pytorch::pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
   
   # Install dependencies
   pip install -r requirements.txt
   ```

4. **Download Models**
   ```powershell
   # Windows Subsystem for Linux (WSL2) or using manual download
   cd scripts
   # Download models using browser or curl
   ```

5. **Run Avatarify**
   ```powershell
   run_windows.bat
   ```

#### Windows 10/11 (CPU Only)

```powershell
# Create conda environment
conda create -n avatarify python=3.8 -y
conda activate avatarify

# Install CPU PyTorch
conda install pytorch::pytorch torchvision torchaudio pytorch-cpu -c pytorch

# Install dependencies
pip install -r requirements_client.txt

# Run
run_windows.bat
```

### Docker Setup

Docker provides a containerized environment ensuring consistency across systems.

```bash
# Build the Docker image
docker build -t avatarify:latest .

# Run with GPU support
docker run --gpus all -it \
  -e DISPLAY=$DISPLAY \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  avatarify:latest

# Run CPU-only version
docker run -it avatarify:latest bash run.sh --cpu
```

**Docker Compose** (Optional)

Create `docker-compose.yml`:
```yaml
version: '3.8'
services:
  avatarify:
    build: .
    runtime: nvidia
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix:rw
    environment:
      - DISPLAY=$DISPLAY
```

Run:
```bash
docker-compose up
```

### Google Colab Setup

Run Avatarify in Google Colab **without requiring a local GPU**:

1. Open the notebook: [avatarify.ipynb](https://colab.research.google.com/github/alievk/avatarify/blob/master/avatarify.ipynb)

2. Follow the cells to:
   - Install dependencies
   - Download pre-trained models
   - Run avatar generation in the cloud

3. Download output videos directly from Colab

---

## Quick Start

### 1. Activate Environment

```bash
# Linux/macOS
conda activate avatarify

# Windows
conda activate avatarify
```

### 2. Run Avatarify

```bash
# Linux
bash run.sh

# macOS
bash run_mac.sh

# Windows
run_windows.bat
```

### 3. Select Avatar

When the application starts:
- Use **arrow keys** to navigate through available avatars
- Press **Space** to select an avatar
- Press **Q** for random StyleGAN-generated avatars

### 4. Integrate with Video Conference

**Zoom/Skype/Teams:**
- Select Avatarify virtual camera as video input
- On Windows: Use OBS Studio to bridge the output

**Slack:**
- Use the Zoom/Skype integration method

### 5. Stop Application

- Press **ESC** or **Q** to exit cleanly

---

## Configuration

### Config File: `config.yaml`

```yaml
# Avatar configuration
avatar_path: "avatars/"
default_avatar: "mona.png"

# Model settings
model_checkpoint: "var/checkpoints/vox-adv-cpk.pth.tar"
model_device: "cuda"  # or "cpu"

# Performance settings
batch_size: 1
image_size: 512
use_cuda: true

# Video settings
fps: 30
resolution: [640, 480]
```

### Runtime Parameters

```bash
# Run with specific avatar
bash run.sh --avatar avatars/custom.png

# CPU-only mode (slower)
bash run.sh --cpu

# Lower resolution (faster on weak GPUs)
bash run.sh --resolution 480

# Custom model path
bash run.sh --checkpoint path/to/model.pth

# Verbose logging
bash run.sh --verbose
```

---

## Advanced Features

### Remote GPU Support

Deploy inference on a remote machine for better performance:

1. **Setup Remote Server**
   ```bash
   # On remote machine with GPU
   pip install -r requirements.txt
   python -m afy.server --port 5000
   ```

2. **Run Local Client**
   ```bash
   # On local machine
   bash run.sh --server 192.168.1.100:5000
   ```

Deployment instructions: [Remote GPU Wiki](https://github.com/alievk/avatarify-python/wiki/Remote-GPU)

### StyleGAN Avatars

Generate synthetic avatars that never existed:

- Press **Q** during runtime to sample new StyleGAN avatars
- Each press generates a unique face
- Fully compatible with the motion model

### Custom Avatar Upload

Create avatars from your own images:

```bash
# 1. Prepare image (clear face, well-lit)
# 2. Place in avatars/ directory
# 3. Run and select from menu
```

Recommended image specifications:
- Resolution: 512x512 pixels
- Format: PNG or JPEG
- Clear frontal face view
- Good lighting

### Video Export

Record avatar video output:

```bash
# Using FFmpeg
ffmpeg -f x11grab -i :0 -f pulse -i default \
  -c:v libx264 -crf 23 output.mp4
```

---

## Troubleshooting

### Common Issues

#### 1. "No module named 'torch'"
```bash
# Reinstall PyTorch
conda install pytorch -c pytorch -y
```

#### 2. "CUDA out of memory"
```bash
# Run with lower resolution
bash run.sh --resolution 480

# Or use CPU mode
bash run.sh --cpu
```

#### 3. "Virtual camera not found" (Linux)
```bash
# Reload v4l2loopback module
sudo rmmod v4l2loopback
sudo modprobe v4l2loopback
```

#### 4. "Model checkpoint not found"
```bash
# Download models
cd scripts && bash download.sh && cd ..
```

#### 5. "Cannot open camera"
```bash
# Check camera permissions
# Linux: Add user to video group
sudo usermod -a -G video $USER

# macOS: Grant camera access in System Preferences
```

#### 6. Performance Issues (Low FPS)
- Reduce resolution: `bash run.sh --resolution 480`
- Use GPU: Ensure CUDA is properly installed
- Close unnecessary applications
- Monitor GPU usage: `nvidia-smi`

### Getting Help

- **GitHub Issues**: https://github.com/alievk/avatarify-python/issues
- **Slack Community**: [Join Slack](https://join.slack.com/t/avatarify/shared_invite/zt-dyoqy8tc-~4U2ObQ6WoxuwSaWKKVOgk)
- **Documentation**: Check `/docs` folder

---

## Project Structure

```
avatarify-python/
├── afy/                      # Core library
│   ├── __init__.py
│   ├── arguments.py
│   ├── predictor.py         # Model inference
│   └── util.py              # Utility functions
├── avatars/                 # Avatar models and images
│   └── *.png               # Avatar images
├── scripts/                 # Setup and utility scripts
│   └── download.sh         # Download pre-trained models
├── var/                    # Models and checkpoints
│   └── checkpoints/        # Pre-trained weights
├── docs/                   # Documentation and images
│   ├── README.md          # Installation guide
│   ├── mona.gif           # Demo GIF
│   └── *.jpg              # Screenshots
├── config.yaml            # Configuration file
├── requirements.txt       # Python dependencies
├── requirements_client.txt # Lightweight dependencies
├── run.sh                # Linux launcher
├── run_mac.sh           # macOS launcher
├── run_windows.bat      # Windows launcher
├── Dockerfile           # Docker configuration
└── README.md            # This file
```

---

## Dependencies

### Core Requirements

```
torch>=1.7.0
torchvision>=0.8.0
numpy>=1.19.0
opencv-python>=4.5.0
PyYAML>=5.3.0
imageio>=2.9.0
imageio-ffmpeg>=0.4.5
```

### Optional Dependencies

```
# For Jupyter notebook support
jupyter>=1.0.0
ipython>=7.0.0

# For GPU optimization
torch-cuda>=11.0
tensorrt>=8.0.0

# For remote deployment
flask>=2.0.0
requests>=2.25.0
```

View full dependency lists:
- Main: `requirements.txt`
- Lightweight: `requirements_client.txt`

---

## Advanced Features & Tips

### Performance Optimization

1. **Reduce Processing Resolution**
   ```bash
   bash run.sh --resolution 480  # Default: 512
   ```

2. **Batch Processing**
   - Process multiple frames simultaneously on high-end GPUs
   - Modify `config.yaml` batch_size parameter

3. **Model Quantization**
   - Convert to INT8 for faster inference
   - Supported on NVIDIA GPUs with TensorRT

### Multi-User Deployment

1. **Server Mode** (Enterprise)
   ```bash
   python -m afy.server --port 5000 --workers 4
   ```

2. **Load Balancing**
   - Deploy multiple instances
   - Use Nginx for distribution

---

## Contributing

We welcome contributions! Please follow these steps:

1. **Fork the repository**
   ```bash
   git clone https://github.com/yourusername/avatarify-python.git
   cd avatarify-python
   ```

2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes** and commit
   ```bash
   git add .
   git commit -m "Add descriptive commit message"
   ```

4. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Create a Pull Request**
   - Provide clear description of changes
   - Link related issues

### Contribution Guidelines

- Follow Python PEP 8 style guide
- Add tests for new features
- Update documentation accordingly
- Test on multiple platforms if possible

---

## News & Updates

- **7 Mar 2021**: Renamed project to Avatarify Python to distinguish from other versions
- **14 December 2020**: Released Avatarify Desktop for easier installation
- **11 July 2020**: Added Docker support for Linux deployment
- **22 May 2020**: Google Colab integration (run without GPU)
- **7 May 2020**: Remote GPU support for all platforms
- **24 April 2020**: Windows installation tutorial released
- **17 April 2020**: Slack community created
- **15 April 2020**: StyleGAN-generated avatars feature added
- **13 April 2020**: Full Windows support added

---

## Alternative Versions

We maintain other implementations for different use cases:

- **[Avatarify Desktop](https://github.com/alievk/avatarify-desktop)** - Easier to install and use (Recommended for most users)
- **[Avatarify iOS](https://apps.apple.com/app/apple-store/id1512669147)** - iPhone/iPad application
- **[Avatarify Android](https://play.google.com/store/apps/details?id=com.avatarify.android)** - Android application

---

## Acknowledgedments

- **First Order Motion Model**: [AliaksandrSiarohin/first-order-model](https://github.com/AliaksandrSiarohin/first-order-model)
- **StyleGAN**: [NVlabs/stylegan](https://github.com/NVlabs/stylegan)
- **Contributors**: [mikaelhg](https://github.com/mikaelhg), [mintmaker](https://github.com/mintmaker), [mynameisfiberin](https://github.com/mynameisfiberin), [9of9](https://github.com/9of9)

---

## License

This project is licensed under the **Creative Commons Attribution-NonCommercial 4.0 International License**.

**Key Terms:**
- ✅ Personal use permitted
- ✅ Commercial use with permission
- ✅ Educational purposes
- ❌ Commercial redistribution without attribution
- ❌ Removal of license notices

See [LICENSE.md](LICENSE.md) for full details.

---

## FAQ

**Q: Do I need a GPU?**
A: A GPU is recommended for real-time performance (30+ FPS). CPU mode works but at ~5-10 FPS.

**Q: Which GPUs are supported?**
A: NVIDIA GPUs with CUDA Compute Capability 6.0+ (GTX 1060 or better). RTX series recommended.

**Q: Can I use custom avatars?**
A: Yes! Place images in the `avatars/` folder and select from the menu.

**Q: Is this compatible with Zoom/Teams/Skype?**
A: Yes! Works on Linux and Windows. macOS support requires additional setup.

**Q: How much storage is required?**
A: ~4 GB for models and dependencies, 1-2 GB for avatar images.

**Q: Can I run this on Cloud Platforms?**
A: Yes! Google Colab and AWS EC2 with GPU support.

**Q: Is this real-time?**
A: On modern GPUs: 30+ FPS (real-time). CPU mode: ~5-10 FPS.

---# Avatarify Python

![](docs/mona.gif)

**Photorealistic avatars for video-conferencing**

Avatarify Python is a cutting-edge deep learning application that replaces your face with a photorealistic avatar in real-time during video conferences. It harnesses state-of-the-art computer vision and generative models to create immersive virtual avatars that track your facial movements and expressions naturally.

---

## Table of Contents

- [Project Definition](#project-definition)
- [Key Features](#key-features)
- [System Requirements](#system-requirements)
- [Installation](#installation)
  - [Linux Setup](#linux-setup)
  - [macOS Setup](#macos-setup)
  - [Windows Setup](#windows-setup)
  - [Docker Setup](#docker-setup)
  - [Google Colab Setup](#google-colab-setup)
  - [Installation Troubleshooting](#installation-troubleshooting)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Advanced Features](#advanced-features)
- [Performance Optimization Guide](#performance-optimization-guide)
- [Security, Privacy & Ethics](#security-privacy--ethics)
- [Troubleshooting](#troubleshooting)
- [Glossary](#glossary)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)
- [Resources](#resources)

---

## Project Definition

### Overview

Avatarify Python is a Python-based implementation of photorealistic avatar generation technology, designed to enhance video conferencing experiences. It allows users to replace their physical appearance with a virtual avatar while maintaining natural facial expressions and head movements.

### Technology Stack

- **AI Model**: Based on [First Order Motion Model](https://github.com/AliaksandrSiarohin/first-order-model) for efficient pose-guided image animation
- **Framework**: PyTorch for deep learning inference
- **Language**: Python 3.8+
- **Platforms**: Linux, macOS, Windows
- **Deployment Options**: Local, Docker, Google Colab, Remote GPU

### Architecture


## Contact & Social

- **GitHub**: https://github.com/alievk/avatarify-python
- **Issues**: https://github.com/alievk/avatarify-python/issues
- **Slack Community**: [Join Now](https://join.slack.com/t/avatarify/shared_invite/zt-dyoqy8tc-~4U2ObQ6WoxuwSaWKKVOgk)

---

## Disclaimer

Avatarify Python is provided "as is" without warranty. Users are responsible for:
- Ethical use of avatar technology
- Compliance with local laws and regulations
- Proper disclosure when using avatars in professional settings
- Respect for privacy and consent of others

---

**Made with ❤️ by the Avatarify Community**

*Last Updated: March 2026*
