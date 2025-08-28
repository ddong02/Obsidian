
# 데이터셋 준비

**Cityscapes** 데이터셋을 COCO로, COCO를 YOLO 포맷으로 변환
https://github.com/TillBeemelmanns/cityscapes-to-coco-conversion
![[Pasted image 20250827130249.png]]

![[Pasted image 20250827130621.png]]

annotations 파일의 크기는 180MB, 30MB

![[Pasted image 20250827130550.png]]

![[Pasted image 20250827130322.png]]

coco를 yolo포맷으로 변환
`from ultralytics.data.converter import convert_coco` 이용
converter의 convert_coco 수정
![[Pasted image 20250827142535.png]]

https://github.com/ultralytics/JSON2YOLO/issues/125