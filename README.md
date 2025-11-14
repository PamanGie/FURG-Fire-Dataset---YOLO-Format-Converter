# FURG Fire Dataset - YOLO Format Converter

Tool to extract frames from videos and convert XML annotations (OpenCV format) to YOLO format for the [FURG Fire Dataset](https://github.com/steffensbola/furg-fire-dataset).

## 📋 Description

This script extracts all frames from MP4 videos and converts bounding box annotations from OpenCV XML format to YOLO format, ready for training object detection models.

## 🙏 Credits

The original dataset and annotations are from:
- **Repository:** [steffensbola/furg-fire-dataset](https://github.com/steffensbola/furg-fire-dataset)
- **License:** CC0-1.0 license

Please visit the original repository for more information about the dataset.

## ✨ Features

- ✅ Extract frames from all MP4 videos
- ✅ Automatic conversion of XML annotations (OpenCV) → YOLO format
- ✅ Progress bar for process tracking
- ✅ Organized folder structure per video
- ✅ Support for multiple video files
- ✅ Normalized bounding box coordinates (0-1)

## 📦 Requirements

Install dependencies with:

```bash
pip install opencv-python tqdm
```

## 🚀 Usage

1. **Ensure the folder structure is as follows:**
   ```
   .
   ├── furg-fire-dataset-master/
   │   ├── barbecue.mp4
   │   ├── barbecue.xml
   │   ├── car1.mp4
   │   ├── car1.xml
   │   └── ...
   └── main.py
   ```

2. **Run the script:**
   ```bash
   python main.py
   ```

3. **Output will be saved to:**
   ```
   dataset/
   └── images/
       ├── barbecue/
       │   ├── images/
       │   │   ├── barbecue000.jpg
       │   │   ├── barbecue001.jpg
       │   │   └── ...
       │   └── labels/
       │       ├── barbecue000.txt
       │       ├── barbecue001.txt
       │       └── ...
       ├── car1/
       │   ├── images/
       │   └── labels/
       └── ...
   ```

## 📁 Output Structure

Each video will have its own folder with the following structure:
- `images/` → Folder containing extracted frames (JPG format)
- `labels/` → Folder containing YOLO annotation files (TXT format)

File naming: `[videoname][framenumber].jpg` and `[videoname][framenumber].txt`

Example:
- `barbecue000.jpg` → Frame 0 from barbecue video
- `barbecue000.txt` → YOLO annotation for frame 0

## 📝 YOLO Annotation Format

Annotation format follows YOLO standard:
```
class_id center_x center_y width height
```

**Description:**
- `class_id`: Class ID (0 = fire)
- `center_x`: Center X coordinate (normalized 0-1)
- `center_y`: Center Y coordinate (normalized 0-1)
- `width`: Bounding box width (normalized 0-1)
- `height`: Bounding box height (normalized 0-1)

**Example:**
```
0 0.406250 0.485556 0.169531 0.508333
```

This means:
- Class 0 (fire)
- Center at (0.406250, 0.485556)
- Width: 0.169531, Height: 0.508333

## 🔄 Format Conversion

This script converts from OpenCV format to YOLO format:

**OpenCV Format (from XML):**
- `x y width height` (absolute pixels, x,y = top-left corner)

**YOLO Format (output):**
- `class_id center_x center_y width height` (normalized 0-1, center_x/y = center point)

Conversion formula:
- `center_x = (x + width/2) / img_width`
- `center_y = (y + height/2) / img_height`
- `width = width / img_width`
- `height = height / img_height`

## 📊 Progress Tracking

The script uses `tqdm` to display progress:
- Progress bar for each video being processed
- Progress bar for frame extraction in each video
- Displays the number of frames processed

## ⚙️ Configuration

You can modify some parameters in the `main()` function:

```python
dataset_dir = Path('furg-fire-dataset-master')  # Dataset folder
output_dir = Path('dataset/images')              # Output folder
```

## 📌 Notes

- All frames from videos will be extracted (including frames without annotations)
- Frames without annotations will have an empty `.txt` file
- Video folder names will be converted to lowercase (e.g., `Car1.mp4` → `car1/`)
- Class ID for fire is `0`

## 📄 License

This tool is created to facilitate the conversion of the FURG Fire dataset to YOLO format. 
The original dataset uses CC0-1.0 license from [steffensbola/furg-fire-dataset](https://github.com/steffensbola/furg-fire-dataset).

## 🤝 Contributing

Feel free to create issues or pull requests if you find bugs or want to add features.

---

**Happy Training! 🔥**
