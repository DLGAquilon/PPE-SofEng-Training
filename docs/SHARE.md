# Push this repo and send the weights

The images and the `.pt` files stay out of git. `.gitignore` already
excludes `data/`, `artifacts/`, `reports/`, the virtual environment, and
`*.pt`. Nothing in this folder has been committed yet, so the first commit
can stay small.

## Check the file list

From the repo root:

```powershell
git status --short
```

`data/`, `artifacts/`, `reports/`, and `.venv313/` must not appear as files
to be committed. If a weight or image is listed, do not continue. The ignore
file is not hiding it.

## First commit

```powershell
git add .gitignore README.md requirements.txt configs docs notebooks scripts
git status --short
git commit -m "Add the PPE training notebook, sample window, and share notes."
```

Read the status output before the commit. It should be the notebook, the
demo script, the docs, the example dataset yaml, and `requirements.txt`.

## Push to the other repository

Use the repository URL your friend controls.

```powershell
git remote add origin https://github.com/FRIEND/REPO.git
git push -u origin HEAD
```

If `origin` already exists, replace the URL instead:

```powershell
git remote set-url origin https://github.com/FRIEND/REPO.git
git push -u origin HEAD
```

Your friend needs write access for the push, or you need permission to push
to that repository.

## Send the weights

Send two files, not the whole `artifacts` folder:

| File | Size | Where your friend saves it |
| --- | --- | --- |
| `artifacts/rt-detr/rt-detr-i/weights/best.pt` | 63 MB | same path in his clone |
| `artifacts/yolov8/ppe_yolov8s/weights/best.pt` | 22 MB | same path in his clone |

`last.pt` is the final epoch. The sample window and the evaluation cells
use `best.pt`.

Your friend controls the GitHub repository, so attach the two `best.pt`
files to a Release after the code push. They are under GitHub's 100 MB
file limit. A Release keeps them out of the git history and can be replaced
when a later training run is better.

```powershell
gh release create weights-v1 `
  "artifacts/rt-detr/rt-detr-i/weights/best.pt" `
  "artifacts/yolov8/ppe_yolov8s/weights/best.pt" `
  --repo FRIEND/REPO `
  --title "PPE weights" `
  --notes "RT-DETR-L best.pt and YOLOv8s best.pt. See README for the folders."
```

He downloads those two assets from the Release page and saves them at the
paths in the table. The sample window then finds the RT-DETR file, and the
notebook finds the YOLOv8 file.

The dataset is separate. Zip `data/processed/PPE-Normalized-v2` and send
that archive the same way, or hand it over on Drive. It is thousands of
images, so it does not belong in the git commit or in this small Release.
After he unpacks it, `data.yaml` inside that folder must use his own
machine path. `configs/ppe_normalized_v2.example.yaml` is the class list.
