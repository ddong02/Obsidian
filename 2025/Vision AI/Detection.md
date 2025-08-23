# Dataset

### Test1
https://github.com/ddong02/Object-Detection/commit/29268e0ef54a5eac1c7e3c630674f91cd7037ad1

```python
class Custom_COCO_Dataset(Dataset):
    def __init__(self, root, annFile, transforms=None):
        self.root = root
        self.transforms = transforms
        self.coco = CocoDetection(root, annFile)
        self.coco_api = self.coco.coco
        self.cat_ids = self.coco_api.getCatIds()
        print(f"cat_ids: {self.cat_ids}")
        self.cat2label = {cat_id: i+1 for i, cat_id in enumerate(self.cat_ids)}
        print(f"cat2label\n{self.cat2label}")
        categories = self.coco_api.loadCats(self.cat_ids)
        print(f"categories: {categories}")
        print(f"### Dataset info")
        print(f"Total images num: {len(self.coco)}")
        print(f"Class num: {len(self.cat_ids)}")
        print(f"Class name: {[cat['name'] for cat in categories]}")

train_image_dir = os.path.join('data', 'dataset2', 'train', 'JPEGImages')
train_ann_path = os.path.join('data', 'dataset2', 'train', 'annotations.json')
print(f"train image dir: {train_image_dir}")
print(f"train ann path: {train_ann_path}")

temp = Custom_COCO_Dataset(train_image_dir, train_ann_path)
```
![[Pasted image 20250823123148.png]]

`torchvision.datasets.CocoDetection(root, annFile)`
coco 데이터셋에 접근할 수 있게 해주는 클래스. `pycocotools` 패키지를 설치해야한다.

`CocoDetection.coco` 는 `pycocotools.coco` 모듈의 `COCO` 클래스의 객체이다.
이는 제공된 어노테이션 파일을 파싱하여 `coco` 객체를 초기화한다.

`coco.getCatIds()`는 coco 데이터셋의 카테고리 ID를 조회하는 메서드이다.
`coco.loadCats()`는 `getCatIds()`로 얻은 카테고리 ID를 사용해 카테고리에 대한 정보를 로드하는 메서드이다.

- 어노테이션 파일 경로 수정
기존의 어노테이션 파일에는 파일 이름이 다음과 같이 되어 있다.
`"file_name": "JPEGImages\\IMG_7725.jpg"`
`\\` → 맥 환경에서 파일 불러오는 과정에서 에러가 발생
![[Pasted image 20250823145024.png]]

`\\` 를 `/` 로 변환해준다.

```python
import json
import os

def fix_coco_annotations(input_file_path, output_file_path):
    """
    COCO annotation 파일의 파일 경로에서 백슬래시를 슬래시로 변경합니다.
    """
    with open(input_file_path, 'r', encoding='utf-8') as f:
        data = json.load(f)

    # images 리스트 순회하며 file_name 수정
    for image_info in data.get('images', []):
        if 'file_name' in image_info:
            image_info['file_name'] = image_info['file_name'].replace('\\', '/')

    with open(output_file_path, 'w', encoding='utf-8') as f:
        json.dump(data, f, indent=4)

    print(f"파일 경로가 수정되었습니다. 수정된 파일: {output_file_path}")

if __name__ == "__main__":
    input_file = '/Users/eogus/Library/CloudStorage/OneDrive-군산대학교/visionAI/detection/data/dataset2/train/annotations.json'
    output_file = '/Users/eogus/Library/CloudStorage/OneDrive-군산대학교/visionAI/detection/data/dataset2/train/annotations_mac.json'
    
    if os.path.exists(input_file):
        fix_coco_annotations(input_file, output_file)
    else:
        print(f"오류: 입력 파일 '{input_file}'을 찾을 수 없습니다.")
```
![[Pasted image 20250823145136.png]]

변환된 ann 파일
![[Pasted image 20250823145157.png]]

`__getitem__()`, `__len__()` → 어노테이션 파일에서 정보 추출 (bbox, labels, image_id, areas ···)

```python
    
    def __getitem__(self, idx):
        img, target = self.coco[idx]
        image_id = self.coco.ids[idx]
        boxes = []
        labels = []
        areas = []
        iscrowd = []

        for annotation in target:
            bbox = annotation['bbox']
            x1, y1, w, h = bbox
            x2, y2 = x1 + w, y1 + h
            boxes.append([x1, y1, x2, y2])
            cat_id = annotation['category_id']
            labels.append(self.cat2label[cat_id])
            areas.append(annotation['area'])
            iscrowd.append(annotation['iscrowd'])
        
        boxes = torch.as_tensor(boxes, dtype=torch.float32)
        labels = torch.as_tensor(labels, dtype=torch.int64)
        image_id = torch.tensor([image_id])
        areas = torch.as_tensor(areas, dtype=torch.float32)
        iscrowd = torch.as_tensor(iscrowd, dtype=torch.int64)

        target = {
            'boxes': boxes,
            'labels': labels,
            'image_id': image_id,
            'area': areas,
            'iscrowd': iscrowd
        }

        if self.transforms is not None:
            img = self.transforms(img)

        print(f"img\n{img}")
        print(f"target\n{target}")

        return img, target
    
    def __len__(self):
        return len(self.coco)
```

훈련 데이터
![[Pasted image 20250823151132.png]]

검증 데이터
![[Pasted image 20250823151938.png]]
https://github.com/ddong02/Object-Detection/commit/9e79847b473aff66b18ebac44d3657eabaa46486

# Model

```python
def get_model(num_classes, freeze_backbone=True):
    model = fasterrcnn_resnet50_fpn(pretrained=True)
    backbone = resnet_fpn_backbone('resnet101', pretrained=True,
                                   trainable_layers=0 if freeze_backbone else 5)
    model.backbone = backbone

    in_features = model.roi_heads.box_predictor.cls_score.in_features
    model.roi_heads.box_predictor = FastRCNNPredictor(in_features, num_classes)

    if freeze_backbone:
        for param in model.backbone.parameters():
            param.requires_grad = False
        print(f"backbone frozen. num_classes: {num_classes}")
    
    return model
```

backbone = resnet101
backbone freeze
num_classes = 3

![[Pasted image 20250823154308.png]]

`box_predictor` 클래스 예측, 바운딩 박스 회귀의 작업을 함.

# Collate

```python
def collate_fn(batch):
    return tuple(zip(*batch))
```

`collate_fn`이 필요한 이유?
`DataLoader`의 기본 `collate`함수는 같은 크기의 텐서만을 입력으로 받을 수 있음.
detection에서는 annotation에 따라 객체의 수가 달라지는 등 매번 같은 크기의 텐서를 입력할 수 없음.
따라서 가변적인 값들을 다루기 위해서 직접 만들어 줘야 함.
`DataLoader`의 매개변수로 전달해서 사용

`*` 는 unpacking 연산자로 튜플 등을 개별적인 값으로 풀어줌
`zip` 은 같은 인덱스의 값들을 묶어줌
이 과정을 통해 이미지는 이미지끼리, 타겟은 타겟끼리 묶임
![[Pasted image 20250823160321.png]]

https://github.com/ddong02/Object-Detection/commit/122a6bf786833fe341f69d5b5e1ea5fec89e5c91

# Train & Evaluate

```python
def train_one_epoch(model, optimizer, data_loader, device):
    model.train()
    running_loss = 0.0

    for i, (images, targets) in enumerate(tqdm(data_loader, desc='Train')):
        images = list(image.to(device) for image in images)
        for target in targets:
            for key, value in target.items():
                target[key] = value.to(device)

        loss_dict = model(images, targets)
        losses = sum(loss for loss in loss_dict.values())

        optimizer.zero_grad()
        losses.backward()
        optimizer.step()

        running_loss += losses.item()
    
    return running_loss / len(data_loader)

def evaluate(model, data_loader, device):
    model.eval()
    total_loss = 0.0

    with torch.no_grad():
        for images, targets in tqdm(data_loader, desc='Valid'):
            images = list(image.to(device) for image in images)
            targets = [{k: v.to(device) for k, v in t.items()} for t in targets]
            # classification과 달리 detection에서는 model.train()에서만 Loss 계산함.
            model.train()
            loss_dict = model(images, targets)
            losses = sum(loss for loss in loss_dict.values())
            total_loss += losses.item()
            model.eval()

    return total_loss / len(DataLoader)
```

detection에서는 classification과 다르게 손실값을 계산하기 위해 `model.train()`으로 설정해줘야 한다.
1. 손실함수의 복잡성
- 분류 손실: 각 예측 박스에 대해 객체가 어떤 클래스에 속하는지 예측
- 바운딩 박스 회귀 손실: 예측된 바운딩 박스와 실제 정답 바운딩 박스 간의 위치 차이를 줄이는 데 사용(IoU)
- 객체성 손실: 이미지 내에 객체가 있는지 여부를 판단
즉, 손실 계산 과정이 classification보다 복잡해서 train 모드로 해야함

https://github.com/ddong02/Object-Detection/commit/f57b2e99cd3b50f20766118ba52c83f983edb26d

# 