---
layout: post
title: "광통신공학 11강: Li-Fi, 위성 광통신과 FSO 백홀"
date: 2026-09-16 03:11:00 +0900
categories: [optical-communication]
tags:
  - optical-wireless-communication
  - li-fi
  - satellite-optical-communication
  - free-space-optics
math: true
---

## 광무선통신: Li-Fi, 위성 광통신, FSO 모바일 백홀

광무선통신(Optical Wireless Communications, OWC)은 광섬유 대신 자유공간을 전파하는 빛에 정보를 실어 보내는 통신 방식이다. 실내에서는 LED 조명과 포토다이오드(Photo Detector, PD)를 이용한 Li-Fi/VLC로, 지상과 공중에서는 Free-Space Optical communication(FSO)으로, 우주에서는 Satellite Optical Communication(SOC)으로 구현된다. 세 응용은 거리와 운용 환경이 크게 다르지만 다음의 공통 문제를 공유한다.

- 송신 광원과 수신기의 대역폭 및 감도
- 자유공간 채널의 감쇠, 난류, 배경광 잡음
- 변조, 다중화, 채널 코딩
- 좁은 광빔을 유지하기 위한 Pointing, Acquisition, Tracking(PAT)
- 링크 단절(outage)을 줄이기 위한 채널 적응과 중계

이 장에서는 먼저 OWC가 필요한 이유를 살펴보고, Li-Fi와 가시광 측위, 위성 광통신, FSO 모바일 백홀의 순서로 범위를 확장한다.

## 1. 전파 무선통신에서 광무선통신으로

### 1.1 주파수와 파장

전자기파의 주파수와 파장은

$$
f=\frac{c}{\lambda}
$$

의 관계를 갖는다. 여기서 $c$는 빛의 속도, $f$는 주파수, $\lambda$는 파장이다. RF 무선통신은 주로 GHz 대역을 이용하지만, 가시광과 근적외선은 수백 THz에 해당한다. 광 반송파 자체가 높은 주파수라는 사실은 곧바로 데이터율을 정하지는 않지만, 이용 가능한 변조 대역폭과 다중화 자원을 크게 확장할 잠재력을 제공한다.

대역폭 $B$와 신호대잡음비 $\mathrm{SNR}$이 주어진 이상적인 채널의 용량은 Shannon 식으로 표현할 수 있다.

$$
C=B\log_2(1+\mathrm{SNR})
$$

OWC의 높은 전송 용량은 큰 $B$를 확보할 수 있다는 장점에서 출발한다. 다만 실제 링크에서는 LED, 레이저, 광검출기, 구동 회로와 대기 채널이 모두 유효 대역폭과 SNR을 제한한다.

### 1.2 OWC의 장점과 제약

강의안이 제시한 OWC의 핵심 장점은 다음과 같다.

1. **비면허 광 주파수 자원**: 광 주파수는 RF처럼 주파수 할당에 직접 묶이지 않는다.
2. **광대역성**: 높은 반송 주파수와 WDM 등의 다중화로 큰 전송 용량을 구성할 수 있다.
3. **전자기 간섭 회피**: RF 사용이 제한되거나 EMI가 민감한 환경에서 유리하다.
4. **지향성과 보안성**: 빛은 직진성이 강하고 빔을 좁힐 수 있어 공간적 누설을 줄인다.
5. **장거리 확장성**: 적절한 광학계와 PAT를 결합하면 지상, 공중, 위성 간 링크로 확장할 수 있다.

반면 좁은 빔은 수신 전력을 높이는 동시에 정렬 오차에 민감하게 만든다. 즉, 지향성은 보안과 링크 버짓에는 이점이지만 PAT 관점에서는 부담이다.

### 1.3 응용 영역과 거리 규모

OWC의 범위는 실내 atto-cell부터 위성 간 링크까지 이어진다.

- **Li-Fi/VLC**: 실내 LED 조명을 이용한 통신 및 위치 측정
- **Optical Camera Communication**: 교통 신호등, 가로등, 차량 조명에서 카메라 수신기로 정보를 전달
- **FSO 모바일 백홀**: 지상 기지국과 UAV/HAP를 연결하는 수직·수평 광 링크, 강의안의 목표 운용 거리는 약 20 km
- **SOC**: LEO/GEO 위성, 위성 간 링크(Inter-Satellite Link, ISL), 위성-지상 및 심우주 링크

## 2. Li-Fi와 가시광통신(VLC)

### 2.1 Li-Fi의 기본 구조

Li-Fi는 LED 가시광 스펙트럼에 데이터를 변조해 전송하고 PD로 수신하는 VLC 기반 무선통신 시스템이다. 기본적인 intensity modulation/direct detection(IM/DD) 링크는

$$
y(t)=R\,h(t)*x(t)+n(t)
$$

로 모델링할 수 있다.

- $x(t)\ge 0$: LED에 인가되어 방사 광세기를 변화시키는 실수 신호
- $h(t)$: 광 채널의 임펄스 응답
- $R$ [A/W]: PD의 responsivity
- $n(t)$: shot noise, thermal noise, 배경광 잡음을 포함한 등가 잡음

IM/DD에서는 정보 신호가 광세기에 실리므로 $x(t)$는 음수가 될 수 없다. 따라서 RF에서 쓰는 복소 기저대역 변조를 그대로 적용하기 어렵고, DC bias나 비대칭 클리핑을 포함한 광 OFDM 구조가 필요할 수 있다.

### 2.2 atto-cell과 기술 과제

천장 LED 하나 또는 작은 LED 군집이 매우 작은 셀을 형성한다. 이른바 atto-cell 구조는 공간 재사용률을 높이지만 셀 간 간섭과 이동 시 handover를 어렵게 한다. 강의안은 Li-Fi의 과제를 다음 순서로 정리한다.

1. 기가비트 전송을 위한 송수신기 성능 향상
2. 고효율 변조 및 다중화
3. Massive MIMO
4. uplink 구성, 특히 무선 적외선 링크

실내 다운링크에는 조명 LED를 자연스럽게 활용할 수 있지만, 단말 uplink에 가시광을 쓰면 눈부심과 소비전력 문제가 생긴다. 따라서 uplink를 적외선으로 분리하는 비대칭 구조가 실용적이다.

### 2.3 VLC 시스템을 구성하는 네 가지 문제

VLC 링크는 LED driver - LED - 광 채널 - PD - 수신 회로로 이어진다. 설계 문제는 네 묶음으로 나뉜다.

1. **Tx/Rx**: 대역폭 확장, 고감도 수신기, uplink 송수신 구조
2. **변조**: flicker와 dimming을 만족하는 다중반송파 변조
3. **채널**: 광 채널 모델, MIMO, WDM/PDM/SDM 다중화
4. **셀 계획**: 조명 배치, 간섭, 사용자 이동과 handover

조명 기능을 유지하려면 평균 광전력 $P_{\mathrm{avg}}$가 목표 밝기를 만족해야 한다.

$$
P_{\mathrm{avg}}=\frac{1}{T}\int_0^T x(t)\,dt
$$

통신 신호는 이 평균값 주변에서 충분히 빠르게 변해야 사람이 flicker를 느끼지 않으며, dimming 비율을 바꾸더라도 신호의 동적 범위와 비음수 조건을 만족해야 한다.

### 2.4 LED 대역폭 제한

백색 LED는 흔히 청색 LED와 형광체의 황색 성분을 결합한다. 강의안에 제시된 3 dB 대역폭은 다음과 같다.

- 백색 LED 전체: 약 2-2.5 MHz
- 황색 성분: 약 2-2.5 MHz
- 청색 성분: 약 10 MHz

단일 극점으로 근사한 LED의 주파수 응답은

$$
H_{\mathrm{LED}}(f)=\frac{1}{1+jf/f_c},
\qquad
\left|H_{\mathrm{LED}}(f_c)\right|^2=\frac{1}{2}
$$

로 쓸 수 있다. $f_c$가 3 dB 대역폭이다. 수신 PD의 대역폭이 LED보다 훨씬 넓다면 전체 링크 대역폭은 LED가 지배한다.

#### 대역폭을 넘는 전송

채널과 LED의 결합 응답을 $H(f)$라 하면 송신 또는 수신 등화기는 이상적으로

$$
G_{\mathrm{eq}}(f)\approx \frac{1}{H(f)}
$$

를 목표로 한다. 그러나 $|H(f)|$가 작은 고주파 성분을 역필터링하면 잡음까지 증폭된다. 따라서 실제 설계에서는 pre-emphasis, post-equalization, 다중반송파 bit loading, 청색 성분 분리 등을 조합한다. 강의안은 제한된 백색 LED로 100 Mb/s NRZ를 구현한 post-equalization 사례를 제시한다.

## 3. VLC 기반 실내 측위

### 3.1 왜 가시광 측위인가

VLC 기반 Indoor Positioning System(IPS)은 이미 설치된 LED 조명 인프라를 beacon으로 활용한다. 추가 AP 설치 비용을 줄일 수 있고, EMI의 영향을 적게 받으며, 위치 기반 서비스·실내 내비게이션·공간 관리에 적용할 수 있다.

강의안의 비교에서 VLC 측위는 3 cm 미만의 정확도를 목표로 하며, 저비용과 고정밀이 장점이다. 다만 광원 가시성이 필요한 실내 환경에 범위가 제한된다. Bluetooth, Wi-Fi, ZigBee, cellular network, GPS는 적용 범위와 전력, 환경 의존성, 정확도 사이에서 서로 다른 절충점을 갖는다.

### 3.2 Lambertian 채널과 RSS 거리 추정

LED를 Lambertian 방사원으로 보면 방사 차수 $m$은 LED의 반전력각 $\Phi_{1/2}$로 정해진다.

$$
m=-\frac{\ln 2}{\ln(\cos\Phi_{1/2})}
$$

LOS 조건의 DC channel gain은 보통

$$
H(0)=
\frac{(m+1)A_r}{2\pi d^2}
\cos^m(\phi)\,T_s(\psi)\,g(\psi)\cos(\psi)
$$

로 표현한다. 여기서 $A_r$은 검출기 면적, $d$는 LED와 수신기 거리, $\phi$는 irradiance angle, $\psi$는 incidence angle, $T_s$는 optical filter gain, $g$는 concentrator gain이다. 수신 광전력은

$$
P_r=P_tH(0)
$$

이므로, 송신 전력과 자세가 알려져 있으면 RSS로부터 $d$를 추정할 수 있다. 단순 정렬 조건에서는 $P_r\propto d^{-2}$에 가깝지만, 실제로는 $\cos^m\phi\cos\psi$ 항 때문에 자세 오차가 거리 오차로 바뀐다.

### 3.3 삼변측량

위치가 알려진 LED $i$의 좌표를 $(x_i,y_i,z_i)$, 수신기 좌표를 $(x,y,z)$라 하면 추정 거리 $\hat d_i$는

$$
(x-x_i)^2+(y-y_i)^2+(z-z_i)^2=\hat d_i^2
$$

를 만족한다. 높이 $z$를 알고 있는 2차원 측위라면 최소 3개의 독립적인 LED 신호가 필요하다. 첫 식을 나머지 식에서 빼면 제곱항이 제거되어 선형식

$$
2(x_i-x_1)x+2(y_i-y_1)y
=
\hat d_1^2-\hat d_i^2+x_i^2-x_1^2+y_i^2-y_1^2
$$

을 얻는다. 측정이 3개보다 많으면 최소제곱법으로 위치를 구한다.

$$
\hat{\mathbf p}
=(\mathbf A^{\mathsf T}\mathbf A)^{-1}\mathbf A^{\mathsf T}\mathbf b
$$

강의안의 연구 사례는 carrier allocation으로 셀 간 간섭을 제어하고 각 carrier의 RSS로 거리를 추정해, 3.61 m 거리에서 1.87 cm 미만의 정확도를 보였다.

#### 설계 관점

- LED 식별을 위해 carrier 또는 시간 슬롯을 분리해야 한다.
- 위치 오차는 RSS 오차뿐 아니라 LED 좌표, 수신기 기울기, 가림에도 민감하다.
- LED의 기하 배치가 나쁘면 $\mathbf A^{\mathsf T}\mathbf A$의 조건수가 커져 작은 거리 오차가 큰 위치 오차로 증폭된다.

## 4. 위성 광통신(SOC)

### 4.1 RF 우주통신을 보완하는 이유

SOC는 지상국-위성, 위성-위성, 위성-항공기 링크에 레이저를 사용한다. 강의안은 RF 대비 장점을 넓은 대역폭, 비면허 스펙트럼, 높은 보안성, 낮은 요구 전력, 소형화로 정리한다. 이는 우주 시스템의 핵심 지표인 SWaP-c, 즉 size, weight, power, cost 절감으로 이어진다.

광 송신의 지향성은 회절로 설명할 수 있다. 원형 개구의 회절 제한 발산각은 대략

$$
\theta_{\mathrm{div}}\approx 1.22\frac{\lambda}{D_t}
$$

이다. $D_t$는 송신 개구 직경이다. 파장이 짧고 개구가 클수록 빔이 좁아져 먼 거리에서도 높은 power density를 유지한다. 동시에 허용 pointing error도 작아진다.

### 4.2 SOC 네트워크의 전개

강의안은 SOC의 발전 방향을 다음과 같이 제시한다.

1. LEO 위성군의 inter-satellite optical link
2. 위성-지상 고용량 링크
3. GEO relay와 데이터 중계 시스템
4. 달 및 화성으로 확장되는 deep-space optical communication

위성 간 레이저 링크는 우주 공간에서 고속 backbone을 구성하므로 흔히 "fiber in the sky"로 표현된다. Starlink, DARPA Blackjack, NASA CLICK와 같은 위성군·초소형 위성 프로젝트가 이러한 링크를 개발해 왔다.

### 4.3 위성-지상 양방향 링크의 필요성

LEO 관측 위성은 통신, 관측, 국방 등에서 데이터를 생성하지만 탑재 저장공간은 유한하다. 생성 데이터율을 $R_g$, 평균 downlink 서비스율을 $R_d(t)$라 하면 저장량 $Q(t)$는

$$
\frac{dQ(t)}{dt}=R_g-R_d(t)
$$

로 생각할 수 있다. 지상국 가시 시간 동안 충분히 큰 $R_d$를 확보하지 못하면 $Q(t)$가 저장 한계에 도달한다. 고용량 optical downlink는 이 병목을 줄이며, uplink는 telecommand, adaptive control, 양방향 채널 추정에 사용된다.

## 5. SOC/FSO 채널과 링크 버짓

### 5.1 링크 버짓의 기본 형태

수신 전력은 송신 전력에 모든 이득과 손실을 곱한 형태로 쓸 수 있다.

$$
P_r=P_t\eta_t\eta_r\eta_{\mathrm{geo}}\eta_{\mathrm{atm}}\eta_{\mathrm{point}}
$$

dB 단위에서는 곱셈이 덧셈으로 바뀐다.

$$
P_r[\mathrm{dBm}]
=P_t[\mathrm{dBm}]
+G_t+G_r
-L_{\mathrm{geo}}-L_{\mathrm{atm}}-L_{\mathrm{point}}-L_{\mathrm{sys}}
$$

여기서 기하 손실은 빔 확산과 수신 개구의 관계를 나타낸다. 거리 $L$에서 Gaussian beam 반경을 $w(L)$, 수신 개구 반경을 $a$라 하면 중심 정렬 시 포착 효율을

$$
\eta_{\mathrm{geo}}\approx 1-\exp\!\left(-\frac{2a^2}{w^2(L)}\right)
$$

로 근사할 수 있다. 장거리에서 $w(L)\gg a$이면

$$
\eta_{\mathrm{geo}}\approx \frac{2a^2}{w^2(L)}
$$

가 되어 빔 폭이 링크 버짓을 직접 지배한다.

### 5.2 대기 감쇠: 흡수와 산란

대기 구성 분자($\mathrm{O_2}$, $\mathrm{CO_2}$, $\mathrm{H_2O}$ 등)는 특정 파장을 흡수하고, 안개·비·눈·구름의 입자는 빛을 산란시킨다. Beer-Lambert 법칙을 이용하면 거리 $L$에서의 transmittance는

$$
T_{\mathrm{atm}}(L)=\exp[-\alpha(\lambda)L]
$$

이고 감쇠는

$$
L_{\mathrm{atm}}[\mathrm{dB}]
=-10\log_{10}T_{\mathrm{atm}}
=4.343\,\alpha(\lambda)L
$$

로 나타난다. 실제 $\alpha$는 파장, 고도, 시간, 기상 상태에 따라 달라진다. 따라서 고정된 평균 감쇠만으로 링크를 설계하면 기상 변화 시 power margin이 부족해질 수 있다.

### 5.3 대기 난류와 scintillation

기온과 기압 변화는 굴절률을 불규칙하게 변화시키며, 수신 광세기가 시간에 따라 요동하는 scintillation을 만든다. 정규화한 수신 세기 $I/\mathbb E[I]$의 변동은 scintillation index

$$
\sigma_I^2
=\frac{\mathbb E[I^2]-\mathbb E[I]^2}{\mathbb E[I]^2}
$$

로 정량화할 수 있다. $\sigma_I^2$가 크면 deep fading의 빈도와 강도가 증가한다. uplink와 downlink는 난류를 통과하는 빔의 크기와 위치가 달라 같은 대기에서도 서로 다른 열화 양상을 보일 수 있다.

### 5.4 Pointing error

빔 중심과 수신기 중심 사이의 횡방향 오차를 $r$라 하면 Gaussian beam의 pointing loss는

$$
\eta_{\mathrm{point}}(r)
\approx \exp\!\left(-\frac{2r^2}{w^2(L)}\right)
$$

로 근사된다. 각도 오차가 $\theta_e$이면 장거리에서 $r\approx L\theta_e$이므로, 동일한 각도 jitter도 거리가 길어질수록 큰 손실이 된다. 좁은 빔은 기하 이득을 높이지만 $w(L)$가 작아 pointing tolerance를 줄인다.

### 5.5 배경광과 수신 잡음

태양과 별, 인공광은 수신기에서 배경광 shot noise를 만든다. PD 전류를 $i_s=RP_r$라 하면 대표적인 잡음 분산은

$$
\sigma_n^2
\approx 2q(I_s+I_{\mathrm{bg}})B
+\sigma_{\mathrm{thermal}}^2
$$

로 쓸 수 있다. $q$는 전자 전하량, $B$는 수신 대역폭이다. 강의안은 optical filter와 전기적 RF filter로 잡음을 분리할 수 있음을 강조한다. 시야각(Field of View)을 줄이는 것도 배경광을 낮추지만 PAT 오차 허용범위를 함께 줄인다.

## 6. SOC/FSO 광 변복조 방식

강의안은 PAM, PPM, DPSK, coherent QAM을 감도와 대역폭 효율 관점에서 비교한다.

### 6.1 PAM과 OOK

PAM은 광세기 레벨로 심볼을 표현한다. OOK는 2-PAM의 특수한 경우다. $M$-PAM의 심볼당 정보량은

$$
k=\log_2 M\quad [\mathrm{bit/symbol}]
$$

이다. 구조가 단순하고 상용 부품을 활용하기 쉽지만, 레벨 수가 증가하면 인접 심볼 간 거리가 줄어 동일 BER을 얻기 위한 SNR이 커진다.

### 6.2 PPM

$M$-PPM은 하나의 심볼 구간을 $M$개 슬롯으로 나누고 펄스 위치로 정보를 표현한다. 심볼당 정보량은 역시 $\log_2M$ bit이며, 낮은 평균 광전력에서 높은 감도를 얻을 수 있다. 그러나 한 심볼이 $M$개 슬롯을 차지하므로 $M$이 커질수록 요구 대역폭과 동기화 부담이 증가한다. 강의안은 4-PPM이 DPSK와 유사한 감도를 갖는 사례를 제시한다.

### 6.3 DPSK

DPSK는 연속 심볼 사이의 위상 차에 정보를 실는다.

$$
\Delta\phi_k=\phi_k-\phi_{k-1}
$$

강의안은 DPSK가 OOK 대비 약 3 dB의 감도 이점을 갖는다고 정리한다. 대신 위상 검출 구조가 필요해 직접검출 OOK보다 수신기가 복잡하다.

### 6.4 Coherent QAM

coherent receiver는 수신광을 local oscillator와 혼합해 동상 성분 $I$와 직교 성분 $Q$를 복원한다.

$$
s_k=I_k+jQ_k
$$

따라서 amplitude와 phase를 모두 이용할 수 있고, 고차 QAM으로 높은 spectral efficiency를 얻는다. 강의안의 비교에서는 OOK가 약 70 photons/bit 수준인 데 비해 QPSK가 약 18 photons/bit 수준의 감도 사례를 보여 준다. 다만 local oscillator와 optical phase-locked loop(OPLL) 등으로 시스템 복잡도가 높다.

#### 선택 기준

- 단순성과 상용성 중심: OOK/PAM
- photon efficiency 중심: PPM
- 위상 감도 향상: DPSK
- 감도와 대역폭 효율을 함께 추구: coherent QAM

어떤 방식도 항상 우월하지 않다. 목표 BER, available optical power, 대역폭, 동기화 정밀도, 탑재체 SWaP-c를 함께 평가해야 한다.

## 7. 광지상국과 CubeSat 링크 설계

### 7.1 광지상국(OGS) 구성

강의안의 광지상국은 다음 블록으로 구성된다.

- Cassegrain telescope
- beacon laser
- coarse/fine PAT optics와 Fast Steering Mirror(FSM)
- 대기 채널 보상 모듈
- 데이터 송수신부: LD, modulator, PD, EDFA
- 대기 상태의 실시간 모니터링

수신 망원경의 개구를 키우면 포착 광전력이 증가하지만 크기와 무게가 커지고, 시야각과 광학 정렬 요구도 달라진다. 설계의 목적은 구성부 간 optical loss를 줄이면서 충분한 link margin을 확보하는 것이다.

링크 마진은

$$
M=P_r-P_{\mathrm{sens}}
$$

로 정의한다. $P_{\mathrm{sens}}$는 목표 BER을 만족하는 수신 감도다. 실제 운용에서는 평균 상태가 아니라 기상 감쇠, pointing jitter, 부품 열화까지 포함한 worst-case 조건에서도 $M>0$이어야 한다.

### 7.2 PAT의 계층 구조

PAT는 세 단계로 이해할 수 있다.

1. **Acquisition**: ephemeris와 자세 정보로 상대 단말이 존재하는 탐색 영역을 찾는다.
2. **Coarse tracking**: gimbal이나 플랫폼 자세 제어로 큰 각도 오차를 제거한다.
3. **Fine tracking**: beacon과 FSM으로 잔여 jitter를 보상한다.

총 pointing error의 분산을 독립 오차 성분의 합으로 보면

$$
\sigma_\theta^2
=\sigma_{\mathrm{ephemeris}}^2
+\sigma_{\mathrm{attitude}}^2
+\sigma_{\mathrm{vibration}}^2
+\sigma_{\mathrm{sensor}}^2
$$

로 예산을 배분할 수 있다. 이 오차가 빔 발산각보다 충분히 작도록 해야 안정적인 링크가 유지된다.

### 7.3 Inter-CubeSat FSO

강의안의 CubeSat 간 FSO 구조는 1,000 km급 LEO 편대비행에서 1 Gb/s 링크를 목표로 하며, 과제를 두 묶음으로 나눈다.

- **PAT**: coarse tracking과 fine tracking
- **Communication**: optical power link budget과 optical Tx/Rx

지상국은 telemetry download와 telecommand upload를 담당한다. 초소형 위성에서는 개구 크기, 레이저 전력, 자세제어기, 열 설계가 모두 제한되므로 통신과 PAT를 별개가 아니라 하나의 link budget으로 공동 최적화해야 한다.

## 8. FSO 수직 모바일 백홀

### 8.1 왜 광섬유만으로 충분하지 않은가

섬, 사막, 산악 지형과 같이 광섬유 포설이 어렵거나, 재난으로 지상망이 단절되었거나, 행사·도심에서 일시적으로 트래픽이 폭증한 환경에서는 유선 backhaul을 빠르게 증설하기 어렵다. FSO는 UAV 또는 high-altitude platform을 이용해 지상 기지국과 core network 사이를 수직·수평 광 링크로 연결한다.

강의안의 수직 모바일 백홀 framework는 다음 목표를 제시한다.

- 수직 최대 통신 거리: 20 km
- 최대 전송 용량: 100 Gb/s
- 지상 cellular coverage: 약 25 km
- UAV-UAV 수평 FSO, core-UAV 수직 FSO, FSO gateway의 결합

WDM을 사용하면 총 데이터율은 채널별 데이터율의 합으로 쓸 수 있다.

$$
R_{\mathrm{total}}=\sum_{k=1}^{N_\lambda}R_k
$$

따라서 하나의 광 링크에서 여러 파장을 병렬 전송해 100 Gb/s급 용량으로 확장할 수 있다. 다만 각 파장의 송수신기와 광 증폭기 대역, 필터, 전체 optical power budget을 함께 관리해야 한다.

### 8.2 FSO 모바일 백홀의 네 가지 기술 과제

1. 대기 채널에 적합한 고효율 광 변조와 충분한 power margin
2. 채널 예측으로 outage를 줄이는 seamless communication
3. UAV를 이용한 optical relay
4. 이동 플랫폼에서도 링크를 유지하는 PAT

## 9. 대기 채널 적응과 outage 완화

### 9.1 고차 PAM의 적응형 threshold

대기 난류는 모든 PAM 레벨을 시간에 따라 함께 확대·축소하는 fading을 만든다. $M$-PAM 수신 모델을

$$
y_k=h_ka_k+n_k
$$

로 두면, 고정 threshold는 $h_k$가 변할 때 최적점을 벗어난다. 채널 또는 레벨 중심을 추정해 threshold $\tau$를 갱신해야 한다. LMS 형태의 갱신식은

$$
\tau_{k+1}=\tau_k+\mu e_kx_k
$$

로 나타낼 수 있다. $e_k$는 결정 오차, $\mu$는 step size다. 큰 $\mu$는 빠른 추종 대신 잡음에 민감하고, 작은 $\mu$는 안정적이지만 빠른 채널 변화를 놓칠 수 있다.

강의안은 SOA의 포화 특성으로 채널 dynamics를 압축하고, LMS 기반 adaptive threshold로 PAM4를 검파하는 접근을 제시한다.

### 9.2 Outage 확률

수신 SNR이 목표 임계값 $\gamma_{\mathrm{th}}$ 아래로 내려가면 outage가 발생한다고 정의할 수 있다.

$$
P_{\mathrm{out}}
=\Pr\{\gamma<\gamma_{\mathrm{th}}\}
$$

평균 SNR만 높이는 것으로는 deep fading에 의한 tail probability를 충분히 낮추지 못할 수 있다. 따라서 채널 예측, power loading, relay diversity가 필요하다.

### 9.3 Reciprocity와 전치 보상

양방향 FSO 채널이 같은 시간과 경로에서 충분히 reciprocal하다면 uplink에서 추정한 채널을 downlink 전송에 활용할 수 있다. 그러나 추정 시각과 실제 전송 시각의 차이 $\Delta t$가 커지면 상관이 감소한다.

$$
\rho(\Delta t)
=\frac{\mathbb E[(h(t)-\bar h)(h(t+\Delta t)-\bar h)]}{\sigma_h^2}
$$

$\rho$가 높을 때는 나쁜 채널 상태를 미리 예측해 송신 전력을 조정할 수 있다. 간단한 channel inversion은

$$
P_t(t)\propto \frac{1}{|\hat h(t)|^2}
$$

이지만, deep fade에서 요구 전력이 발산하므로 실제로는 peak power와 average power 제약을 둔 truncated power loading을 사용해야 한다. 강의안은 시간 지연에 따른 성능 열화, 양방향 reciprocity 검증, EDFA 기반 pre-compensation을 순서대로 제시한다.

### 9.4 Optical relay와 all-optical regeneration

장거리 또는 악천후에서 직접 경로가 불안정할 때 relay는 광 경로를 여러 구간으로 나누고 공간 다양성을 제공한다. 독립적인 두 경로 중 하나라도 성공하면 되는 선택 결합의 outage는

$$
P_{\mathrm{out,div}}
=P_{\mathrm{out},1}P_{\mathrm{out},2}
$$

로 낮아질 수 있다. 실제 경로가 같은 기상권을 공유하면 fading이 상관되므로 이 곱셈식보다 이득이 작아진다.

EDFA나 SOA는 saturation 영역에서 큰 세기 변화를 압축해 scintillation을 완화할 수 있다. 그러나 EDFA는 amplified spontaneous emission(ASE) noise도 추가한다. 단순 증폭만 반복하면

$$
\mathrm{SNR}_{\mathrm{out}}
\not=G\,\mathrm{SNR}_{\mathrm{in}}
$$

이며, gain $G$와 함께 ASE 잡음도 증가한다. all-optical regeneration은 배경광과 ASE를 줄이고 파형을 재생해 다음 홉으로 전달하는 목적을 갖는다.

## 10. FSO 모바일 백홀 성능 향상 전략

강의안은 성능 개선을 세 계층으로 정리한다.

### 10.1 전송 계층

- 높은 수신 감도와 spectral efficiency를 갖는 변복조
- WDM 기반 용량 확장
- 대기 채널에 적응하는 threshold와 DSP
- 배경광 및 증폭기 잡음 완화

### 10.2 광학 구조

- 송수신기 내부 optical loss 최소화
- beam shaping으로 수신 세기 확보
- 광 필터와 제한된 FOV로 배경광 억제
- 충분한 개구와 발산각 설계

### 10.3 PAT와 네트워크 계층

- 링크 단절 시간을 최소화하는 빠른 reacquisition
- narrow beam의 이득을 유지하면서 jitter를 억제하는 fine tracking
- UAV 간 대체 경로와 optical relay
- 채널 예측과 power loading을 통한 proactive outage mitigation

설계 변수 사이에는 분명한 trade-off가 있다. 빔을 좁히면 수신 전력은 늘지만 pointing loss가 증가하고, FOV를 줄이면 배경광은 감소하지만 acquisition이 어려워진다. 변조 차수를 높이면 spectral efficiency는 증가하지만 요구 SNR과 채널 추정 정확도가 높아진다.

## 11. OWC의 전망

강의안은 OWC의 응용을 실내 Li-Fi, 단방향 V2X, 6G 이후의 공중 backhaul, 우주 통신으로 확장한다. 이를 가능하게 하는 연구 과제는 photonics와 communications의 공동 설계다.

### Photonics

- 고속 LED transmitter
- imaging MIMO photodetector
- image sensor bandwidth 향상
- 낮은 optical loss
- beam divergence 최적화

### Communications

- mobility와 handover
- 고차 변조와 DSP
- low latency
- channel robustness algorithm
- WDM, PDM, SDM을 이용한 multidimensional transmission

광원, 광학계, 검출기만 개선해서는 OWC를 완성할 수 없다. 채널 예측, 부호화, 변조, 네트워크 경로 제어가 광학 하드웨어의 제한을 함께 보상해야 한다.

## 핵심 요약

1. OWC는 가시광·적외선의 넓은 자원, 지향성, EMI 회피를 이용해 실내부터 우주까지 통신 범위를 확장한다.
2. Li-Fi는 LED 조명과 PD를 이용하는 IM/DD 시스템이며, LED 대역폭, flicker/dimming, uplink, 셀 간 간섭이 핵심 과제다.
3. VLC 측위는 LED별 RSS로 거리를 추정하고 최소 3개의 신호로 trilateration을 수행한다. 자세 오차와 LED 배치가 위치 정확도를 좌우한다.
4. SOC의 좁은 빔은 높은 link gain과 보안성을 제공하지만 정밀 PAT를 요구한다.
5. 대기 채널은 흡수·산란·난류·pointing error·배경광으로 수신 전력을 열화시키므로 link budget에 시간·고도·기상을 포함해야 한다.
6. PAM, PPM, DPSK, coherent QAM은 각각 단순성, 감도, 위상 이득, spectral efficiency에서 서로 다른 장점을 갖는다.
7. FSO 모바일 백홀은 약 20 km 수직 링크와 100 Gb/s급 용량을 목표로 재난망, 오지, 일시적 트래픽 밀집 지역에 빠른 네트워크 확장을 제공한다.
8. 안정적인 FSO 운용에는 adaptive detection, reciprocity 기반 전치 보상, relay/regeneration, seamless PAT가 함께 필요하다.
