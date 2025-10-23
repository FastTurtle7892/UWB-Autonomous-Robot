# UWB 기반 자율주행 이동로봇 설계 및 구현

> **UWB 통신 모듈(DW1000)을 활용하여, PHY/MAC 계층 통신부터 DS-TWR 정밀 거리 측정, 삼각측량(Triangulation) 알고리즘을 거쳐 최종적으로 메카넘 휠 로봇의 자율주행을 구현하는 종합 임베디드 시스템 프로젝트**

[![UWB](https://img.shields.io/badge/UWB-IEEE%20802.15.4-blue)](https://www.ieee802.org/15/pub/TG4z.html)
[![Hardware](https://img.shields.io/badge/Module-DW1000-orange)]()
[![Platform](https://img.shields.io/badge/Platform-Arduino%20UNO-cyan)]()
[![Robot](https://img.shields.io/badge/Robot-Mecanum%20Wheel-lightgrey)]()

---

## 1. 프로젝트 개요

본 프로젝트는 **UWB(Ultra-Wideband) 기술**을 활용한 실시간 위치 인식 시스템(RTLS)을 구축하고, 이를 **자율주행 이동로봇**에 적용하는 것을 목표로 합니다.

프로젝트는 UWB 통신의 가장 기초적인 Basic Sender/Receiver 구현에서 시작하여, 물리계층(PHY)과 MAC 계층의 차이를 학습합니다. 이후 UWB의 핵심 기능인 **DS-TWR (Double-Sided Two-Way Ranging)**을 구현하여 정밀한 거리를 측정하고, 3개 이상의 앵커(Anchor)와 1개의 태그(Tag)를 이용한 **삼각측량(Triangulation)** 알고리즘으로 태그의 2D 좌표 (x, y)를 실시간으로 계산합니다.

최종적으로, 이 좌표값을 메카넘 휠(Mecanum Wheel) 로봇에 탑재된 태그로 전송하여, 로봇이 스스로 위치를 인지하고 장애물을 회피하며 목표 지점까지 이동하는 **자율주행 시스템**을 완성합니다.

---

## 2. 핵심 구현 기능

* **UWB 통신 기초:** Arduino와 DW1000 모듈을 이용한 **물리계층(PHY)** 및 **MAC 계층** 기반의 기본 데이터 송/수신 기능 구현.
* **로봇 하드웨어 제어:** 4채널 모터 드라이버를 이용한 **메카넘 휠 로봇**의 조립 및 전후좌우, 대각선, 회전 제어.
* **원격 조종 시스템 (HW1):** 조이스틱 모듈과 UWB 통신을 결합하여 로봇을 무선 원격 제어하는 기능 구현.
* **정밀 거리 측정:** **DS-TWR (Double-Sided Two-Way Ranging)** 방식을 PHY 및 MAC 계층에서 각각 구현하여, 클럭 오차를 보정한 정확한 거리(ToF) 측정.
* **실시간 위치 인식 (RTLS):** 3개의 고정된 앵커와 1개의 이동형 태그 간의 DS-TWR을 수행하고, **삼각측량(Triangulation)** 알고리즘을 통해 태그의 실시간 (x, y) 좌표 도출.
* **자율 주행 (Capstone - HW2):** RTLS를 통해 계산된 로봇(태그)의 현재 위치를 기반으로 장애물을 인식하고 경로를 계획하여, 출발점에서 목표 지점까지 스스로 주행하는 **자율주행 알고리즘** 구현.
* **UWB 채널 분석:** **CIR (Channel Impulse Response)** 데이터를 추출하고 Arduino 시리얼 플로터를 통해 시각화하여, 다중 경로(Multipath) 환경에서의 신호 품질 분석.

---

## 3. 기술 스택 (Tech Stack)

### 하드웨어 (Hardware)
* **UWB Module:** Decawave DWS1000 (DW1000 IC 기반)
* **Controller:** Arduino UNO R3 (x4)
* **Robot Platform:** 4륜 메카넘 휠 (Mecanum Wheel) 로봇 섀시
* **Motor Driver:** 4CH Motor Driver Shield (TB6612FNG 기반)
* **Input Device:** 2축 조이스틱(Joystick) 모듈
* **Power:** 7-12V 리튬 배터리
> ![alt text](/imgs/image.png)

### 소프트웨어 (Software)
* **IDE:** Arduino IDE, Visual Studio Code
* **Language:** C/C++ (Arduino)
* **Visualization:** Arduino Serial Monitor & Serial Plotter

---

## 4. 프로젝트 모듈 상세 (실습 순서)

본 프로젝트는 총 9개의 실습과 2개의 하드웨어 과제를 통해 점진적으로 완성됩니다.

* **실습 1:** Basic Connectivity Test & 환경 설치
    * Arduino IDE, 라이브러리 설치 및 DW1000 모듈 기본 연결 테스트.
* **실습 2:** Basic Sender & Receiver (물리계층 - PHY)
    * MAC 헤더 없이 순수 물리계층(PHY)에서 데이터를 송수신.
* **실습 3:** Basic Sender & Receiver (MAC 계층)
    * PAN ID, Device Address 등 IEEE 802.15.4 MAC 프레임을 사용하여 특정 장치 간 통신.
* **실습 4:** Mecanum Wheel Control
    * 로봇 조립 및 4개의 모터를 개별 제어하여 메카넘 휠의 8방향(전후좌우, 대각선, 회전) 구동.
* **HW 1:** Joystick-controlled Vehicle
    * `실습 3`과 `실습 4`를 결합하여 UWB 통신으로 로봇을 원격 조종.
* **실습 5:** UWB Chat Program
    * Interrupt를 활용하여 송/수신(Transceiver)이 동시에 가능한 양방향 채팅 프로그램 구현.
* **실습 6:** DS-TWR (물리계층 - PHY)
    * PHY 계층에서 DS-TWR (ADS-TWR)을 구현하여 정밀 거리 측정.
* **실습 7:** DS-TWR (MAC 계층)
    * MAC 계층에서 주소 필터링을 적용하여 원하는 앵커/태그 간의 안정적인 거리 측정.
* **실습 8:** Triangulation (삼각측량)
    * `실습 7`을 확장하여 3개의 앵커(Anchor)로부터 거리 값을 받아 태그(Tag)의 2D 좌표를 계산.
* **실습 9:** CIR (Channel Impulse Response) 분석
    * DW1000 레지스터에서 CIR 데이터를 추출하여 신호의 품질과 첫 번째 경로(ToF)를 시각적으로 분석.
* **HW 2 (Capstone):** UWB를 이용한 자율주행 차량 설계
    * `실습 8`과 `실습 4`를 결합하여, 로봇이 스스로 위치를 파악하고 장애물을 피해 목적지까지 이동하는 최종 자율주행 시스템 구현.
