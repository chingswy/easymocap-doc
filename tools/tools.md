---
layout: default
title: 实用工具
nav_order: 6
has_children: true
---

# Tools
{: .no_toc }

1. TOC
{:toc}
---

## 手动白平衡

拍摄色板的照片

### 色板位置标注

创建标定板

```bash
python3 apps/calibration/create_marker.py ${data} --N 4
```

手动标注
```bash
python3 apps/annotation/annot_calib.py ${data} --annot chessboard --mode marker
```

### Homography变换

