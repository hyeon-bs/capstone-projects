# 🔧 시각장애인을 위한 신호등 안내 시스템 (Blind Traffic Light Assistance System)

## 📝 프로젝트 개요
기존 음향신호기의 접근성 한계와 유지보수 문제를 개선하기 위해, OpenCV 기반 영상처리 기술을 활용하여 신호등의 색상 및 원형 패턴을 인식하고 시각장애인에게 보행 가능 여부를 진동 및 알림 형태로 전달하는 Android 애플리케이션을 설계·구현했습니다.

카메라 입력 스트림을 실시간 분석하여 신호등 객체를 탐지하고, 원형 검출 및 색상 필터링 알고리즘을 통해 보행 상태를 판별하는 구조를 구현했습니다.

## 📅 프로젝트 기간
* 2019.03 ~ 2019.06

## 💻 기술 스택
* **Language:** Java, C++
* **Framework & Library:** OpenCV, Android SDK
* **Platform:** Android
* **Development Environment:** Android Studio
* **OS:** Linux / Android

## 🎯 프로젝트 목적 및 내용

### 1. OpenCV 기반 신호등 객체 인식 시스템 구축
카메라 입력 데이터를 실시간으로 처리하여 신호등 객체의 색상과 원형 패턴을 인식하는 영상처리 파이프라인을 설계했습니다.
* **원형 검출(Hough Circle 기반)**
OpenCV `vector<Vec3f>` 구조를 활용하여 신호등 원형 객체를 탐지하고 불필요한 객체 인식을 최소화했습니다.
* **색상 인식(Color Detection)**
RGB 기반 영상 데이터를 변환하여 빨간불/초록불 상태를 필터링하고, 특정 색상 범위만 추출하도록 처리했습니다.

```C
/* 원형 및 색상 인식 처리 흐름 */
vector<Vec3f> circles;
vector<Mat> planes;

cvtColor(src, dst, COLOR_RGB2YCrCb);
```

### 2. Android 카메라 및 OpenCV 연동 구조 설계
Android Camera 모듈과 OpenCV 라이브러리를 연동하여 모바일 디바이스 환경에서 실시간 영상 스트림을 처리할 수 있도록 구현했습니다.

* **퍼미션 처리 및 카메라 제어**
`AndroidManifest.xml` 퍼미션 등록 및 `hasPermissions()`, `onRequestPermissionResult()` 흐름을 통해 카메라 접근 제어를 구현했습니다.
* **Native Library 연동**
JNI 기반 `native-lib.cpp`와 Java 레이어를 연결하여 영상 데이터를 처리하는 구조를 설계했습니다.

### 3. 상태 전달 및 사용자 알림 시스템 구현
인식된 신호등 상태를 사용자에게 전달하기 위해 Android 진동(Vibrator) 기반 알림 구조를 설계했습니다.

* **상태 기반 인터랙션**
초록불 인식 시 보행 가능 알림, 빨간불 인식 시 보행 중지 알림을 전달하도록 이벤트 흐름을 분리했습니다.
* **BroadcastReceiver 기반 알림 관리**
전역 이벤트 수신 구조를 적용하여 상태 변화 시 즉각적인 피드백이 가능하도록 구성했습니다.


## 🛠️ 주요 기능 및 코드 구조

### 1. 영상처리 및 객체 인식 모듈 (`Vision Core`)
#### `native-lib.cpp`
* OpenCV 기반 원형 검출 및 색상 추출 처리
* RGB → YCbCr 변환 기반 색상 필터링
* JNI 브릿지를 통한 Android 프레임 전달

#### `MainActivity.java`
* Camera Preview 스트림 관리
* OpenCV 라이브러리 초기화
* 권한 요청 및 카메라 상태 제어

### 2. 사용자 인터랙션 모듈 (`Alert System`)
#### `alarm.java`
* 상태값 기반 진동 알림 처리
* BroadcastReceiver 기반 이벤트 수신

#### `Loading.java`
* 앱 시작 시 로딩 화면 처리
* Thread 기반 초기 UI 전환 흐름 구현


## 🖥️ 주요 구현 내용

### OpenCV 기반 원형 객체 인식
신호등 원형 패턴만 인식하도록 제한하여 불필요한 객체 탐지를 최소화했습니다.

### 신호등 색상 판별
빨간색 및 초록색 범위를 필터링하여 보행 가능 여부를 판단했습니다.

### Android 카메라 연동
실시간 카메라 입력 스트림을 OpenCV 처리 파이프라인과 연결했습니다.

### 진동 기반 사용자 피드백
시각 정보 없이도 상태를 인지할 수 있도록 진동 기반 알림 시스템을 구성했습니다.


## ⚠️ 프로젝트 문제점 및 한계
1. Android Camera Preview와 OpenCV 연동 과정에서 디바이스 환경에 따라 검은 화면(Camera Black Screen) 이슈가 발생했습니다.

2. 다양한 조명 환경 및 거리 조건에서 색상 인식 정확도가 일정하지 않았으며, 주변 객체에 대한 오검출 가능성이 존재했습니다.

3. 스마트폰 카메라를 직접 신호등 방향으로 맞춰야 하는 구조 특성상 실제 보행 환경에서 사용성이 제한될 수 있었습니다.
