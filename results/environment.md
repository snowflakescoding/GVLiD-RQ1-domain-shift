# Environment

* Hardware: Tesla T4
* Python 3.13.16 on Linux-6.6.159+-x86_64-with-glibc2.39
* torch 2.14.1+cu130, torchvision 0.29.1+cu130, numpy 2.4.6, pandas 2.2.3, scikit-learn 1.6.1, scipy 1.16.3
* Seeds: [42, 43, 44] (training seed and split seed; the OOD test domain is fixed)
* Image size 224, batch 16, flips-only augmentation, AdamW with cosine schedule
* Pretrained checkpoints: torchvision IMAGENET1K_V1 weights for ResNet-18, MobileNetV3-Small and ConvNeXt-Tiny
* Wall-clock training time per run: results/tables/rq1_runs.csv (column train_time_s); total 6.4 min
