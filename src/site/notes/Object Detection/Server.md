---
{"dg-publish":true,"permalink":"/object-detection/server/"}
---

💥 서버 폭파 후 복구하기
1. apt-get update -y&&apt-get install -y libgl1-mesa-glx&&apt-get install -y libglib2.0-0&&apt-get install wget -y&&cd ~&&wget [https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/20240902115340/code.tar.gz&&wget](https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/20240902115340/code.tar.gz&&wget) [https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/data.tar.gz&&tar](https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/data.tar.gz&&tar) -zxvf data.tar.gz&&tar --strip-components=1 -xzf code.tar.gz
2. requirements.txt의 마지막 줄을 지운다.
3. pip install -r requirements.txt&&mim install mmcv-full==1.7.0&&apt install git-all -y&&pip install lightning==2.1 torch==1.12.1&&pip install "ray[data,train,tune,serve]" wandb&&pip install protobuf==3.19.6&&apt-get install tmux -y&&cd ~/mmdetection&&pip install -v -e .&&cd /home&&mkdir bohyun&&cd bohyun&&mkdir proj2&&cd proj2

💥 복구 변형 버전
1. apt-get update -y&&apt-get install -y libgl1-mesa-glx&&apt-get install -y libglib2.0-0&&apt-get install wget -y&&cd ~&&wget [https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/20240902115340/code.tar.gz&&wget](https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/20240902115340/code.tar.gz&&wget) [https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/data.tar.gz&&tar](https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/data.tar.gz&&tar) -zxvf data.tar.gz&&tar --strip-components=1 -xzf code.tar.gz
2. apt install git-all -y&&pip install lightning==2.1 torch==1.12.1&&pip install "ray[data,train,tune,serve]" wandb&&pip install protobuf==3.19.6&&apt-get install tmux -y&&cd ~/mmdetection&&pip install -v -e .&&cd /home&&mkdir github&&cd github&&mkdir proj2&&cd proj2