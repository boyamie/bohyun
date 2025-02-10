---
{"dg-publish":true,"permalink":"/4-projects//da-and-kd/"}
---

[A Survey on Knowledge Distillation of Large Language Models](https://arxiv.org/html/2402.13116v1)
LLM 시대에 데이터 증강(DA)  Wang et al. ([2022년](https://arxiv.org/html/2402.13116v1#bib.bib40)); Ye et al. ([2022](https://arxiv.org/html/2402.13116v1#bib.bib41))는 지식 증류 과정에 필수적인 중요한 패러다임으로 등장합니다.  Gangal et al.의 의역과 같은 전통적인 DA 기술과는 달리([2022](https://arxiv.org/html/2402.13116v1#bib.bib42)) 또는 역번역  Longpre et al. ([2019](https://arxiv.org/html/2402.13116v1#bib.bib43)) , 주로 다소 기계적인 방식으로 훈련 데이터 세트를 확장하는 것을 목표로 합니다. LLM 맥락 내의 DA는 특정 도메인과 기술에 맞게 조정된 새롭고 맥락이 풍부한 훈련 데이터를 생성하는 데 중점을 둡니다. 이 혁신은 다양한 분야의 인간 전문가의 미묘한 이해와 인지 능력을 면밀히 모방하는 일관되고 다양하며 복잡한 데이터 샘플을 생성하는 LLM의 고유한 역량에 의해 주도됩니다.

LLM에서 DA와 KD의 관계는 공생적이고 기초적입니다. 일련의 시드 지식을 활용하여 KD는 DA를 사용하여 LLM이 특정 기술이나 도메인 전문 지식을 캡슐화하는 명시적 데이터를 생성하도록 촉구합니다.  Chaudhary([2023](https://arxiv.org/html/2402.13116v1#bib.bib21)); West et al. ([2022](https://arxiv.org/html/2402.13116v1#bib.bib44)) . 이 방법은 독점 모델과 오픈 소스 모델 간의 지식과 역량 격차를 메우는 강력한 메커니즘으로 돋보입니다. DA를 통해 LLM은 단순히 볼륨이 더 클 뿐만 아니라 다양성과 특이성이 풍부한 타겟팅된 고품질 데이터 세트를 만들도록 유도됩니다. 이 접근 방식은 증류 프로세스를 보다 효과적으로 수행하여 증류된 모델이 교사 모델의 출력 동작을 복제할 뿐만 아니라 깊이 자리 잡은 이해와 인지 전략을 구현하도록 보장합니다.

LLM 시대에 KD를 달성하기 위한 DA의 중요성과 필요성은 과장할 수 없습니다. DA는 힘의 증폭기 역할을 하여, 증류된 모델이 그렇지 않으면 기하급수적으로 더 큰 데이터 세트와 계산 리소스가 필요할 역량을 획득하고 개선할 수 있도록 합니다. DA는 양적 확장보다는 학습의 질적 측면에 초점을 맞추어 지식의 보다 미묘하고 효과적인 전달을 용이하게 합니다. KD 프로세스 내에서 DA를 전략적으로 사용하는 것은 LLM의 힘을 활용하기 위한 보다 효율적이고 지속 가능하며 접근 가능한 접근 방식으로의 중요한 전환을 강조합니다. 오픈소스 모델이 독점적인 대응 모델의 맥락적 적응성, 윤리적 일치, 심층적인 의미적 통찰력을 근사할 수 있는 능력을 제공하여 고급 AI 역량에 대한 접근성을 민주화하고 더 광범위한 애플리케이션과 사용자에 걸쳐 혁신을 촉진합니다.
![](https://i.imgur.com/srHmohZ.png)

 json 파일을 분리한 후에도 KD를 적용 파이프라인

- **Stage1**:
    
    - Clean 데이터로 Teacher 모델을 학습.
    - 같은 clean 데이터로 Student 모델에 KD 적용하여 기초 성능 확보.
- **Stage2**:
    
    - 기타 데이터에 대해 학습할 때, Stage1의 Teacher 모델을 활용해 soft label을 생성하고, Student 모델을 KD 방식(예: KL Divergence Loss, Feature-based Loss 등)으로 추가 학습.
    - 또는 두 단계의 학습을 통합하여 혼합 학습을 진행할 수도 있음.

KD로 clean 데이터에서 얻은 고품질의 지식을 기타 데이터 학습에 전이시키는 중요한 역할을 수행

#### DA와 KD를 결합한 오디오 모델 학습
```python
"""
da_kd.py

데이터 증강(DA)와 Knowledge Distillation(KD)을 결합한 오디오 모델 학습 예제.
- audiomentations를 사용하여 오디오 데이터 증강을 수행
- 미리 학습된 Teacher 모델(여기서는 Dummy 모델)을 활용하여 KD 학습 진행
- Student 모델은 Teacher의 예측(soft label)과 실제 라벨(하드 라벨)을 함께 학습
"""

import os
import numpy as np
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import Dataset, DataLoader
from audiomentations import Compose, Normalize, TimeStretch, PitchShift, AddGaussianSNR

def get_transforms_asr():
    """
    ASR 전용 데이터 증강 파이프라인 예제
    """
    transforms = Compose(transforms=[
        Normalize(apply_to="all", p=1),
        TimeStretch(p=0.5, min_rate=0.95, max_rate=1.05, leave_length_unchanged=False),
        PitchShift(p=0.5, min_semitones=-2, max_semitones=2),
        AddGaussianSNR(p=0.5, min_snr_db=25, max_snr_db=40)
    ])
    return transforms

# ---------------------------
# Dummy Dataset
# ---------------------------

class DummyAudioDataset(Dataset):
    """
    데모용 Dummy 오디오 데이터셋 (랜덤 신호와 랜덤 라벨)
    """
    def __init__(self, num_samples=1000, sample_length=16000, num_classes=10):
        self.num_samples = num_samples
        self.sample_length = sample_length
        self.num_classes = num_classes
        
        # 랜덤 오디오 데이터 (예: 1초 길이, 16kHz sampling)
        self.data = np.random.randn(num_samples, sample_length).astype(np.float32)
        # 랜덤 라벨 (0 ~ num_classes-1)
        self.labels = np.random.randint(0, num_classes, size=(num_samples,))
        
    def __len__(self):
        return self.num_samples
    
    def __getitem__(self, idx):
        sample = self.data[idx]
        label = self.labels[idx]
        return {"audio": sample, "labels": label}

# ---------------------------
# Dummy Teacher & Student Models
# ---------------------------

class DummyTeacher(nn.Module):
    def __init__(self, input_length=16000, num_classes=10):
        super(DummyTeacher, self).__init__()
        # 간단한 1D CNN 기반 모델
        self.conv = nn.Conv1d(in_channels=1, out_channels=16, kernel_size=3, stride=1, padding=1)
        self.relu = nn.ReLU()
        self.pool = nn.AdaptiveAvgPool1d(1)
        self.fc = nn.Linear(16, num_classes)
    
    def forward(self, x):
        # x: (batch, num_samples)
        x = x.unsqueeze(1)  # (batch, 1, num_samples)
        x = self.conv(x)
        x = self.relu(x)
        x = self.pool(x).squeeze(-1)  # (batch, 16)
        logits = self.fc(x)          # (batch, num_classes)
        return logits

class DummyStudent(nn.Module):
    def __init__(self, input_length=16000, num_classes=10):
        super(DummyStudent, self).__init__()
        # 더 경량화된 모델
        self.conv = nn.Conv1d(in_channels=1, out_channels=8, kernel_size=3, stride=1, padding=1)
        self.relu = nn.ReLU()
        self.pool = nn.AdaptiveAvgPool1d(1)
        self.fc = nn.Linear(8, num_classes)
    
    def forward(self, x):
        # x: (batch, num_samples)
        x = x.unsqueeze(1)  # (batch, 1, num_samples)
        x = self.conv(x)
        x = self.relu(x)
        x = self.pool(x).squeeze(-1)  # (batch, 8)
        logits = self.fc(x)          # (batch, num_classes)
        return logits

# ---------------------------
# KD Loss Functions
# ---------------------------

# KL Divergence Loss (logit 기반 KD)
kl_loss_fn = nn.KLDivLoss(reduction="batchmean", log_target=True)
ce_loss_fn = nn.CrossEntropyLoss()

def kd_loss(teacher_logits, student_logits, T=2.0):
    """
    Teacher와 Student의 logit 분포 차이를 KL Divergence Loss로 계산
    """
    teacher_probs = torch.nn.functional.log_softmax(teacher_logits / T, dim=-1)
    student_probs = torch.nn.functional.log_softmax(student_logits / T, dim=-1)
    return kl_loss_fn(student_probs, teacher_probs)

# ---------------------------
# Training Loop
# ---------------------------

def train_da_kd(
    teacher_model,
    student_model,
    dataloader,
    optimizer,
    device,
    num_epochs=10,
    T=2.0,
    kd_weight=0.5,
    ce_weight=0.5
):
    teacher_model.eval()  # Teacher는 고정
    student_model.train()
    
    # 데이터 증강 파이프라인 (ASR용)
    aug_transforms = get_transforms_asr()
    
    for epoch in range(num_epochs):
        epoch_loss = 0.0
        
        for batch in dataloader:
            # 원본 오디오와 라벨 불러오기
            audio = batch["audio"]  # numpy array (batch, num_samples)
            labels = batch["labels"]
            
            # audiomentations는 numpy array 입력을 받으므로,
            # batch 내 각 샘플에 증강 적용
            augmented_audio = []
            for sample in audio:
                aug_sample = aug_transforms(samples=sample, sample_rate=16000)
                augmented_audio.append(aug_sample)
            augmented_audio = np.stack(augmented_audio)
            
            # tensor로 변환 및 device 이동
            augmented_audio = torch.tensor(augmented_audio).float().to(device)
            labels = torch.tensor(labels).long().to(device)
            
            # Teacher 예측 (고정된 모델이므로 gradient 계산 불필요)
            with torch.no_grad():
                teacher_logits = teacher_model(augmented_audio)
            
            # Student 예측
            student_logits = student_model(augmented_audio)
            
            # KD Loss (soft label)와 Hard Label Loss (CE)
            loss_kd = kd_loss(teacher_logits, student_logits, T=T)
            loss_ce = ce_loss_fn(student_logits, labels)
            
            # 최종 Loss 결합
            loss = kd_weight * loss_kd + ce_weight * loss_ce
            
            optimizer.zero_grad()
            loss.backward()
            optimizer.step()
            
            epoch_loss += loss.item()
        
        avg_loss = epoch_loss / len(dataloader)
        print(f"Epoch {epoch+1}/{num_epochs}, Loss: {avg_loss:.4f}")

# ---------------------------
# Main 실행부
# ---------------------------

if __name__ == '__main__':
    # device 설정
    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    
    # 하이퍼파라미터
    num_classes = 10
    sample_length = 16000
    batch_size = 16
    num_epochs = 5
    learning_rate = 1e-4
    
    # Dummy 데이터셋 및 DataLoader 생성
    dataset = DummyAudioDataset(num_samples=200, sample_length=sample_length, num_classes=num_classes)
    dataloader = DataLoader(dataset, batch_size=batch_size, shuffle=True)
    
    # Teacher와 Student 모델 생성
    teacher_model = DummyTeacher(input_length=sample_length, num_classes=num_classes).to(device)
    student_model = DummyStudent(input_length=sample_length, num_classes=num_classes).to(device)
    
    # Teacher는 미리 학습되었다고 가정하므로, 여기서는 weight 초기화를 다르게 할 수도 있음.
    # 데모를 위해 teacher_model은 무작위 weight로 두지만, 실제로는 학습된 모델을 로드합니다.
    
    # Optimizer 설정
    optimizer = optim.Adam(student_model.parameters(), lr=learning_rate)
    
    # KD + DA 학습 수행
    train_da_kd(
        teacher_model=teacher_model,
        student_model=student_model,
        dataloader=dataloader,
        optimizer=optimizer,
        device=device,
        num_epochs=num_epochs,
        T=2.0,
        kd_weight=0.5,
        ce_weight=0.5
    )

```