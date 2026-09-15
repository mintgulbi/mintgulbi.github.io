---
layout: post
title: "광통신공학 2강: 광파의 전파, 분산, 공진과 유전체 경계"
date: 2026-09-16 03:02:00 +0900
categories: [optical-communication]
tags:
  - electromagnetic-wave
  - dispersion
  - polarization
  - total-internal-reflection
math: true
---

## Lecture 02. 광파의 전파, 분산, 공진과 유전체 경계

이번 강의는 빛을 전자기파로 표현하는 데서 시작한다. 먼저 파동의 위상과 매질 안에서의 파장 변화를 정의하고, 실제 전송에서 나타나는 감쇠와 분산을 다룬다. 이어서 편광을 도입한 뒤, 레이저를 광 공진기로 해석하고, 마지막으로 유전체 경계의 반사와 전반사, 소멸장(evanescent field)까지 연결한다.

### 1. 빛을 전자기파로 표현하기

#### 1.1 진행파와 전파 상수

$z$ 방향으로 진행하는 단색 전기장은 다음과 같이 쓸 수 있다.

$$
E(z,t)=E_0\sin(\omega t-kz)
$$

여기서

- $E_0$: 전기장 진폭
- $\omega=2\pi f$: 각주파수
- $\lambda$: 파장
- $1/\lambda$: 파수(wave number)
- $k$: 전파 상수(propagation constant)
- $v_p$: 위상 속도

이다. 위상이 같은 점은 $\omega t-kz=\text{constant}$를 만족하므로, 위상 속도는

$$
v_p=\frac{\omega}{k}
$$

이고 전파 상수는

$$
k=\frac{\omega}{v_p}
$$

로 정의된다. 자유 공간에서는 $v_p=c$이고 파장을 $\lambda_0$라고 쓰면

$$
k=\frac{\omega}{c}=\frac{2\pi}{\lambda_0}
$$

이다.

평면파(plane wave)는 전파 방향에 수직인 한 평면 위에서 위상이 모두 같은 파동이다. 강의안은 이를 자유 공간에서 퍼져 나가는 구면파(spherical wave)와 대비해 소개한다.

#### 1.2 매질 안에서의 속도와 파장

굴절률 $n$인 매질에서는 위상 속도가

$$
v_p=\frac{c}{n}
$$

으로 감소한다. 비자성 매질을 전제로 하면 강의안의 굴절률은

$$
n=\sqrt{\varepsilon_r}
$$

로 표현된다. 주파수와 파장 사이에는

$$
f\lambda=v_p=\frac{c}{n}
$$

의 관계가 있으므로, 매질 안의 파장은

$$
\lambda=\frac{v_p}{f}
       =\frac{c}{nf}
       =\frac{\lambda_0}{n}
$$

이 된다. 경계를 지나도 주파수 $f$는 그대로인 반면, 속도가 $c/n$으로 바뀌기 때문에 파장이 $1/n$로 짧아진다는 것이 핵심이다.

### 2. 전파 손실과 분산

#### 2.1 파동 감쇠

손실이 있는 매질에서 진행하는 전기장은 감쇠 계수 $\alpha$를 사용해

$$
E(z,t)=E_0e^{-\alpha z}\sin(\omega t-kz)
$$

로 나타낸다. 강의안에서 $\alpha$의 단위는 $\mathrm{cm^{-1}}$이다. 전기장 진폭이 $e^{-\alpha z}$에 비례하고 전력은 전기장 크기의 제곱에 비례하므로, 길이 $L$을 전파한 뒤의 전력비는

$$
\frac{P_{\mathrm{out}}}{P_{\mathrm{in}}}
=e^{-2\alpha L}
$$

이다. 이를 dB로 표시하면

$$
10\log_{10}\left(\frac{P_{\mathrm{out}}}{P_{\mathrm{in}}}\right)
=10\log_{10}\left(e^{-2\alpha L}\right)
=-20\alpha L\log_{10}e
$$

가 된다. 위 값은 출력 대 입력의 비이므로 음수이다. 감소량의 크기를 양의 손실로 표시하면

$$
A_{\mathrm{dB}}
=10\log_{10}\left(\frac{P_{\mathrm{in}}}{P_{\mathrm{out}}}\right)
=20\alpha L\log_{10}e
$$

로 쓸 수 있다. 즉, 전기장 진폭은 $e^{-\alpha L}$, 전력은 $e^{-2\alpha L}$로 감소한다.

#### 2.2 분산의 출발점

광원에는 하나의 파장만 존재하는 것이 아니라 유한한 파장 폭 $\Delta\lambda$가 존재한다. 강의안은 전자기파에 왜 주파수 폭 $\Delta f$가 존재하는지 질문을 던진 뒤, 광섬유 전송에서 고려할 분산을 다음 두 가지로 제시한다.

1. 재료 분산(material dispersion): 굴절률이 파장에 따라 달라지는 $n(\lambda)$의 효과
2. 도파로 분산(waveguide dispersion): 도파 구조가 파장별 전파 특성에 미치는 효과

분산이 존재하면 입력 펄스를 이루는 $\lambda_1$, $\lambda_2$, $\lambda_3$ 성분이 서로 다른 지연을 겪는다. 수신점에서 이 성분들을 다시 합하면 도착 펄스의 폭이 넓어지고 최고 전력이 낮아진다.

$$
\text{분산}
\quad\Longrightarrow\quad
\text{펄스 확장}+\text{첨두 전력 감소}
\quad\Longrightarrow\quad
\text{수신 데이터 복원 난이도 증가}
$$

감쇠가 신호 전체의 크기를 줄이는 현상이라면, 분산은 서로 다른 파장 성분의 도착 시간을 벌려 펄스 모양을 변형하는 현상이다.

#### 2.3 분산 매질에서의 시간 지연

$\tau(\lambda)$를 길이 $L$인 경로를 펄스가 통과하는 데 걸리는 시간이라고 하자. 그러면

$$
\frac{\tau(\lambda)}{L}
$$

은 단위 길이당 지연이다. 두 파장 $\lambda_1$, $\lambda_2$ 사이의 단위 길이당 지연 차는

$$
\Delta\left(\frac{\tau}{L}\right)
=\left(\frac{\tau}{L}\right)_2
-\left(\frac{\tau}{L}\right)_1
$$

이다. 강의안은 분산 계수 $M$을 사용해 이를

$$
\Delta\left(\frac{\tau}{L}\right)=-M\Delta\lambda
$$

로 나타낸다. $M$의 단위는

$$
\mathrm{ps/(nm\cdot km)}
$$

이다. 따라서 실제 길이 $L$에서 두 파장 성분 사이에 생기는 시간 차는

$$
\Delta\tau=-ML\Delta\lambda
$$

이며, 펄스 확장의 크기는

$$
|\Delta\tau|=|M|L|\Delta\lambda|
$$

로 해석할 수 있다.

강의안의 부호 규약에서 $\Delta\lambda>0$이고 $M>0$이면

$$
\left(\frac{\tau}{L}\right)_2
<
\left(\frac{\tau}{L}\right)_1
$$

이므로 더 짧은 파장 성분의 지연이 더 길다. 결국 $M$, 전송 거리 $L$, 광원의 파장 폭 $\Delta\lambda$가 커질수록 파장 성분의 도착 시간 차가 커지고, 이는 전송 용량을 제한하는 주요 원인이 된다.

### 3. 편광과 TE/TM 성분

편광(polarization)은 진행파 또는 정상파에서 전기장 벡터가 향하는 방향과 시간에 따른 궤적을 나타낸다. 강의안은 편광을 다음 세 형태로 구분한다.

- 선형 편광(linear polarization)
- 원형 편광(circular polarization)
- 타원 편광(elliptical polarization)

유전체 경계에 빛이 비스듬히 입사할 때는 전기장 방향에 따라 두 편광을 구분한다.

- TE 편광: 전기장이 입사면에 수직인 편광
- TM 편광: 전기장이 입사면에 평행한 편광

입사파 $E_i$가 경계에서 반사파 $E_r$와 투과파 $E_t$로 나뉘는 기본 구조는 같지만, TE와 TM의 반사 계수는 서로 다르다. 뒤에서 다룰 브루스터 각이 TM 편광에서만 나타나는 이유도 이 편광 의존성에 있다. 도파 구조에서는 전기장이 전파 방향에 수직인 경우를 transverse electric(TE), 자기장이 전파 방향에 수직인 경우를 transverse magnetic(TM)이라고 부른다.

### 4. 공진 광 공동과 레이저

#### 4.1 레이저를 광 발진기로 보기

레이저는 광 발진기(optical oscillator)로 볼 수 있다. 발진에는 증폭, 주파수 선택, 되먹임이 필요하며, 광 공동에서는 다음 요소가 그 역할을 맡는다.

- 이득 매질(gain medium): 광 증폭
- 양쪽 거울: 광 되먹임(optical feedback)
- 공동의 위상 조건: 허용 공진 모드 선택

빛이 두 거울 사이를 왕복하면서 같은 위상으로 중첩되면 정상파(standing wave)가 형성된다. 이는 한 방향으로만 진행하는 진행파와 구별된다.

#### 4.2 공진 조건과 모드 간격

길이가 $L$인 공동을 한 번 왕복할 때의 위상 변화는 $2kL$이다. 같은 위상으로 되돌아오기 위한 조건은

$$
2kL=2\pi m,\qquad m\in\mathbb{Z}
$$

이다. $k=2\pi/\lambda$를 대입하면

$$
2\left(\frac{2\pi}{\lambda}\right)L=2\pi m
$$

이므로 허용 파장은

$$
\lambda_m=\frac{2L}{m}
$$

이다. 또한 매질에서

$$
f\lambda=\frac{c}{n}
$$

이므로 공진 주파수는

$$
f_m=\frac{mc}{2nL}
$$

가 된다. 이웃한 종방향 공진 모드의 주파수 간격은

$$
\Delta f_{\mathrm{cavity}}
=f_{m+1}-f_m
=\frac{c}{2nL}
$$

이다. 따라서 공동이 길거나 굴절률이 클수록 모드 간격은 좁아진다.

공동은 위 식을 만족하는 이산적인 공진 모드만 허용한다. 실제 레이저에서는 이 공진 모드들이 이득 매질의 광 이득 분포와 겹치며, 공진 조건과 광 이득을 함께 만족하는 모드가 발진에 기여한다.

### 5. 유전체 평면 경계에서의 반사

유전체 경계 반사는 광통신 시스템의 여러 접속 지점에서 나타난다.

1. 레이저 다이오드(LD)와 광섬유 사이
2. 광섬유와 광섬유 사이
3. 광섬유 내부의 코어와 클래딩 사이

굴절률 $n_1$인 매질에서 $n_2$인 매질로 빛이 입사하면, 경계에서 입사파가 반사파와 투과파로 나뉜다.

#### 5.1 수직 입사

수직 입사에서 전기장의 진폭 반사 계수는

$$
\rho
=\frac{n_1-n_2}{n_1+n_2}
$$

이고, 전력 반사율은 진폭 반사 계수 크기의 제곱이다.

$$
R=|\rho|^2
=\left(\frac{n_1-n_2}{n_1+n_2}\right)^2
$$

$n_1=n_2$이면 $\rho=0$이므로 반사가 없다. 두 굴절률의 차이가 커질수록 반사율이 증가한다.

#### 5.2 비스듬한 입사

입사각을 $\theta$라고 할 때 강의안에 제시된 TM 편광의 진폭 반사 계수는

$$
\rho_{\mathrm{TM}}
=
\frac{
-n_2^2\cos\theta
+n_1\sqrt{n_2^2-n_1^2\sin^2\theta}
}{
n_2^2\cos\theta
+n_1\sqrt{n_2^2-n_1^2\sin^2\theta}
}
$$

이고, TE 편광의 진폭 반사 계수는

$$
\rho_{\mathrm{TE}}
=
\frac{
n_1\cos\theta
-\sqrt{n_2^2-n_1^2\sin^2\theta}
}{
n_1\cos\theta
+\sqrt{n_2^2-n_1^2\sin^2\theta}
}
$$

이다. 수직 입사에서는 $\theta=0$이므로

$$
\rho_{\mathrm{TM}}=\rho_{\mathrm{TE}}
=\frac{n_1-n_2}{n_1+n_2}
$$

가 되어 편광에 따른 차이가 사라진다. 반면 비스듬한 입사에서는 TE와 TM의 반사 계수가 달라지므로, 경계 결합과 반사 특성은 편광에 민감해진다.

### 6. 브루스터 각과 전반사

#### 6.1 브루스터 각

TM 편광에서는 특정 입사각에서 반사가 0이 된다. 이 각을 브루스터 각 $\theta_B$라고 하며,

$$
\tan\theta_B=\frac{n_2}{n_1}
$$

을 만족한다. 따라서

$$
\theta_B=\tan^{-1}\left(\frac{n_2}{n_1}\right)
$$

이다. $\theta=\theta_B$에서 TM 성분은 반사되지 않고 모두 투과하므로, 유전체 매질로 빛을 결합할 때 유용하다. 강의안은 레이저 결정의 경계면을 브루스터 각으로 기울여 배치한 구조를 예로 든다.

#### 6.2 임계각과 전반사

빛이 굴절률이 큰 매질에서 작은 매질로 진행할 때, 즉

$$
n_1>n_2
$$

일 때 임계각 $\theta_c$가 존재한다. 임계각은

$$
\sin\theta_c=\frac{n_2}{n_1}
$$

또는

$$
\theta_c=\sin^{-1}\left(\frac{n_2}{n_1}\right)
$$

로 주어진다.

- $\theta>\theta_c$: 전반사(total internal reflection, TIR)
- $\theta<\theta_c$: 일부는 반사되고 일부는 투과

광섬유의 코어와 클래딩 경계는 이 조건을 적용하는 대표적인 유전체 경계이다.

### 7. 전반사와 소멸장

전반사가 일어나면 $n_2$ 매질로 진행하는 전력은 없지만, 경계 바깥의 장 자체가 즉시 0이 되는 것은 아니다. $n_2$ 매질 안에는 경계에서 멀어질수록 지수적으로 감소하는 소멸장(evanescent field)이 존재한다.

경계에서 $n_2$ 방향으로 떨어진 거리를 $z$라고 하면 장의 공간 의존성은

$$
E(z)\propto e^{-\gamma z}
$$

이고, 감쇠 상수는

$$
\gamma
=k_0\sqrt{n_1^2\sin^2\theta_i-n_2^2}
$$

이다. 여기서 $k_0$는 자유 공간 파수이고 $\theta_i$는 입사각이다.

침투 깊이 또는 skin depth $\delta$는 장의 크기가 경계값의 $1/e$로 감소하는 거리이다. 따라서

$$
e^{-\gamma\delta}=\frac{1}{e}
$$

이므로

$$
\delta=\frac{1}{\gamma}
=\frac{1}{
k_0\sqrt{n_1^2\sin^2\theta_i-n_2^2}
}
$$

이다. 전반사는 경계 너머로 전달되는 진행파를 만들지 않지만, 경계 가까운 영역에는 유한한 깊이까지 장이 침투한다.

### 8. 설계 관점에서 연결하기

이 강의의 개념은 광통신 링크의 서로 다른 설계 문제로 이어진다.

- 매질의 굴절률 $n$은 광속, 파장, 전파 상수를 함께 결정한다.
- 감쇠 계수 $\alpha$와 전송 거리 $L$은 수신 전력을 결정한다.
- 분산 계수 $M$, 거리 $L$, 광원의 선폭 $\Delta\lambda$는 펄스 확장을 결정하며 전송 용량을 제한한다.
- 편광은 비스듬한 유전체 경계에서 반사 계수를 달라지게 한다.
- 공진기 길이 $L$과 굴절률 $n$은 레이저 공진 모드 간격 $\Delta f_{\mathrm{cavity}}$를 결정한다.
- 굴절률 비와 입사각은 브루스터 무반사 조건과 전반사 조건을 결정한다.
- 전반사 경계에서도 $\delta=1/\gamma$만큼 소멸장이 침투한다.

### 핵심 요약

1. 광파는 $E=E_0\sin(\omega t-kz)$로 표현할 수 있고, 매질에서는 $v_p=c/n$, $\lambda=\lambda_0/n$이다.
2. 손실 매질에서 전기장 진폭은 $e^{-\alpha L}$, 전력은 $e^{-2\alpha L}$로 감소한다.
3. 분산은 파장 성분별 지연 차를 만들며, $|\Delta\tau|=|M|L|\Delta\lambda|$가 펄스 확장의 크기를 나타낸다.
4. TE와 TM 편광은 비스듬한 유전체 경계에서 서로 다른 반사 계수를 갖는다.
5. 광 공동의 공진 조건은 $2kL=2\pi m$이며, 모드 간격은 $\Delta f_{\mathrm{cavity}}=c/(2nL)$이다.
6. 수직 입사의 전력 반사율은 $R=[(n_1-n_2)/(n_1+n_2)]^2$이다.
7. TM 편광은 $\tan\theta_B=n_2/n_1$인 브루스터 각에서 반사가 0이 된다.
8. $n_1>n_2$이고 $\theta>\theta_c$이면 전반사가 일어나며, 경계 너머에는 $e^{-\gamma z}$로 감소하는 소멸장이 남는다.
