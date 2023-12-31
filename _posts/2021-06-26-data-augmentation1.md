---
layout: post
title: TTA(Test Time Augmentation) 소개
tags: [ML]
use_math: true
---

TTA에 대해 이해하기 위해선 먼저 data augmentation에 대해 알아야 합니다.

# What is Data augmentation?

보통 머신 러닝 학습시 데이터 추가에 많은 비용과 시간이 들기에 데이터 개수를 증강시키는 **data augmentation** 기법을 씁니다. Data Augmentation은 기존 데이터에서 augmentation된s 데이터를 생성해 데이터셋 크기를 인위적으로 확장합니다.

<br>

# Methods

## Geometric

Geometric augmentation의 방법으론 flip, crop, rotate, multisize로 이미지를 만드는 방법이 있습니다.

> 이미지 자체를 자르거나 크기를 수정함

![](https://user-images.githubusercontent.com/31475037/123568246-e34be900-d7fe-11eb-97b2-569aed7227d9.png)

<br>

## Color

Color augmentation의 방법으론 color(RGB or HSV) 값을 증가시키거나 감소 시키는 방법이 있습니다.

> HSV space에서 augmentation

![](https://user-images.githubusercontent.com/31475037/123568243-e2b35280-d7fe-11eb-9147-a1f68810262a.png)

<br>

## Kernel filters

Kernel filter augmentation의 방법으론 sharpen, blur 와같은 필터를 씌우는 방식이 있습니다.

> filter를 통한 방식

![](https://user-images.githubusercontent.com/31475037/123568242-e21abc00-d7fe-11eb-964d-e18aed82b9a5.png)

<br>

## Erasing

image에 random 크기의 bounding box를 만든 뒤 그 안을 random noise, ImageNet mean value, [0, 0, 0], [255, 255, 255] 값 등으로 채워서 학습하는 방식입니다.

> [255, 255, 255] bounding box 생성

![](https://user-images.githubusercontent.com/31475037/123568239-e1822580-d7fe-11eb-8350-af7b384cfade.png)

# What is TTA?

TTA(Test Time Augmentation)이란, 모델을 test(inference)시 augmentation을 하는 방식입니다. 기존 data augmentation을 이용한 성능 향상은 모델 학습 과정 중 적용했지만, TTA는 이미 학습된 모델에 적용 가능합니다. 일종의 data ensemble이라 볼 수도 있습니다.

<br>

## TTA example

TTA의 예시는 다음과 같습니다. 원본 입력 이미지 한 장을 augmentation해 2장을 생성해냅니다. 총 3장의 이미지를 학습된 모델에 각각 입력해주고, 모델에서 나온 예측 확률 값(score)을 평균해 최종 클래스를 예측합니다.

> TTA example

![](https://user-images.githubusercontent.com/31475037/123568577-94eb1a00-d7ff-11eb-8182-14abc59ce384.png)

<br>

# TTA in Image Classification

Image Clssification의 경우 굉장히 다양한 augmentation 기법이 사용되고 있습니다. Flip, crop, scale, rotate, shift를 보통 많이 사용합니다.

> Image classication에서 TTA

![](https://user-images.githubusercontent.com/31475037/123568575-94eb1a00-d7ff-11eb-9aa6-92ca1ade1895.png)

<br>

# TTA in Object Detection

Object detection에선 HorizontalFlip과 Multisize 기법이 많이 쓰이고 있습니다. 이는 bbox나 mask의 존재 및 vertical flip과 같은 기법을 쓰게 되면 성능이 오히려 떨어지거나 미미한 성능향상에 그치기 때문입니다.

다만, Multisize 기법이나 Horizontal flip을 사용하게 되면 성능이 굉장히 많이 올라 갑니다.

> Object detection에서 TTA

![](https://user-images.githubusercontent.com/31475037/123568573-93b9ed00-d7ff-11eb-8317-2eb793154155.png)

<br>

## Post processing in Faster R-CNN

보통 많이 사용되는 Faster R-CNN 계열 모델의 경우 TTA 사용시 다음의 과정을 거쳐 bbox를 검출해 냅니다.

### 1. Augmentation

원본 이미지에 대해 augmentation을 합니다.

### 2. Inference

원본 이미지, augmentation 이미지에 대해 각각 inference 해줍니다. 

### 3. Bbox aggregation

inference 결과로 나온 모든 bbox를 모아줍니다.

### 4. Confidence score threshold

Faster R-CNN 모델이 bbox에 대해 예측을하고, 각 bbox에 대한 confidence score를 예측합니다. 예측한 값 중에서 confidence score threshold 보다 낮은 값은 bbox로 사용하지 않습니다.

### 5. Non-Maximum Suppression(NMS)

남은 bbox 중 겹치는 bbox를 제거하는 과정이 NMS(Non Maximum Supression) 과정입니다. Bbox 끼리 IOU가 NMS threshold보다 높으면, confidence score가 가장 높은 bbox만 남깁니다.

> NMS threshold

![](https://user-images.githubusercontent.com/31475037/123568571-93215680-d7ff-11eb-8375-39a01e6f2cfc.jpg)

**참고자료**

[Data Augmentation in Python: Everything You Need to Know - neptune.ai](https://neptune.ai/blog/data-augmentation-in-python)

[Papers with Code - Image Augmentation](https://paperswithcode.com/task/image-augmentation)

[Image Data Augmentation Overview](https://hoya012.github.io/blog/Image-Data-Augmentation-Overview/)
