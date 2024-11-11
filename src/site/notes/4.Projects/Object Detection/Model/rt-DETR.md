---
{"dg-publish":true,"permalink":"/4-projects/object-detection/model/rt-detr/"}
---

[RT-DETR (Realtime Detection Transformer) - Ultralytics YOLO Docs](https://docs.ultralytics.com/models/rtdetr/)
## Overview

Real-Time Detection Transformer (RT-DETR)는 cutting-edge end-to-end object detector이다. 
real-time performance & high [accuracy](https://www.ultralytics.com/glossary/accuracy). 

DETR (the NMS-free framework)에 기반해서 conv-based backbone 을 도입했다. 
real-time speed를 얻기 위해 efficient hybrid encoder를 도입했다. 
RT-DETR은 intra-scale intertion을 분리하고 cross-scale을 융합해서 효율적으로 multiscale features를 처리한다. 
retraining없이 다른 decoder layer를 사용해서 inference speed를 유연하게 조절할 수 있다.
TensorRT있는 CUDA같은 고성능 백엔드에 최적화되어있다.
![](https://i.imgur.com/N01T35G.png)
**Overview of Baidu's RT-DETR.** The RT-DETR model architecture diagram shows the last three stages of the backbone {S3, S4, S5} as the input to the encoder. 
backbone의 마지막 세 단계 {S3, S4, S5}를 인코더의 입력으로 사용한다.
efficient hybrid encoder는 multiscale features를 intrascale feature interaction (AIFI)와 cross-scale feature-fusion module (CCFM)을 통해 sequence of image features로 변환한다.
IoU-aware query selection을 사용하여 고정된 수의 이미지 특징을 선택하고, 이를 decoder의  initial object queries로 사용한다.
디코더는 보조 prediction heads를 사용하여 반복적으로 object queries를 최적화하여 
The efficient hybrid encoder transforms multiscale features into a sequence of image features through intrascale feature interaction (AIFI) and cross-scale feature-fusion module (CCFM). The IoU-aware query selection is employed to select a fixed number  of image features to serve as initial object queries for the decoder. Finally, the decoder with auxiliary prediction heads iteratively optimizes object queries to generate boxes and confidence scores ([source](https://arxiv.org/pdf/2304.08069.pdf)).
RT-DETR 모델 아키텍처 다이어그램은 백본의 마지막 세 단계 {S3, S4, S5}를 인코더의 입력으로 사용합니다. 효율적인 하이브리드 인코더는 멀티스케일 특징을 인트라스케일 특징 상호작용(AIFI)과 크로스스케일 특징 융합 모듈(CCFM)을 통해 이미지 특징 시퀀스로 변환합니다. 그런 다음 IoU-인식 쿼리 선택을 사용하여 고정된 수의 이미지 특징을 선택하고, 이를 디코더의 초기 객체 쿼리로 사용합니다. 마지막으로, 디코더는 보조 예측 헤드를 사용하여 반복적으로 객체 쿼리를 최적화하여 박스와 신뢰도 점수를 생성합니다.
### Key Features

- **Efficient Hybrid Encoder:** Baidu's RT-DETR uses an efficient hybrid encoder that processes multiscale features by decoupling intra-scale interaction and cross-scale fusion. This unique Vision Transformers-based design reduces computational costs and allows for real-time [object detection](https://www.ultralytics.com/glossary/object-detection).
- **IoU-aware Query Selection:** Baidu's RT-DETR improves object query initialization by utilizing IoU-aware query selection. This allows the model to focus on the most relevant objects in the scene, enhancing the detection accuracy.
- **Adaptable Inference Speed:** Baidu's RT-DETR supports flexible adjustments of inference speed by using different decoder layers without the need for retraining. This adaptability facilitates practical application in various real-time object detection scenarios.

## Pre-trained Models

The Ultralytics Python API provides pre-trained PaddlePaddle RT-DETR models with different scales:

- RT-DETR-L: 53.0% AP on COCO val2017, 114 FPS on T4 GPU
- RT-DETR-X: 54.8% AP on COCO val2017, 74 FPS on T4 GPU

## Usage Examples

This example provides simple RT-DETR training and inference examples. For full documentation on these and other [modes](https://docs.ultralytics.com/modes/) see the [Predict](https://docs.ultralytics.com/modes/predict/), [Train](https://docs.ultralytics.com/modes/train/), [Val](https://docs.ultralytics.com/modes/val/) and [Export](https://docs.ultralytics.com/modes/export/) docs pages.

Example

[Python](https://docs.ultralytics.com/models/rtdetr/#__tabbed_1_1)[CLI](https://docs.ultralytics.com/models/rtdetr/#__tabbed_1_2)

`[](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-1)from ultralytics import RTDETR [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-2) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-3)# Load a COCO-pretrained RT-DETR-l model [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-4)model = RTDETR("rtdetr-l.pt") [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-5) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-6)# Display model information (optional) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-7)model.info() [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-8) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-9)# Train the model on the COCO8 example dataset for 100 epochs [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-10)results = model.train(data="coco8.yaml", epochs=100, imgsz=640) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-11) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-12)# Run inference with the RT-DETR-l model on the 'bus.jpg' image [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-0-13)results = model("path/to/bus.jpg")`

## Supported Tasks and Modes

This table presents the model types, the specific pre-trained weights, the tasks supported by each model, and the various modes ([Train](https://docs.ultralytics.com/modes/train/) , [Val](https://docs.ultralytics.com/modes/val/), [Predict](https://docs.ultralytics.com/modes/predict/), [Export](https://docs.ultralytics.com/modes/export/)) that are supported, indicated by ✅ emojis.

|Model Type|Pre-trained Weights|Tasks Supported|Inference|Validation|Training|Export|
|---|---|---|---|---|---|---|
|RT-DETR Large|[rtdetr-l.pt](https://github.com/ultralytics/assets/releases/download/v8.2.0/rtdetr-l.pt)|[Object Detection](https://docs.ultralytics.com/tasks/detect/)|✅|✅|✅|✅|
|RT-DETR Extra-Large|[rtdetr-x.pt](https://github.com/ultralytics/assets/releases/download/v8.2.0/rtdetr-x.pt)|[Object Detection](https://docs.ultralytics.com/tasks/detect/)|✅|✅|✅|✅|

## Citations and Acknowledgements

If you use Baidu's RT-DETR in your research or development work, please cite the [original paper](https://arxiv.org/abs/2304.08069):

[BibTeX](https://docs.ultralytics.com/models/rtdetr/#__tabbed_2_1)

`[](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-1)@misc{lv2023detrs, [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-2)      title={DETRs Beat YOLOs on Real-time Object Detection}, [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-3)      author={Wenyu Lv and Shangliang Xu and Yian Zhao and Guanzhong Wang and Jinman Wei and Cheng Cui and Yuning Du and Qingqing Dang and Yi Liu}, [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-4)      year={2023}, [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-5)      eprint={2304.08069}, [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-6)      archivePrefix={arXiv}, [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-7)      primaryClass={cs.CV} [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-2-8)}`

We would like to acknowledge Baidu and the [PaddlePaddle](https://github.com/PaddlePaddle/PaddleDetection) team for creating and maintaining this valuable resource for the [computer vision](https://www.ultralytics.com/glossary/computer-vision-cv) community. Their contribution to the field with the development of the Vision Transformers-based real-time object detector, RT-DETR, is greatly appreciated.

## FAQ

### What is Baidu's RT-DETR model and how does it work?

Baidu's RT-DETR (Real-Time Detection Transformer) is an advanced real-time object detector built upon the Vision Transformer architecture. It efficiently processes multiscale features by decoupling intra-scale interaction and cross-scale fusion through its efficient hybrid encoder. By employing IoU-aware query selection, the model focuses on the most relevant objects, enhancing detection accuracy. Its adaptable inference speed, achieved by adjusting decoder layers without retraining, makes RT-DETR suitable for various real-time object detection scenarios. Learn more about RT-DETR features [here](https://arxiv.org/pdf/2304.08069.pdf).

### How can I use the pre-trained RT-DETR models provided by Ultralytics?

You can leverage Ultralytics Python API to use pre-trained PaddlePaddle RT-DETR models. For instance, to load an RT-DETR-l model pre-trained on COCO val2017 and achieve high FPS on T4 GPU, you can utilize the following example:

Example

[Python](https://docs.ultralytics.com/models/rtdetr/#__tabbed_3_1)[CLI](https://docs.ultralytics.com/models/rtdetr/#__tabbed_3_2)

`[](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-1)from ultralytics import RTDETR [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-2) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-3)# Load a COCO-pretrained RT-DETR-l model [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-4)model = RTDETR("rtdetr-l.pt") [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-5) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-6)# Display model information (optional) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-7)model.info() [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-8) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-9)# Train the model on the COCO8 example dataset for 100 epochs [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-10)results = model.train(data="coco8.yaml", epochs=100, imgsz=640) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-11) [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-12)# Run inference with the RT-DETR-l model on the 'bus.jpg' image [](https://docs.ultralytics.com/models/rtdetr/#__codelineno-3-13)results = model("path/to/bus.jpg")`

### Why should I choose Baidu's RT-DETR over other real-time object detectors?

Baidu's RT-DETR stands out due to its efficient hybrid encoder and IoU-aware query selection, which drastically reduce computational costs while maintaining high accuracy. Its unique ability to adjust inference speed by using different decoder layers without retraining adds significant flexibility. This makes it particularly advantageous for applications requiring real-time performance on accelerated backends like CUDA with TensorRT, outclassing many other real-time object detectors.

### How does RT-DETR support adaptable inference speed for different real-time applications?

Baidu's RT-DETR allows flexible adjustments of inference speed by using different decoder layers without requiring retraining. This adaptability is crucial for scaling performance across various real-time object detection tasks. Whether you need faster processing for lower [precision](https://www.ultralytics.com/glossary/precision) needs or slower, more accurate detections, RT-DETR can be tailored to meet your specific requirements.

### Can I use RT-DETR models with other Ultralytics modes, such as training, validation, and export?

Yes, RT-DETR models are compatible with various Ultralytics modes including training, validation, prediction, and export. You can refer to the respective documentation for detailed instructions on how to utilize these modes: [Train](https://docs.ultralytics.com/modes/train/), [Val](https://docs.ultralytics.com/modes/val/), [Predict](https://docs.ultralytics.com/modes/predict/), and [Export](https://docs.ultralytics.com/modes/export/). This ensures a comprehensive workflow for developing and deploying your object detection solutions.