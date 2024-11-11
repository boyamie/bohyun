---
{"dg-publish":true,"permalink":"/4-projects/object-detection/experiments/inference/"}
---

```python
python inference.py --config /data/ephemeral/home/mmdetection/configs/cascade_rcnn/cascade_rcnn_x101_64x4d_fpn_20e_coco.py --epoch latest
```
MMDetection 프레임워크를 사용하여 모델의 체크포인트에서 inference 실행 
그 결과를 Pascal VOC 형식의 `submission.csv` 파일로 저장하는 구조 

1. **output 처리 수정**:
   추론 결과(`output`)가 Pascal VOC 포맷에 맞게 저장되도록 처리 로직을 명확히 함
   예측된 bbox 좌표 `(xmin, ymin, xmax, ymax)`와 그에 상응하는 클래스 및 신뢰도 점수를 적절히 포맷팅

3. **파일 경로 설정 및 CSV 파일 생성**:
   `submission.csv` 파일의 경로를 올바르게 설정하고, 그 파일에 정확한 형식으로 데이터를 기록

```python
import mmcv
from mmcv import Config
from mmdet.datasets import build_dataloader, build_dataset
from mmdet.models import build_detector
from mmdet.apis import single_gpu_test
from mmcv.runner import load_checkpoint
import os
from mmcv.parallel import MMDataParallel
import pandas as pd
from pycocotools.coco import COCO
import argparse

# 클래스 정의 (Pascal VOC 포맷에 맞춘 클래스명)
classes = ("General trash", "Paper", "Paper pack", "Metal", "Glass", 
           "Plastic", "Styrofoam", "Plastic bag", "Battery", "Clothing")

def inference(cfg, epoch):
    # 데이터셋 및 데이터로더 구축
    dataset = build_dataset(cfg.data.test)
    data_loader = build_dataloader(
        dataset,
        samples_per_gpu=1,
        workers_per_gpu=cfg.data.workers_per_gpu,
        dist=False,
        shuffle=False
    )

    # 체크포인트 경로
    checkpoint_path = os.path.join(cfg.work_dir, f'{epoch}.pth')

    # 모델 구축 및 체크포인트 로드
    model = build_detector(cfg.model, test_cfg=cfg.get('test_cfg'))
    checkpoint = load_checkpoint(model, checkpoint_path, map_location='cpu')

    model.CLASSES = dataset.CLASSES
    model = MMDataParallel(model.cuda(), device_ids=[0])

    # 추론 실행 (단일 GPU)
    output = single_gpu_test(model, data_loader, show_score_thr=0.05)

    # COCO 형식의 주석 파일 로드
    coco = COCO(cfg.data.test.ann_file)

    # 추론 결과를 Pascal VOC 포맷에 맞춰서 후처리
    prediction_strings = []
    file_names = []
    for i, out in enumerate(output):
        prediction_string = ''
        image_info = coco.loadImgs(coco.getImgIds(imgIds=i))[0]
        for j, class_output in enumerate(out):
            for bbox in class_output:
                score = bbox[4]
                xmin, ymin, xmax, ymax = bbox[0], bbox[1], bbox[2], bbox[3]
                prediction_string += f'{j} {score} {xmin} {ymin} {xmax} {ymax} '
        
        prediction_strings.append(prediction_string.strip())
        file_names.append(image_info['file_name'])

    # 결과를 CSV 파일로 저장
    submission = pd.DataFrame({
        'image_id': file_names,
        'PredictionString': prediction_strings
    })
    submission.to_csv(os.path.join(cfg.work_dir, f'submission_{epoch}.csv'), index=False)
    print(f'Submission file saved at {os.path.join(cfg.work_dir, f"submission_{epoch}.csv")}')

def main():
    parser = argparse.ArgumentParser(description='PyTorch Object Detection Inference')
    parser.add_argument('--config', type=str, default='./configs/faster_rcnn/faster_rcnn_r50_fpn_1x_coco.py', help='config file')
    parser.add_argument('--epoch', type=str, default='latest', help='epoch number')
    args = parser.parse_args()

    # config 파일 로드
    cfg = Config.fromfile(args.config)

    root = '../../dataset/'
    cfg.data.test.classes = classes
    cfg.data.test.img_prefix = root
    cfg.data.test.ann_file = root + 'test.json'
    cfg.data.test.pipeline[1]['img_scale'] = (512, 512)  # 이미지 크기 조정
    cfg.data.test.test_mode = True

    cfg.seed = 2021
    cfg.gpu_ids = [0]
    cfg.work_dir = './work_dirs/faster_rcnn_r50_fpn_1x_trash'

    # num_classes 수정
    cfg.model.roi_head.bbox_head.num_classes = len(classes)

    # Optimizer 설정 (필요 시)
    cfg.optimizer_config.grad_clip = dict(max_norm=35, norm_type=2)
    cfg.model.train_cfg = None

    # Inference 실행
    inference(cfg, args.epoch)

if __name__ == "__main__":
    main()
```

1. `prediction_string`추론 결과에서 클래스, 신뢰도 점수, 바운딩 박스 좌표를 Pascal VOC 형식에 맞춰서 문자열로 생성
2. **`image_info['file_name']` 처리**: 각 이미지의 파일 이름을 추출하여 `submission.csv`에 저장
3. **`cfg.model.roi_head.bbox_head.num_classes` 설정**: 클래스 수를 10개로 설정하여 모델이 맞게 작동하도록 수정

### 명령어

atss 모델을 실행한 예시입니다. 돌리신 모델로 적절히 수정해주세요. 경로는 대부분 절대경로로 해두었습니다.
1. 터미널에서 명령어
   ```bash
python inference.py --config /data/ephemeral/home/mmdetection/configs/atss/atss_r101_fpn_1x_coco.py --epoch latest
   ```
2. cfg.work_dir = '/data/ephemeral/home/output/mmdetection/2024-10-11_18-44-24_d215e' 
4. `submission.csv` 파일이 `cfg.work_dir` 경로에 생성됩니다. (Pascal VOC 형식의 추론 결과를 `submission.csv`로 저장)

   ```bash
python inference.py --config /data/ephemeral/home/mmdetection/configs/cascade_rcnn/cascade_rcnn_x101_64x4d_fpn_20e_coco.py --epoch latest
   ```