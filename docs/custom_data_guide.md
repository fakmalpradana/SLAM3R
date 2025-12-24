# How to Use Custom Data with SLAM3R

This guide explains how to prepare your own image sequences for 3D reconstruction using SLAM3R.

## 1. Data Preparation

SLAM3R works with monocular RGB video frames. You need to extract frames from your video or organize your images into a single directory.

### Directory Structure
Organize your data as follows:
```
path/to/your/custom_data/
├── frame00001.jpg
├── frame00002.jpg
├── frame00003.jpg
...
```

### Requirements
- **Format**: JPG or PNG.
- **Naming**: Files should be named sequentially (e.g., `frame0000.jpg`, `frame0001.jpg`). The loader sorts files by name.
- **Resolution**: Images will be resized to **224x224** automatically. High-resolution images are fine, but aspect ratio should be consistent.
- **Content**: A continuous video sequence with sufficient overlap and parallax is best. Avoid extremely fast motion or pure rotation without translation.

## 2. Running Reconstruction

Use the provided scripts or run `recon.py` directly.

### Command Template
Run the following command in your terminal (ensure your `slam3r` environment is active):

```bash
python recon.py ^
  --test_name "MyCustomScene" ^
  --dataset "path/to/your/custom_data" ^
  --gpu_id -1 ^
  --keyframe_stride 10 ^
  --win_r 5 ^
  --num_scene_frame 10 ^
  --initial_winsize 5 ^
  --conf_thres_l2w 10 ^
  --conf_thres_i2p 1.5 ^
  --num_points_save 1000000 ^
  --update_buffer_intv 3 ^
  --max_num_register 10
```

### Key Parameters to Tune
- `--keyframe_stride`: How many frames to skip for keyframes. Increase for slow videos, decrease for fast videos. (Default: 3 or 20 for demo)
- `--conf_thres_i2p`: Confidence threshold for Image-to-Points. Higher values = cleaner but fewer points.
- `--num_points_save`: Target number of points in final cloud.

## 3. Results
Output will be saved in `results/MyCustomScene/`.
- `final_cloud.ply`: The final accumulated point cloud.
- `preds/`: Per-frame point clouds (if `--save_preds` is used).

## 4. Visualization
You can use MeshLab or CloudCompare to view the `.ply` files.
For incremental visualization, check `scripts/demo_vis_wild.sh`.
