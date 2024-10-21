---
{"dg-publish":true,"permalink":"/object-detection/model/rt-detr/"}
---

[RT-DETR (Realtime Detection Transformer) - Ultralytics YOLO Docs](https://docs.ultralytics.com/models/rtdetr/)
## Overview

Real-Time Detection Transformer (RT-DETR)는 cutting-edge end-to-end object detector이다. real-time performance while maintaining high [accuracy](https://www.ultralytics.com/glossary/accuracy). It is based on the idea of DETR (the NMS-free framework), meanwhile introducing conv-based backbone and an efficient hybrid encoder to gain real-time speed. RT-DETR efficiently processes multiscale features by decoupling intra-scale interaction and cross-scale fusion. The model is highly adaptable, supporting flexible adjustment of inference speed using different decoder layers without retraining. RT-DETR excels on accelerated backends like CUDA with TensorRT, outperforming many other real-time object detectors.
![](https://i.imgur.com/N01T35G.png)
