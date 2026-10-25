# Training

Run `notebooks/ppe_yolov8_vs_rfdetr.ipynb` from the top. The dataset folder
must already be at `data/processed/PPE-Normalized-v2` with `images/train`,
`images/val`, `labels/train`, and `labels/val`.

Copy `configs/ppe_normalized_v2.example.yaml` to
`data/processed/PPE-Normalized-v2/data.yaml`. Set `path` to that dataset
folder. A path of `.` is only correct when the working directory is the
dataset folder itself. The notebook rewrites `path` to the folder it found
before YOLOv8 starts.

## Sections

1. Check the GPU.
2. Check the seven classes, paired images and labels, and normalized boxes.
3. Build the 90/10 split if `split_90_10.json` is not already there. The
   cleaned set used for the finished runs is about 7,936 train and 882 val.
   Running the cell again does not reshuffle a split that was already written.
4. Train YOLOv8s for 40 epochs at 640, batch 16, patience 10, mosaic 1.0,
   mixup 0.1, cls 1.5, box 7.5.
5. Evaluate the best YOLOv8 checkpoint on the validation split.
6. Print precision, recall, mAP, and the confusion matrix.
7. Copy `best.pt` to `artifacts/weights/yolov8/best.pt`.
8. Train RF-DETR Small on the same split for 40 epochs at 640, batch 16,
   patience 10, then print the same validation metrics.

A GPU with less than 6 GB, including a GeForce MX130, uses YOLOv8 batch 4
with AMP off and SGD, and RF-DETR batch 1. A 16 GB T4 or P100 keeps batch
16 and AMP.

## Finished checkpoints

| Model | Weights |
| --- | --- |
| YOLOv8s | `artifacts/yolov8/ppe_yolov8s/weights/best.pt` |
| RT-DETR-L | `artifacts/rt-detr/rt-detr-i/weights/best.pt` |

The RT-DETR-L run was trained as Ultralytics `rtdetr-l.pt` for 40 epochs at
640, batch 8. The sample window loads that `best.pt`. It is a different
checkpoint from the RF-DETR Small run in section 8.
