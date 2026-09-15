---
layout: post
title: "광통신공학 10강: 아날로그 광전송과 Radio over Fiber"
date: 2026-09-16 03:10:00 +0900
categories: [optical-communication]
tags:
  - radio-over-fiber
  - subcarrier-multiplexing
  - spurious-free-dynamic-range
  - optical-single-sideband
math: true
---

## 10강. 아날로그 광전송과 Radio over Fiber

아날로그 광전송은 디지털 비트를 복원하는 대신, RF 파형의 진폭과 위상을 광섬유를 통해 가능한 한 그대로 전달한다. 대표적인 구조가 **SCM(Sub-Carrier Multiplexing)** 과 **RoF(Radio over Fiber)** 이다. 광파는 장거리 전송을 담당하는 주 반송파(main carrier), 각 서비스의 RF 신호는 부반송파(subcarrier)가 된다.

이 강의의 흐름은 다음과 같다.

1. SCM과 RoF의 구조 및 응용
2. 아날로그 광변조에서 변조지수와 선형성이 중요한 이유
3. 비선형 왜곡을 CSO, CTB, IMD3, SFDR로 평가하는 방법
4. 전치왜곡, 피드포워드, dual-parallel EML을 이용한 선형화
5. DSB 광신호의 색분산 페이딩과 OSSB 변조에 의한 완화

### 1. 아날로그 SCM 광링크

#### 1.1 SCM의 정의와 신호 흐름

$N$개의 RF 채널을 주파수축에서 서로 겹치지 않도록 배치하면 전기적 합성 신호를

$$
v_{\mathrm{SCM}}(t)
=\sum_{k=1}^{N} A_k s_k(t)
\cos\!\left(2\pi f_{s,k}t+\phi_k\right)
$$

로 나타낼 수 있다. 여기서 $s_k(t)$는 $k$번째 채널의 베이스밴드 정보, $f_{s,k}$는 RF 부반송파 주파수이다. 전력 결합기(power combiner)가 이 채널들을 하나로 합치고, 합성 RF 신호가 레이저 또는 외부 광변조기를 구동한다. 작은 신호 영역의 광세기는

$$
P_t(t)=\bar P_t
\left[1+\sum_{k=1}^{N}m_k s_k(t)
\cos\!\left(2\pi f_{s,k}t+\phi_k\right)\right]
$$

처럼 쓸 수 있다. $\bar P_t$는 평균 송신 광전력, $m_k$는 각 채널의 광변조지수이다. 수신단에서는 포토다이오드가 광세기를 전류로 바꾸고, RF 채널 필터와 국부발진기(local oscillator)가 원하는 채널을 선택해 복조한다.

직접검파 포토다이오드의 responsivity를 $\mathcal R$이라 하면

$$
i(t)=\mathcal R P_r(t)
$$

이므로 광세기에 실린 RF 파형이 전류 영역에서 그대로 복원된다. SCM의 핵심은 주파수 선택과 채널 합성·분리를 성숙한 마이크로파 회로로 처리한다는 점이다. 광학 소자보다 안정적인 RF 발진기와 선택도가 높은 RF 필터를 활용할 수 있어 비용 측면에서 유리하다.

#### 1.2 TDM, WDM, SCM의 비교

- **TDM(Time Division Multiplexing)**: 시간 슬롯을 정밀하게 맞춰야 하며, 장거리 링크에서는 잡음 누적, 색분산, 비선형 누화, 편광모드분산의 영향을 함께 관리해야 한다. 따라서 정밀한 분산 보상과 링크 엔지니어링이 필요하다.
- **WDM(Wavelength Division Multiplexing)**: 광원 파장 안정도와 광필터 선택도가 채널 간격을 제한한다. 강의안은 당시 WDM 시스템의 최소 채널 간격으로 50 GHz를 제시한다.
- **SCM**: 채널 배치와 선택을 RF 영역에서 수행한다. 대신 충분한 RF 이득과 넓은 SFDR(Spurious-Free Dynamic Range)을 확보해야 한다.

세 방식은 서로 배타적이지 않다. 예를 들어 하나의 파장 안에 여러 RF 부반송파를 실은 뒤, 여러 파장을 WDM으로 묶는 계층적 다중화도 가능하다. 이때 각 계층의 제약이 누적되므로 광전력 예산뿐 아니라 RF 선형성과 채널 간 왜곡까지 함께 설계해야 한다.

#### 1.3 광섬유 링크가 RF 전송에 적합한 이유

강의안은 광섬유와 동축 케이블을 다음과 같이 비교한다.

| 항목 | 광섬유 | 동축 케이블 |
|---|---:|---:|
| 직경 | 약 0.2 mm | 약 5 mm |
| 무게 | 약 0.073 g/m | 약 30-100 g/m |
| 손실 | 약 0.2-0.4 dB/km | 약 1000-1500 dB/km |

광섬유는 전자기 간섭에 강하고 대역폭이 넓으며 가볍다. 특히 RF 신호를 별도의 주파수 변환이나 복잡한 신호 처리 없이 원격 안테나까지 직접 운반할 수 있다. 그 결과 원격 기지국의 장치를 작게 만들 수 있고, 전송 형식이나 프로토콜이 달라도 같은 광 인프라를 공유하기 쉽다.

### 2. SCM과 RoF의 응용

#### 2.1 응용별 요구 조건

SCM/RoF의 공통 구조는 같지만 서비스에 따라 중심 주파수와 설계 목표가 달라진다.

| 응용 | 강의안의 주파수 예 | 주요 요구 조건 |
|---|---|---|
| CATV | 상향 5.75-41.75 MHz, 방송 채널 대역 54-450 MHz | 약 75 dB CNR 또는 SFDR, 넓은 동적 범위, 낮은 왜곡, RF 이득 |
| 이동통신 중계 | 800 MHz cellular, 1.8 GHz PCS, 2 GHz IMT-2000, 실험실 기반 35-40 GHz fiber-radio | 저비용 기지국, 넓은 동적 범위, 낮은 왜곡, RF 이득 |
| 광가입자망 | 시스템 의존 | 서비스 구조에 맞춘 성능 정의 |
| ITS | 5.8 GHz 요금징수, 36-37 GHz 도로-차량, 59.4-60 GHz 차량 간, 77 GHz 차량 레이더 | RF 이득, 채널 간섭 억제, 낮은 왜곡, 빠른 핸드오프, 높은 안정도 |

아날로그 링크에서 요구 CNR 또는 SFDR가 큰 이유는 광링크가 여러 RF 채널을 동시에 전달하기 때문이다. 한 채널의 왜곡 성분이 다른 채널의 대역에 들어오면 수신 필터만으로 제거할 수 없다.

#### 2.2 RoF의 기본 구조

RoF 시스템은 중앙국(CS, Control/Central Station)에서 RF 신호를 광신호로 변환해 광섬유로 기지국(BS)에 보내고, 기지국에서 광전 변환 후 안테나로 방사한다. 역방향도 같은 원리로 구성한다.

```text
중앙국(CS) -> E/O 변환 -> 광섬유 -> O/E 변환 -> RF 송수신기/안테나
```

신호 라우팅과 처리를 중앙국에 집중하면 기지국은 광전 변환과 RF 송수신 중심의 단순한 구조가 된다. 광섬유의 낮은 손실, 넓은 대역폭, EMI 내성이 중앙국과 다수 기지국 사이에 안정적인 연결을 제공한다.

$\mu$-wave/mm-wave 무선 신호는 주파수가 높을수록 대기 및 전파 경로 손실이 커진다. 이는 셀 반경을 줄이지만, 동시에 같은 주파수를 더 가까운 거리에서 재사용할 수 있게 한다. RoF는 넓은 대역의 밀집 셀을 중앙에서 제어하는 데 적합하다.

#### 2.3 CATV, WDM-PON, fiber-radio, DAS

- **CATV**: headend에서 curb 또는 hub까지는 광으로 분배하고, 가입자 구간은 전기적으로 분배할 수 있다. 방송 서비스뿐 아니라 가입자와 hub/headend 사이의 양방향 burst 데이터도 지원한다.
- **WDM-PON 방송 비디오**: 라우터 기반 망에서 비냉각 LED 같은 광대역 광송신기로 방송 신호를 분배할 수 있다. RF SCM을 함께 사용하면 상향 채널을 분리해 충돌을 피하는 구조를 만들 수 있다.
- **fiber-radio access**: 중앙국과 기지국을 광으로 연결하고, 기지국에서 microwave/mm-wave 신호를 무선 단말에 전달한다.
- **DAS(Distributed Antenna System)**: 건물이나 넓은 구역에 안테나를 분산 배치한다. 아날로그 DAS는 RF 파형을 광으로 직접 운반하며, 디지털 DAS는 기저대역 또는 디지털 무선 인터페이스를 전달한 뒤 원격 무선 헤드에서 RF를 만든다.

### 3. 아날로그 변조의 핵심: 선형성과 변조지수

#### 3.1 디지털 변조와 아날로그 변조의 차이

디지털 광링크에서는 광의 on/off 상태를 확실히 구분하기 위해 높은 소광비(extinction ratio)가 중요하다. 반면 아날로그 링크에서는 입력 RF 파형의 미세한 진폭 변화까지 보존해야 하므로 전달함수의 **선형성**이 핵심이다.

레이저의 광출력-주입전류 특성을 $P(I)$라 하면 바이어스 전류 $I_b$ 주위에서

$$
P(I_b+i_{ac})
=P(I_b)
+\left.\frac{dP}{dI}\right|_{I_b}i_{ac}
+\frac{1}{2}\left.\frac{d^2P}{dI^2}\right|_{I_b}i_{ac}^2
+\frac{1}{6}\left.\frac{d^3P}{dI^3}\right|_{I_b}i_{ac}^3+\cdots
$$

로 전개할 수 있다. 첫 번째 미분항만 남으면 이상적인 선형 변조이지만, 실제 소자에서는 2차 이상 항이 새로운 주파수 성분을 만든다.

#### 3.2 전기적·광학적 변조지수

레이저의 문턱전류를 $I_{th}$, 바이어스 전류를 $I_b$, RF 전류의 첨두값을 $I_{ac}$라 하면 전기적 변조지수는

$$
m_e=\frac{I_{ac}}{I_b-I_{th}}
$$

이다. 평균 또는 정상상태 광출력을 $P_s$, RF 변조 시 첨두 광출력을 $P_{peak}$라 하면 광학적 변조지수는

$$
m_o=\frac{P_{peak}-P_s}{P_s}=\frac{B}{A}
$$

이다. 강의안의 그림에서 $A=P_s$, $B=P_{peak}-P_s$에 해당한다. 문턱 위의 $P-I$ 곡선이 완전히 선형이면 $m_e$와 $m_o$가 거의 같지만, 실제 레이저에서는 기울기 변화와 clipping 때문에 달라질 수 있다.

변조지수를 키우면 검출 RF 전력이 증가한다. 수신 평균 광전력을 $\bar P_r$, 부하를 $R_L$이라 하고 정현파의 첨두 전류를 $i_{ac}=\mathcal R\bar P_r m_o$라 두면

$$
P_{\mathrm{RF}}
=\frac{i_{ac}^2R_L}{2}
=\frac{\left(\mathcal R\bar P_r m_o\right)^2R_L}{2}.
$$

즉 작은 신호 영역에서는 $P_{\mathrm{RF}}\propto m_o^2$이다. 그러나 $m_o$를 과도하게 키우면 저출력 쪽에서 문턱 아래로 내려가거나 전달곡선의 비선형 영역에 들어가 왜곡이 급증한다. 따라서 변조지수는 RF 이득과 왜곡 사이의 핵심 설계 변수이다.

### 4. 비선형 왜곡과 두 톤 시험

#### 4.1 비선형성이 새로운 주파수를 만드는 과정

두 개의 RF 톤을

$$
x(t)=A_1\cos\omega_1t+A_2\cos\omega_2t
$$

라 하고 광송신기와 수신기를 포함한 링크의 전달함수를

$$
y(t)=a_0+a_1x(t)+a_2x^2(t)+a_3x^3(t)+\cdots
$$

로 근사하자. $a_2x^2(t)$에서는

$$
2\omega_1,\quad 2\omega_2,\quad \omega_1+\omega_2,\quad |\omega_1-\omega_2|
$$

성분이 생긴다. $a_3x^3(t)$에서는

$$
3\omega_1,\quad 3\omega_2,\quad
2\omega_1\pm\omega_2,\quad
2\omega_2\pm\omega_1
$$

등이 생긴다. 특히 $2\omega_1-\omega_2$와 $2\omega_2-\omega_1$은 두 원래 톤 가까이에 나타나므로 대역통과필터로 분리하기 어렵다. 이 성분을 3차 상호변조왜곡(IMD3)이라 한다.

입력 RF 전력을 1 dB 높일 때 기본파 출력은 이상적으로 1 dB 증가하지만 IMD3는 3 dB 증가한다. 따라서 입력을 높일수록 기본파보다 IMD3가 훨씬 빠르게 커지며, 높은 RF 이득만 추구해서는 좋은 아날로그 링크를 만들 수 없다.

#### 4.2 CSO와 CTB

다채널 시스템에서는 여러 조합의 왜곡 성분이 같은 주파수에 겹친다. 이를 합성 왜곡(composite distortion)으로 평가한다.

$$
\mathrm{CSO}
=\frac{P_{\mathrm{carrier}}}
{\sum P_{\mathrm{2nd\text{-}order\ products\ at\ CSO\ frequency}}}
$$

$$
\mathrm{CTB}
=\frac{P_{\mathrm{carrier}}}
{\sum P_{\mathrm{3rd\text{-}order\ products\ at\ CTB\ frequency}}}.
$$

dB 단위에서는 각각

$$
\mathrm{CSO}_{\mathrm{dB}}
=P_{c,\mathrm{dBm}}-P_{\mathrm{CSO,tot,dBm}},
\qquad
\mathrm{CTB}_{\mathrm{dB}}
=P_{c,\mathrm{dBm}}-P_{\mathrm{CTB,tot,dBm}}
$$

로 표현한다. 강의안은 CSO를 broadband 지표, CTB를 narrowband 지표로 구분한다. 값이 클수록 반송파에 비해 합성 왜곡이 작다는 뜻이다.

#### 4.3 잡음과 SFDR

수신 잡음에는 shot noise, 열잡음, 레이저의 RIN(Relative Intensity Noise) 등이 포함된다. 대역폭 $B$에서 대표적인 전류 잡음 분산은

$$
\overline{i_n^2}
=\left[2q(I_p+I_d)
+\frac{4kT}{R_L}
+\mathit{RIN}\cdot I_p^2\right]B
$$

로 쓸 수 있다. 여기서 $I_p=\mathcal R\bar P_r$는 평균 광전류, $I_d$는 암전류이다. 따라서 한 RF 채널의 CNR은

$$
\mathrm{CNR}=\frac{P_{\mathrm{RF}}}{P_n}
$$

으로 정의된다.

**SFDR**은 기본파가 잡음 바닥보다 충분히 크면서, 가장 큰 spurious 성분도 아직 잡음 바닥 아래에 있는 입력 또는 출력 범위이다. IMD3가 제한 요인이고 출력 3차 교차점이 $OIP_3$, 해당 측정 대역폭에서 잡음 바닥이 $N$이면

$$
\mathrm{SFDR}_{\mathrm{dB}}
=\frac{2}{3}\left(OIP_3-N\right).
$$

잡음 밀도 $N_0$를 dBm/Hz로 사용하면 SFDR는 보통 $\mathrm{dB\,Hz^{2/3}}$로 표시한다. 서로 다른 자료의 SFDR를 비교할 때는 측정 대역폭과 정규화 여부를 반드시 함께 확인해야 한다.

LDR(Linear Dynamic Range)은 출력이 잡음에서 벗어나 선형적으로 증가하는 구간을 뜻한다. SFDR는 여기에 spurious 성분의 조건까지 포함하므로 아날로그 다채널 링크에서 더 직접적인 지표가 된다.

### 5. 선형화 기법

#### 5.1 전기적 전치왜곡

전치왜곡(predistortion)은 광변조기의 비선형성과 반대 부호의 비선형성을 입력에 미리 더하는 방법이다. 변조기의 전달함수를

$$
f(x)=a_1x+a_2x^2+a_3x^3+\cdots
$$

라 하면 전치왜곡기 $g(x)$를 설계해

$$
f(g(x))\approx Gx
$$

가 되도록 만든다. 구조는 간단하지만 주파수가 높아질수록 전치왜곡 회로의 진폭·위상 응답을 넓은 대역에서 정확히 맞추기 어렵다.

#### 5.2 피드포워드 보상

피드포워드 구조에서는 입력 $S$를 두 경로로 나눈다. 광변조기 1의 출력이 $S+\Delta$라면 일부를 검출해 지연된 기준신호 $S$와 비교함으로써 왜곡 $\Delta$를 추출한다. 이 왜곡을 반전해 $-\Delta$를 만들고 두 번째 광변조기로 보상한 뒤, 지연된 주 신호와 합치면

$$
(S+\Delta)+(-\Delta)=S
$$

가 된다. 실제 성능은 두 경로의 시간 지연과 진폭·위상 응답이 얼마나 정확히 일치하는지에 달려 있다. 두 변조기의 전달특성도 같아야 넓은 주파수 범위에서 상쇄가 유지된다.

### 6. EML 비선형성과 dual-parallel 보상

#### 6.1 단일 EML의 비선형성

EML(Electro-absorption Modulated Laser)은 DFB-LD와 EAM(Electro-Absorption Modulator)을 한 모듈에 결합한다. EAM은 역바이어스 전압에 따라 흡수가 변하지만, 전기장에 대한 흡수 변화가 비대칭이고 DC 바이어스와 접합 전기장 사이도 비선형이다.

EAM 투과 광전력을 바이어스 $V_b$ 주위에서 전개하면

$$
P(V_b+v)
=P(V_b)+P'(V_b)v
+\frac{P''(V_b)}{2}v^2
+\frac{P'''(V_b)}{6}v^3+\cdots
$$

이다. $P'(V_b)$는 원하는 기본파의 변환 효율을, $P''(V_b)$는 주로 2차 왜곡과 CSO를, $P'''(V_b)$는 3차 왜곡과 CTB/IMD3를 결정한다. 강의안의 측정 결과에서도 DC 바이어스에 따른 CSO·CTB 변화가 전달곡선의 1차, 2차, 3차 미분 변화와 연결된다. 그러므로 최대 광출력 지점이 반드시 최적 바이어스는 아니다. 필요한 기본파 이득을 유지하면서 관련 고차 미분이 작은 지점을 선택해야 한다.

#### 6.2 Dual-parallel EML의 원리

두 EML을 병렬로 구동하고 각각 다른 DC 바이어스 $V_{b1}$, $V_{b2}$를 인가하면 두 경로의 비선형 계수를 서로 다르게 만들 수 있다. 결합 전기장을 $E_1$, $E_2$라 하면 포토다이오드에 입사하는 광전력은 결합 조건에 따라

$$
P_{out}=\left|E_1+E_2\right|^2
$$

로 나타난다. 각 경로의 원하는 기본파는 같은 방향으로 더하고 IMD 성분은 반대 위상으로 더하도록 바이어스, 광결합비, RF 구동비를 조절한다. 3차 항을 예로 들면 설계 목표는

$$
a_{3,1}G_1^3+a_{3,2}G_2^3\approx 0,
\qquad
a_{1,1}G_1+a_{1,2}G_2\neq 0
$$

이다. 즉 기본파는 보존하면서 IMD3만 상쇄한다.

#### 6.3 실험 결과의 해석

강의안의 two-tone 시험은 main EML을 1.2 V, sub EML을 0.6 V로 바이어스한다.

- 단일 EML에서는 반송파와 IMD3 간 차이가 약 25 dBc이다.
- Dual EML에서는 그 차이가 약 48 dBc로 증가한다.
- 따라서 IMD3가 23 dB 감소한다.
- 2-8 GHz 범위의 측정에서 fundamental, IMD3, IMD5가 비교적 일정한 추세를 보여 동작 주파수에 둔감한 보상 특성을 제시한다. 측정 채널 간격은 0.01 GHz이다.

동적 범위 슬라이드는 개선량을 19.6 dB로 명시한다. 한편 같은 그래프 안에는 단일 구조 94.9 dB, dual 구조 110 dB라는 표기도 있어 단순 차이는 15.1 dB이다. 두 값의 차이는 동적 범위 정의나 판독 기준을 원 논문에서 확인해야 하므로, 강의안만으로 임의 해석해서는 안 된다. 해당 dual EML 결과의 출처로 강의안은 *IEEE Photonics Technology Letters*, vol. 14, no. 4, pp. 462-464, Apr. 2002를 제시한다.

### 7. DSB 광세기 변조와 색분산 페이딩

#### 7.1 DSB 스펙트럼

RF 각주파수를 $\Omega=2\pi f$라 하자. 광반송파 $\omega_0$를 RF로 세기변조하면 반송파 양쪽에 $\omega_0-\Omega$와 $\omega_0+\Omega$ 성분이 생긴다. 여러 SCM 채널을 변조하면 각 $f_{s,k}$마다 한 쌍의 측파대가 생성된다.

작은 신호에서 chirp가 없는 DSB 광전계는

$$
E(0,t)\approx E_0e^{j\omega_0t}
\left[1+\frac{m}{2}e^{j\Omega t}
+\frac{m}{2}e^{-j\Omega t}\right]
$$

로 쓸 수 있다. 포토다이오드에서는 광반송파와 상측파대의 beating, 광반송파와 하측파대의 beating이 모두 같은 RF 주파수 $\Omega$를 만든다.

#### 7.2 색분산에 의한 RF carrier suppression

광섬유 전파상수를 광반송파 주위에서

$$
\beta(\omega_0\pm\Omega)
\approx\beta_0\pm\beta_1\Omega
+\frac{1}{2}\beta_2\Omega^2
$$

로 전개할 수 있다. $\beta_1$ 항은 공통적인 군지연을 만들고, $\beta_2$ 항은 두 beating 경로 사이에 상대위상을 만든다. 두 RF 성분을 합하면 chirp-free DSB 링크의 주파수응답은

$$
H_{\mathrm{RF}}(f,L)
\propto
\cos\!\left(\frac{\beta_2L(2\pi f)^2}{2}\right)
$$

가 된다. 분산 파라미터 $D$와 $\beta_2$의 관계

$$
D=-\frac{2\pi c}{\lambda^2}\beta_2
$$

를 사용하면 크기 응답은

$$
\left|H_{\mathrm{RF}}(f,L)\right|
\propto
\left|\cos\!\left(\frac{\pi D\lambda^2Lf^2}{c}\right)\right|
$$

로 쓸 수 있다. 따라서 RF 출력은 주파수 또는 광섬유 길이에 따라 주기적으로 감소하며, null은

$$
f_n=
\sqrt{\frac{(2n+1)c}{2|D|\lambda^2L}},
\qquad n=0,1,2,\ldots
$$

에서 발생한다. 이 현상이 DICS(Dispersion-Induced RF Carrier Suppression)이다. 고주파 RF일수록 $f^2$에 비례해 분산 위상이 빠르게 누적되므로 mm-wave RoF 링크에서 특히 중요하다.

설계 시에는 다음 항목을 함께 확인해야 한다.

- 목표 RF 대역 안에 첫 번째 dispersion null이 들어오는가?
- 최대 광섬유 길이에서 RF 전달함수의 ripple이 허용 범위 안에 있는가?
- 광원의 chirp와 변조기의 실제 측파대 진폭·위상이 이상적인 DSB 가정에서 얼마나 벗어나는가?

### 8. OSSB 변조로 분산 페이딩 완화

#### 8.1 동작 원리

OSSB(Optical Single Sideband) 변조는 광반송파와 한쪽 측파대만 전송한다. 전계를

$$
E_{\mathrm{OSSB}}(0,t)
=E_0e^{j\omega_0t}
+E_+e^{j(\omega_0+\Omega)t}
$$

로 두면 직접검파에서 RF를 만드는 주된 항은

$$
i_{\Omega}(t)
\propto 2\Re\!\left\{E_+E_0^*
e^{j[\Omega t-\Delta\beta L]}\right\}
$$

하나뿐이다. DSB처럼 상·하측파대의 두 beating 성분이 서로 상쇄할 수 없으므로 색분산은 주로 RF 위상을 회전시키며, 이상적인 경우 RF 진폭의 주기적 null을 만들지 않는다.

강의안은 MZM 기반 OSSB 구조와 함께, EML의 SPM(Self-Phase Modulation)과 XPM(Cross-Phase Modulation)을 이용한 측파대 제어 결과를 제시한다. RF 위상 detuning으로 XPM의 양을 바꾸면 상·하측파대의 크기 차이를 조절할 수 있다. 이 결과의 출처로 *IEEE Photonics Technology Letters*, vol. 16, no. 5, pp. 1373-1375, May 2004가 제시되어 있다.

#### 8.2 측정 결과의 의미

광섬유 길이를 약 10-100 km로 바꾼 측정에서 DSB 신호의 수신 RF 전력은 특정 길이에서 깊게 감소한 뒤 회복한다. 이는 두 측파대 beating 성분의 상대위상이 길이에 따라 변한다는 이론과 일치한다. OSSB 신호는 같은 길이 범위에서 수신 RF 전력이 비교적 일정하게 유지되어 DICS가 완화됨을 보여준다.

OSSB의 설계 목표는 단순히 한 측파대를 약하게 만드는 데 그치지 않는다. 원하는 RF 대역 전체에서 충분한 sideband suppression ratio를 유지하고, 광반송파는 직접검파에 필요한 beating 기준으로 남기며, 변조기 바이어스와 RF 위상이 온도 및 주파수 변화에도 안정적이어야 한다.

### 9. 설계 관점에서 연결해서 보기

아날로그 광링크의 성능은 하나의 지표로 결정되지 않는다.

1. **RF 이득**: 변조지수, 평균 광전력, 변조 효율, 포토다이오드 responsivity를 높이면 개선된다.
2. **잡음**: 수신 광전력을 높이면 shot noise와 RIN도 함께 증가한다. 특히 높은 광전력에서는 RIN 항이 지배적일 수 있다.
3. **선형성**: 변조지수와 RF 입력을 높이면 기본파 출력이 커지지만 CSO, CTB, IMD3가 더 빠르게 증가한다.
4. **동적 범위**: 잡음 바닥을 낮추는 것과 비선형 교차점을 높이는 것을 동시에 해야 SFDR가 넓어진다.
5. **분산**: DSB 링크에서는 주파수와 길이에 따라 RF null이 생길 수 있다. 고주파 또는 장거리 링크에서는 OSSB와 같은 분산 내성 변조가 필요하다.
6. **운용 안정성**: 전치왜곡, 피드포워드, dual-parallel EML, OSSB 모두 바이어스와 위상 정합에 의존한다. 초기 성능뿐 아니라 온도와 시간 변화에 대한 제어가 필요하다.

즉 RoF 링크 설계는 다음 최적화 문제로 요약할 수 있다.

$$
\max\;\{G_{\mathrm{RF}},\;\mathrm{CNR},\;\mathrm{SFDR}\}
$$

subject to

$$
\begin{aligned}
&P_{\mathrm{opt}}\le P_{\mathrm{device\ limit}},\\
&m_o\le m_{\mathrm{linear\ region}},\\
&\mathrm{CSO},\mathrm{CTB},\mathrm{IMD3}\le \mathrm{allowed\ distortion},\\
&|H_{\mathrm{RF}}(f,L)|\ge H_{\min}
\quad \text{for the target RF band}.
\end{aligned}
$$

### 핵심 요약

- SCM은 여러 RF 부반송파를 전기적으로 합성해 하나의 광반송파에 싣고, 수신단의 RF 필터와 발진기로 채널을 분리한다.
- RoF는 신호 처리와 라우팅을 중앙국에 집중하고 기지국을 단순화한다. CATV, 이동통신 중계, WDM-PON 방송, ITS, fiber-radio, DAS에 적용할 수 있다.
- 디지털 광링크가 소광비를 중시한다면 아날로그 광링크는 전달함수의 선형성을 중시한다. 변조지수는 RF 이득과 비선형 왜곡의 절충을 결정한다.
- 두 톤 입력에서 2차와 3차 비선형은 합·차 주파수와 IMD3를 만든다. 다채널 링크에서는 CSO와 CTB, 시스템 동적 범위에서는 SFDR가 핵심 지표이다.
- 전치왜곡과 피드포워드는 비선형 성분을 반대 부호로 만들어 상쇄한다. Dual-parallel EML은 두 EML의 서로 다른 바이어스점을 이용해 기본파를 유지하면서 IMD3를 줄인다.
- DSB 광세기 변조는 색분산으로 두 측파대 beating 성분의 위상차가 생겨 RF power fading과 null을 만든다.
- OSSB는 한쪽 측파대만 사용해 두 beating 경로의 상쇄를 없애므로 DICS를 크게 완화한다.
