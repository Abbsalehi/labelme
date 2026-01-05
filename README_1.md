<h1 align="center">
  <img src="labelme/icons/icon-256.png" width="200" height="200"><br/>Labelme-SAM
</h1>

<h4 align="center">
  Image Annotation with Official Segment Anything Model (SAM)
</h4>

<div align="center">
  <a href="https://github.com/wkentaro/labelme"><img src="https://img.shields.io/badge/based%20on-labelme-blue"></a>
  <a href="https://github.com/facebookresearch/segment-anything"><img src="https://img.shields.io/badge/powered%20by-SAM-green"></a>
</div>

<div align="center">
  <a href="#installation"><b>Installation</b></a>
  | <a href="#usage"><b>Usage</b></a>
  | <a href="#features"><b>Features</b></a>
  | <a href="#sam-integration"><b>SAM Integration</b></a>
</div>

<br/>

<div align="center">
  <img src="examples/instance_segmentation/.readme/annotation.jpg" width="70%">
</div>

## Description

**Labelme-SAM** is a fork of the popular [labelme](https://github.com/wkentaro/labelme) annotation tool, enhanced with **official Segment Anything Model (SAM)** integration using local PyTorch checkpoints.

### Key Differences from Original Labelme

- ✅ **Official SAM**: Uses Meta's original [segment-anything](https://github.com/facebookresearch/segment-anything) implementation
- ✅ **Local Checkpoints**: No API calls, runs entirely offline with local model files
- ✅ **Multiple SAM Variants**: Support for ViT-H (high accuracy), ViT-L (balanced), and ViT-B (fast)
- ✅ **Automatic Downloads**: Model checkpoints download automatically on first use
- ✅ **GPU Acceleration**: Full CUDA support for fast segmentation
- ✅ **Simplified UI**: Streamlined interface focused on efficient annotation

### Why This Fork?

This fork replaces the osam dependency with the **official segment-anything** implementation from Facebook Research, providing:
- Direct access to Meta's original SAM models
- Better performance with local PyTorch checkpoints
- More control over model selection (ViT-H/L/B variants)
- No dependency on external APIs or services

<img src="examples/instance_segmentation/data_dataset_voc/JPEGImages/2011_000006.jpg" width="19%" /> <img src="examples/instance_segmentation/data_dataset_voc/SegmentationClass/2011_000006.png" width="19%" /> <img src="examples/instance_segmentation/data_dataset_voc/SegmentationClassVisualization/2011_000006.jpg" width="19%" /> <img src="examples/instance_segmentation/data_dataset_voc/SegmentationObject/2011_000006.png" width="19%" /> <img src="examples/instance_segmentation/data_dataset_voc/SegmentationObjectVisualization/2011_000006.jpg" width="19%" />  
<i>VOC dataset example of instance segmentation with SAM.</i>

## Features

### Core Features (from labelme)
- [x] Image annotation for polygon, rectangle, circle, line and point
- [x] Image flag annotation for classification and cleaning
- [x] Video annotation
- [x] GUI customization (predefined labels/flags, auto-saving, label validation)
- [x] Export VOC-format dataset for semantic/instance segmentation
- [x] Export COCO-format dataset for instance segmentation

### SAM Integration Features
- [x] **Automatic segmentation from bounding boxes** using official SAM
- [x] **Three SAM model variants**: ViT-H (accuracy), ViT-L (balanced), ViT-B (speed)
- [x] **Automatic model downloading** with progress tracking
- [x] **GPU/CPU support** with automatic device selection
- [x] **Polygon simplification** control for output quality
- [x] **Dual output modes**: bounding box, segmentation, or both
- [x] **Offline operation** with cached checkpoints

## Installation

### Requirements
- Python 3.8+
- PyTorch (for SAM)
- CUDA (optional, for GPU acceleration)

### Step 1: Install Labelme-SAM
```bash
# Install from GitHub
pip install git+https://github.com/YOUR_USERNAME/local-labelme-sam.git
```

### Step 2: Install Official SAM
```bash
# Install segment-anything from Facebook Research
pip install git+https://github.com/facebookresearch/segment-anything.git
```

### Step 3: Install PyTorch (if not already installed)

**For CUDA (GPU) support:**
```bash
# CUDA 11.8
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118

# CUDA 12.1
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
```

**For CPU only:**
```bash
pip install torch torchvision
```

### Step 4: Install YOLO-World (for text-based detection)
```bash
pip install git+https://github.com/Abbsalehi/osam.git
```

### Complete Installation (All-in-One)
```bash
# Install everything at once
pip install git+https://github.com/Abbsalehi/labelme-sam.git
pip install git+https://github.com/facebookresearch/segment-anything.git
pip install git+https://github.com/Abbsalehi/osam.git
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
```

### Verify Installation
```bash
python -c "import labelme; import segment_anything; print('Installation successful!')"
```

## Usage

### Basic Usage
```bash
labelme  # Open GUI

# Annotate a single image
labelme image.jpg

# Annotate all images in a directory
labelme /path/to/images/
```

### SAM Workflow

1. **Launch Labelme-SAM**
```bash
   labelme your_image.jpg
```

2. **Select SAM Model** (in toolbar)
   - Choose from: ViT-H (high accuracy), ViT-L (balanced), or ViT-B (fast)
   - Models download automatically on first use (~350MB for ViT-H)

3. **Enter Text Prompt** (in toolbar)
   - Type object names separated by commas (e.g., "window, door, balcony")
   - Adjust IoU and score thresholds as needed

4. **Choose Output Mode**
   - **Bounding Box**: Get detection boxes only
   - **Segmentation**: Get polygon masks only
   - **Both**: Get both boxes and masks with different labels

5. **Adjust Polygon Simplification**
   - Use slider to control polygon detail (0.001-0.1)
   - Lower values = more detailed polygons

6. **Generate Annotations**
   - Click "Submit" or press Enter
   - SAM will automatically segment detected objects

### Model Downloads

SAM checkpoints are automatically downloaded to `~/.cache/labelme/sam_checkpoints/`:
- `sam_vit_h_4b8939.pth` (2.4 GB) - Highest accuracy
- `sam_vit_l_0b3195.pth` (1.2 GB) - Balanced
- `sam_vit_b_01ec64.pth` (375 MB) - Fastest

### Command Line Arguments
```bash
labelme image.jpg --output annotations.json  # Save to specific file
labelme images/ --labels labels.txt          # Use predefined labels
labelme image.jpg --nodata                   # Don't include image data in JSON
```

### Configuration

Edit `~/.labelmerc` to customize:
- Default SAM model variant
- Default output mode
- Polygon simplification factor
- Auto-save settings
- Label colors

## Examples

### Facade Segmentation
```bash
cd examples/facade_annotation
labelme building.jpg
# Enter prompt: "window, door, balcony"
# Select SAM ViT-H for best accuracy
```

### Quick Annotation
```bash
cd examples/quick_annotation
labelme dataset/
# Use SAM ViT-B for fast batch processing
```

See more examples in the [examples](examples/) directory.

## SAM Integration

### Supported SAM Models

| Model | Size | Speed | Accuracy | Use Case |
|-------|------|-------|----------|----------|
| ViT-H | 2.4 GB | Slow | Highest | Research, final annotations |
| ViT-L | 1.2 GB | Medium | High | General purpose |
| ViT-B | 375 MB | Fast | Good | Quick annotation, large datasets |

### GPU vs CPU

- **CUDA (GPU)**: ~0.5-2 seconds per image (recommended)
- **CPU**: ~5-15 seconds per image

### Output Modes

1. **Bounding Box Only**: Fast detection without segmentation
2. **Segmentation Only**: Precise polygon masks from SAM
3. **Both**: Get `object_bbox` and `object_seg` with different colors

## FAQ

**Q: Do I need an internet connection?**  
A: Only for initial model download. After that, everything runs offline.

**Q: Which SAM model should I use?**  
A: ViT-H for best quality, ViT-B for speed, ViT-L for balance.

**Q: Can I use my own SAM checkpoint?**  
A: Yes, place it in `~/.cache/labelme/sam_checkpoints/` and modify the config.

**Q: Does this work without GPU?**  
A: Yes, but it's slower. GPU is highly recommended for interactive use.

**Q: How is this different from original labelme?**  
A: Uses official SAM with local checkpoints instead of osam, giving you direct access to Meta's models.

## Building Standalone Executable
```bash
pyinstaller labelme/__main__.py \
  --name=Labelme-SAM \
  --windowed \
  --noconfirm \
  --add-data=labelme/config/default_config.yaml:labelme/config \
  --add-data=labelme/icons/*:labelme/icons \
  --icon=labelme/icons/icon-256.png \
  --onedir
```

## Credits

- **Original Labelme**: [wkentaro/labelme](https://github.com/wkentaro/labelme)
- **Segment Anything**: [facebookresearch/segment-anything](https://github.com/facebookresearch/segment-anything)
- **YOLO-World**: For text-based object detection

## License

This project follows the same license as the original labelme project.

## Citation

If you use this tool in your research, please cite both labelme and SAM:
```bibtex
@misc{labelme2016,
  author = {Kentaro Wada},
  title = {{labelme: Image Polygonal Annotation with Python}},
  howpublished = {\url{https://github.com/wkentaro/labelme}},
  year = {2016}
}

@article{kirillov2023segany,
  title={Segment Anything},
  author={Kirillov, Alexander and Mintun, Eric and Ravi, Nikhila and Mao, Hanzi and Rolland, Chloe and Gustafson, Laura and Xiao, Tete and Whitehead, Spencer and Berg, Alexander C. and Lo, Wan-Yen and Doll{\'a}r, Piotr and Girshick, Ross},
  journal={arXiv:2304.02643},
  year={2023}
}
```

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

---

**Labelme-SAM** - Fast, accurate annotation powered by official Segment Anything Model
