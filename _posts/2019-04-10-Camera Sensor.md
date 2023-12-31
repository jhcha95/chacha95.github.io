---
layout: post
title: Bayer Pattern 
tags: [Image Processing]
---

# 카메라는 어떻게 사진을 촬영하는 것인가?

카메라 본체에서 셔터를 누르는 순간 CMOS/CCD와 같은 센서에서 인식을 해 RAW 이미지를 만들고 이를 카메라 ISP(Image Signal Processing)을 통해 우리가 눈으로 보는 영상을 만들어 냅니다.

<br>

## 카메라의 필름 역할을 하는 '이미지 센서'

이미지 센서는 피사체 정보를 읽어 전기적인 영상신호로 변환해 주는 장치입니다. 즉 빛 에너지를 전기적 어네지로 변환해 영상으로 만드는데 카메라의 필름과 같은 역할을 합니다.

렌즈를 통해 들어온 빛을 전기적 디지털 신호로 변환해 주는 역활입니다.

카메라의 센서는 CCD 센서와 CMOS 센서 크게 두가지로 나뉩니다.

이미지센서는 응용 방식과 제조 공정에 따라 CCD 이미지센서와 CMOS 이미지센서로 나눌 수 있습니다. CCD 이미지센서는 전자 형태의 신호를 직접 전송하는 방식이며, CMOS 이미지센서 대비 노이즈가 적다는 장점을 가지고 있습니다. 반면 CMOS 이미지센서는 신호를 전압 형태로 변환해 전송하는 방식으로 CMOS 제조 공정이 사용되어 가격경쟁력이 있다는 장점이 있습니다. 

이러한 특징 때문에 과거 CCD 이미지센서는 디지털카메라에 사용되고, CMOS 이미지센서는 주로 휴대폰에 사용됐습니다. 하지만 휴대폰, 태블릿PC 등 카메라 기능이 탑재된 모바일 기기의 시장이 확대되면서 핵심 부품으로 CMOS 이미지센서가 주목받기 시작했습니다. 특히 모바일 기기는 전력 소비를 줄이는 것이 중요한데 이는 배터리 사용 시간 연장과 직결되기 때문입니다. 때문에 CCD 이미지센서 대비 저전력 공정으로 소비전력에서 강점을 가지는 CMOS 이미지센서가 핵심 부품으로 떠오르게 된 것입니다. 또한, CMOS 이미지센서는 노이즈 등 성능이 매우 개선되고 동영상 지원 기능과 가격 측면에서도 지속적인 우위를 가지면서 최근에는 DSLR 카메라에도 많이 탑재되고 있습니다. 

이런 CMOS, CCD 이미지 센서에는 R, G, B 필터들이 일정한 패턴을 가지고 배치됩니다.

배치에도 포베온 패턴과 베이어 패턴 두가지가 존재합니다.

> 포베온 패턴(좌), 베이어 패턴(우)

<center><img src="https://user-images.githubusercontent.com/31475037/58937564-bc509380-87ad-11e9-90a0-8da5daf54d65.png"> </center>
<br>

포베온 패턴은 모든 공간에 모든색을 담는 방식으로 R, G, B 삼색 모두를 담습니다. 반면 베이어 패턴은 인간의 광학적 특성에 따라 R 25% G 50% B 25%가 되도록 배치됩니다. 

G가 50%인 것은 다음과같은 이유 때문입니다.

> G는 R과 B의 중간이여서 둘 중 어떤 색과도 보간(interpolation)하기가 쉽다.

<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/RGB_LED_Spectrum.svg/1280px-RGB_LED_Spectrum.svg.png" width="90%" height="50%"> </center>
베이어 패턴에서 각 센서는 R, G, B 값의 밝기만을 감지하는 monochrome 화소입니다.

베이어 패턴에 비해 포베온 패턴이 화질이 더 좋지만, 화질을 제외한 모든 부분에서 안 좋기에 대다수의 센서에서는 베이어 패턴을 사용합니다.

특히나 베이어 패턴에 비해 포베온 패턴이 3배의 용량을 더 차지하기에 효율성이 떨어집니다.

> 베이어 패턴 필터로 생성된 raw image 데이터

<center><img src="https://user-images.githubusercontent.com/31475037/58937958-ccb53e00-87ae-11e9-9272-84353e91bc8d.png"> </center>
베이어 필터 패턴를 쓰는 이미지 센서는 각 화소에서 R, G, B 중 어느 한 색만을 감지할 수 있지만, 우리가 보는 카메라 영상에선는 각 화소마다 R, G, B 전체 색상을 보여줍니다. 이것이 가능한 이유는 카메라 ISP에서 각 화소마다 주변 셀들의 색상값을 interpolation(보간)해주기 때문입니다.

<br>

**참고 글**

[SamSung 블로그](https://www.samsungsemiconstory.com/642)