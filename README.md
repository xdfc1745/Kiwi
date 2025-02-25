# Kiwi : 지능형 한국어 형태소 분석기(Korean Intelligent Word Identifier)

![KiwiLogo](https://repository-images.githubusercontent.com/82677855/eb9fa478-175d-47a5-8e07-0e169c030ff5)

x86_64: 
[![Action Status Centos5](https://github.com/bab2min/Kiwi/workflows/Centos5/badge.svg)](https://github.com/bab2min/Kiwi/actions)
[![Action Status Windows](https://github.com/bab2min/Kiwi/workflows/Windows/badge.svg)](https://github.com/bab2min/Kiwi/actions)
[![Action Status Ubuntu](https://github.com/bab2min/Kiwi/workflows/Ubuntu/badge.svg)](https://github.com/bab2min/Kiwi/actions)
[![Action Status macOS](https://github.com/bab2min/Kiwi/workflows/macOS/badge.svg)](https://github.com/bab2min/Kiwi/actions)

Other:
[![Action Status ARM64](https://github.com/bab2min/Kiwi/workflows/Arm64-Centos7/badge.svg)](https://github.com/bab2min/Kiwi/actions)
[![Action Status PPC64LE](https://github.com/bab2min/Kiwi/workflows/PPC64LE-Centos7/badge.svg)](https://github.com/bab2min/Kiwi/actions)

Kiwi는 빠른 속도와 범용적인 성능을 지향하는 한국어 형태소 분석기 라이브러리입니다. 한국어 자연어처리에 관심 있는 사람이면 누구나 쉽게 사용할 수 있도록 오픈 소스로 공개 중이며, C++로 구현된 코어 라이브러리를 래핑하여 다양한 프로그래밍 언어에 사용할 수 있도록 준비 중입니다. 

형태소 분석은 세종 품사 태그 체계를 기반으로 하고 있으며 모델 학습에는 세종계획 말뭉치와 모두의 말뭉치를 사용하고 있습니다. 문어 텍스트의 경우 약 94% 정도의 정확도로 한국어 문장의 형태소를 분석해 낼 수 있습니다.

또한 라이브러리 차원에서 멀티스레딩을 지원하기 때문에 대량의 텍스트를 분석해야할 경우 멀티코어를 활용하여 빠른 분석이 가능합니다.

아직 부족한 부분이 많기에 개발자분들의 많은 관심과 기여 부탁드립니다.

## 설치

### C++

#### 컴파일된 바이너리 다운로드
https://github.com/bab2min/Kiwi/releases 에서 Windows, Linux, macOS 버전으로 컴파일된 Library 파일과 모델 파일을 다운로드 받을 수 있습니다.

#### Windows
Visual Studio 2019 이상을 사용하여 `Kiwi.sln` 파일을 실행하여 컴파일할 수 있습니다.

#### Linux
이 레포지토리를 clone한 뒤 cmake>=3.9를 사용하여 컴파일합니다. 

##### gcc >= 5.0 이상 혹은 다른 c++11 호환 컴파일러 사용가능 환경
```console
$ git clone https://github.com/bab2min/Kiwi
$ cd Kiwi
$ git submodule sync
$ git submodule update --init --recursive
$ mkdir build && cd build
$ cmake -DCMAKE_BUILD_TYPE=Release ../
$ make
```

##### gcc >= 4.8, < 5.0
Centos5와 같이 gcc 4.8까지만 지원하는 환경에서는 googletest의 버전을 1.8.x로 낮춰야 컴파일 가능합니다.
```console
$ git clone https://github.com/bab2min/Kiwi
$ cd Kiwi
$ git submodule sync
$ git submodule update --init --recursive
$ cd third_party/googletest && git checkout v1.8.x && cd ../../
$ mkdir build && cd build
$ cmake -DCMAKE_BUILD_TYPE=Release ../
$ make
```

설치가 잘 됐는지 확인하기 위해서는 `kiwi-evaluator`를 실행해봅니다.
```console
$ ./kiwi-evaluator --model ../ModelGenerator ../eval_data/web.txt ../eval_data/written.txt

Loading Time : 1198.05 ms
Mem Usage : 158.539 MB

Test file: ../eval_data/web.txt
0.822854, 0.804857
Total (96 lines) Time : 105.274 ms
Time per Line : 1.0966 ms
================
Test file: ../eval_data/written.txt
0.946757, 0.943904
Total (23 lines) Time : 34.2189 ms
Time per Line : 1.48778 ms
================
```

### C API
include/kiwi/capi.h 를 참조하세요.

#### 컴파일된 바이너리 다운로드
https://github.com/bab2min/Kiwi/releases 에서 Windows, Linux, macOS 버전으로 컴파일된 Library 파일과 모델 파일을 다운로드 받을 수 있습니다.

### C# Wrapper
https://github.com/bab2min/kiwi-gui

### Python3 Wrapper
또한 Python3용 API인 Kiwipiepy가 제공됩니다. 이에 대해서는 https://github.com/bab2min/kiwipiepy 를 참조하시길 바랍니다.

### R Wrapper
mrchypark님께서 작업해주신 R언어용 wrapper인 [Elbird](https://github.com/mrchypark/Elbird)가 있습니다.

### 응용 프로그램
Kiwi는 C# 기반의 GUI 형태로도 제공됩니다.
형태소 분석기는 사용해야하지만 별도의 프로그래밍 지식이 없는 경우 이 프로그램을 사용하시면 됩니다.
다음 프로그램은 Windows에서만 구동 가능합니다.
https://github.com/bab2min/kiwi-gui 에서 다운받을 수 있습니다.


## 업데이트 내역
* v0.10
  * 소스 코드 리팩토링. 인터페이스를 `kiwi::KiwiBuilder`(분석기 사전을 관리)와 `kiwi::Kiwi`(실제 형태소 분석을 수행)로 분할
  * CMake 적용
  * 언어 모델 엔진 재구현. 메모리 & 속도 최적화. 모델 파일 크기 최적화
  * Linux 환경에서 간헐적으로 발생하는 Segmentation Fault 해결

* v0.9
  * `default.dict`에 포함된 활용형 단어 때문에 발생하는 오분석 수정
  * custom allocator에서 발생하는 멀티스레딩 메모리 누수 해결
  * mimalloc과 연동가능하도록 옵션 추가 (-DUSE_MIMALLOC)
  * 형태소 탐색 시 조사/어미의 결합조건을 미리 고려하도록 변경, 속도 개선
  * 일부 명사(`전랑` 처럼 받침 + 랑으로 끝나는 미등재 명사) 입력시 분석이 실패하는 버그 수정
  * 공백문자만 포함된 문자열 입력시 분석결과가 `/UN`로 잘못나오는 문제 수정

* v0.8
  * URL, 이메일, 해시태그, 멘션 검출 추가
  * 치(하지), 컨대(하건대), 토록(하도록), 케(하게) 축약형이 포함된 동사 활용형 분석 개선
  * 사용자 사전에 알파벳이나 숫자, 특수 기호가 포함시 버그 수정
  * 특정 상황에서 결합조건이 무시되던 문제를 해결

* v0.7
  * 사전 로딩 속도 개선
  * 이형태 통합 유무 옵션 추가
  * 분석 속도 향상

* v0.6
  * 검색 알고리즘 최적화로 인한 속도 향상 (분석 속도: 0.33MB/s)
  * 전반적인 정확도 상승 (92%~96%까지)

* v0.5
  * 언어 모형 개선(Kneser-Ney 3-gram LM)
  * 전반적인 정확도 상승 (최소 89%에서 94%까지)
  * 코퍼스에서 미등록 단어 추출 기능 추가
  * 멀티스레딩 지원

* v0.4
  * 알고리즘 개선
  * 실행속도 약 101% 향상 (분석 속도: 0.28MB/s)

* v0.3
  * 알고리즘 및 메모리 관리 최적화
  * 실행속도 약 86% 향상 (분석 속도: 0.14MB/s)

* v0.2
  * 정확도 85%까지 향상.
  * 상호정보량 맵을 이용하여 분석 모호성 감소
  * 서술격 조사 생략 추적 가능해짐
  * (분석 속도: 0.08MB/s)

* v0.1
  * 첫 릴리즈. 약 80% 정확도
  
## 품사 태그

세종 품사 태그를 기초로 하되, 일부 품사 태그를 추가/수정하여 사용하고 있습니다.

<table>
<tr><th>대분류</th><th>태그</th><th>설명</th></tr>
<tr><th rowspan='5'>체언(N)</th><td>NNG</td><td>일반 명사</td></tr>
<tr><td>NNP</td><td>고유 명사</td></tr>
<tr><td>NNB</td><td>의존 명사</td></tr>
<tr><td>NR</td><td>수사</td></tr>
<tr><td>NP</td><td>대명사</td></tr>
<tr><th rowspan='5'>용언(V)</th><td>VV</td><td>동사</td></tr>
<tr><td>VA</td><td>형용사</td></tr>
<tr><td>VX</td><td>보조 용언</td></tr>
<tr><td>VCP</td><td>긍정 지시사(이다)</td></tr>
<tr><td>VCN</td><td>부정 지시사(아니다)</td></tr>
<tr><th rowspan='1'>관형사</th><td>MM</td><td>관형사</td></tr>
<tr><th rowspan='2'>부사(MA)</th><td>MAG</td><td>일반 부사</td></tr>
<tr><td>MAJ</td><td>접속 부사</td></tr>
<tr><th rowspan='1'>감탄사</th><td>IC</td><td>감탄사</td></tr>
<tr><th rowspan='9'>조사(J)</th><td>JKS</td><td>주격 조사</td></tr>
<tr><td>JKC</td><td>보격 조사</td></tr>
<tr><td>JKG</td><td>관형격 조사</td></tr>
<tr><td>JKO</td><td>목적격 조사</td></tr>
<tr><td>JKB</td><td>부사격 조사</td></tr>
<tr><td>JKV</td><td>호격 조사</td></tr>
<tr><td>JKQ</td><td>인용격 조사</td></tr>
<tr><td>JX</td><td>보조사</td></tr>
<tr><td>JC</td><td>접속 조사</td></tr>
<tr><th rowspan='5'>어미(E)</th><td>EP</td><td>선어말 어미</td></tr>
<tr><td>EF</td><td>종결 어미</td></tr>
<tr><td>EC</td><td>연결 어미</td></tr>
<tr><td>ETN</td><td>명사형 전성 어미</td></tr>
<tr><td>ETM</td><td>관형형 전성 어미</td></tr>
<tr><th rowspan='1'>접두사</th><td>XPN</td><td>체언 접두사</td></tr>
<tr><th rowspan='3'>접미사(XS)</th><td>XSN</td><td>명사 파생 접미사</td></tr>
<tr><td>XSV</td><td>동사 파생 접미사</td></tr>
<tr><td>XSA</td><td>형용사 파생 접미사</td></tr>
<tr><th rowspan='1'>어근</th><td>XR</td><td>어근</td></tr>
<tr><th rowspan='9'>부호, 외국어, 특수문자(S)</th><td>SF</td><td>종결 부호(. ! ?)</td></tr>
<tr><td>SP</td><td>구분 부호(, / : ;)</td></tr>
<tr><td>SS</td><td>인용 부호 및 괄호(' " ( ) [ ] < > { } ― ‘ ’ “ ” ≪ ≫ 등)</td></tr>
<tr><td>SE</td><td>줄임표(…)</td></tr>
<tr><td>SO</td><td>붙임표(- ~)</td></tr>
<tr><td>SW</td><td>기타 특수 문자</td></tr>
<tr><td>SL</td><td>알파벳(A-Z a-z)</td></tr>
<tr><td>SH</td><td>한자</td></tr>
<tr><td>SN</td><td>숫자(0-9)</td></tr>
<tr><th rowspan='1'>분석 불능</th><td>UN</td><td>분석 불능<sup>*</sup></td></tr>
<tr><th rowspan='4'>웹(W)</th><td>W_URL</td><td>URL 주소<sup>*</sup></td></tr>
<tr><td>W_EMAIL</td><td>이메일 주소<sup>*</sup></td></tr>
<tr><td>W_HASHTAG</td><td>해시태그(#abcd)<sup>*</sup></td></tr>
<tr><td>W_MENTION</td><td>멘션(@abcd)<sup>*</sup></td></tr>
</table>

<sup>*</sup> 세종 품사 태그와 다른 독자적인 태그입니다.



## 다른 형태소 분석기와의 비교
다음의 성능 평가는 konlpy-0.5.1에 포함된 Hannanum, Kkma, Komoran, Okt를 Kiwi와 비교한 것입니다.

평가는 AMD Ryzen 7 3700X @3.6GHz, RAM 32GB, Windows 10(64bit) 환경에서 진행되었습니다.

![형태소 분석기 실행 속도 비교](/KiwiChart.PNG)

| | Loading | 1 | 10 | 100 | 1000 | 10000 | 100000 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
**Hannanum** | 0.434 | 0.003 | 0.005 | 0.015 | 0.055 | 0.424 | 5.360
**Kkma** | 2.481 | 0.004 | 0.062 | 0.087 | 0.348 | 2.058 | 21.054
**Komoran** | 1.068 | 0.005 | 0.003 | 0.009 | 0.045 | 0.469 | 17.974
**Okt** | 1.787 | 0.005 | 0.023 | 0.040 | 0.094 | 0.376 | 2.527
**Kiwi** | **1.009** | **0.002** | **0.002** | **0.004** | **0.029** | **0.137** | **1.361**

Kiwi의 로딩 시간 및 처리 속도는 기존의 분석기들과 비교할 때 매우 빠른 편임을 확인할 수 있습니다.

위의 성능 평가는
https://github.com/bab2min/kiwipiepy/blob/master/evaluate.py 를 통해 직접 실시해볼 수 있습니다.

## 성능

* 비문학(신문기사): 0.928
* 문학작품: 0.960

결과 예시

    프랑스의 세계적인 의상 디자이너 엠마누엘 웅가로가 실내 장식용 직물 디자이너로 나섰다.
    (정답) 프랑스/NNP	의/JKG	세계/NNG	적/XSN	이/VCP	ㄴ/ETM	의상/NNG	디자이너/NNG	엠마누엘/NNP	웅가로/NNP	가/JKS	실내/NNG	장식/NNG	용/XSN	직물/NNG	디자이너/NNG	로/JKB	나서/VV	었/EP	다/EF	./SF
    (Kiwi) 프랑스/NNP	의/JKG	세계/NNG	적/XSN	이/VCP	ᆫ/ETM	의상/NNG	디자이너/NNG	엠마누/NNP	에/JKB	ᆯ/JKO	웅가로/NNP	가/JKS	실내/NNG	장식/NNG	용/XSN	직물/NNG	디자이너/NNG	로/JKB	나서/VV	었/EP	다/EF	./SF
    
    둥글둥글한 돌은 아무리 굴러도 흔적이 남지 않습니다.
    (정답) 둥글둥글/MAG	하/XSA	ㄴ/ETM	돌/NNG	은/JX	아무리/MAG	구르/VV	어도/EC	흔적/NNG	이/JKS	남/VV	지/EC	않/VX	습니다/EF	./SF
    (Kiwi) 둥글둥글/MAG	하/XSA	ᆫ/ETM	돌/NNG	은/JX	아무리/MAG	구르/VV	어도/EC	흔적/NNG	이/JKS	남/VV	지/EC	않/VX	습니다/EF	./SF

	하늘을 훨훨 나는 새처럼
	(정답) 하늘/NNG	을/JKO	훨훨/MAG	날/VV	는/ETM	새/NNG	처럼/JKB
	(Kiwi) 하늘/NNG	을/JKO	훨훨/MAG	날/VV	는/ETM	새/NNG	처럼/JKB

	아버지가방에들어가신다
	(정답) 아버지/NNG	가/JKS	방/NNG	에/JKB	들어가/VV	시/EP	ㄴ다/EF
	(Kiwi) 아버지/NNG	가/JKS	방/NNG	에/JKB	들어가/VV	시/EP	ᆫ다/EC

## 데모

https://lab.bab2min.pe.kr/kiwi 에서 데모를 실행해 볼 수 있습니다.


## 라이센스
Kiwi는 LGPL v3 라이센스로 배포됩니다. 

이메일: bab2min@gmail.com

블로그: http://bab2min.tistory.com/560

## 기여하기
자세한 내용은 [CONTRIBUTING.md](CONTRIBUTING.md) 에서 확인해주세요.
