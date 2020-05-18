---
layout: post
title:  "uipath Robotic Enterprise Framework"
date:   2020-05-18 15:35:00 +0900
categories: jekyll update
comments : true
---

## 트랜잭션이란?

트랜잭션(Transaction)은 데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위 또는 한꺼번에 모두 수행되어야 할 일련의 연산들을 의미한다.

출처 : https://coding-factory.tistory.com/226

## 비즈니스 프로세스의 종류
비즈니스 프로세스 단계와 반복 단계를 고려하여 비즈니스 프로세스를 세 가지 범주로 나눌 수 있습니다.

**Linear**

데이터 처리를 단 한번 실행 합니다. 다른 데이터를 처리하기 위해서는 트랜잭션을 처음부터 다시 시작해야 합니다.

Linear process는 간단하고 사용하기도 쉽지만, 다른 데이터를 반복적으로 활용하는 상황에는 맞지 않습니다.

![Linear Process](https://html.cdn.contentraven.com/crcloud/crscorm/uploads/uipath_lms_11218/encryptedfile/149710/v3.0/scormcontent/assets/txZKugPZFV_DrF8f_T6tN9Z3UOhombemc.png)

**Iterative**

데이터 처리를 반복적으로 실행 합니다. 각 실행 마다 다른 데이터를 사용합니다.

이런 종류의 처리는 반복문으로 쉽게 구현할 수 있습니다. 그러나 반복 처리도중 문제가 생기면 이후 남아 있는 반복 작업도 영향을 받습니다.
![Iterative](https://html.cdn.contentraven.com/crcloud/crscorm/uploads/uipath_lms_11218/encryptedfile/149710/v3.0/scormcontent/assets/SU1vkC-RXGV5NAu6_BvSoMNnr0HjFYzrd.png)

**Transactional**

Iterative 작업과 비슷합니다. 각 작업을 다른 데이터로 실행하지만 병렬로 실행한다는게 차이점 입니다. 따라서 어떤 작업에 문제가 생겨도 다른 작업은 정상적으로 실행합니다.

이런 반복 작업을 트랜잭션이라고 합니다. 각 작업은 데이터를 공유하지 않습니다.
![Transactional](https://html.cdn.contentraven.com/crcloud/crscorm/uploads/uipath_lms_11218/encryptedfile/149710/v3.0/scormcontent/assets/OhMEo4I89IVkOqAK_mIQa-UNPDYqPSY-e.jpg)

## Robotic Enterprise Framework

일반적으로 프레임 워크는 프로세스를 설계하는 데 도움이되는 템플릿입니다. 기본적으로 프레임 워크는 프로젝트 구성 데이터, 강력한 예외 처리 체계, 모든 예외에 대한 이벤트 로깅 및 관련 트랜잭션 정보를 저장하고 읽고 쉽게 수정할 수있는 방법을 제공해야합니다.

REframework는 State machine으로 구현했습니다.

REframework는 4가지 주요 상태(State)가 있습니다.

**Initial State**

프로세스가 시작되는 곳입니다. 프로세스를 시작하기위한 모든 전제 조건이 제자리에 있는지 확인하기 위해 프로세스가 설정을 초기화하고 응용 프로그램 확인을 수행하는 작업입니다.

**Get Transaction Data State**

다음 Transaction item을 가져옵니다. Queue item 또는 컬렉션의 모든 item이 될 수 있습니다. 트랜잭션 item은 기본적으로 Queue item이지만 필요에 따라 쉽게 변경할 수 있습니다. 처리 할 item이 없을 때 개발자가이 상태를 종료하기위한 조건을 설정해야하는 상태이기도합니다.

**Process Transaction State**

Get Transaction Data State에서 가져온 데이터를 가지고 처리합니다.

**End Process State**

프로세스 종료.

## REFramework의 주요기능

1. Setting
2. Logging
3. Business Exception & Application Exception 관리

## Dispatcher & Performer

REFramework는 다른 유형의 데이터 소스와 함께 사용할 수 있지만 Orchestrator 큐와의 강력한 통합을 제공합니다. 큐가 사용될 때 트랜잭션 항목의 우선 순위 및 마감일을 정의하고 실패한 트랜잭션의 재시도 시도를 추적 할 수 있습니다.

큐를 사용하면 프로세스를 두 가지 주요 단계로 나누는 Dispatcher & Performer라는 실행 패턴을 사용할 수 있습니다.

처리할 데이터를 dispatch후 Queue에 추가하고, Queue에서 Item을 검색하며 처리를 Performing합니다. Performing을 REFramework로 만듭니다.

#### Dispatcher

Dispatcher는 트랜잭션 항목을 Orchestrator Queue로 Push하는 데 사용되는 프로세스입니다. 하나 이상의 소스에서 데이터를 추출하고이를 사용하여 Performer 로봇이 처리 할 큐 항목을 작성합니다.

Dispatcher 패턴 사용의 주요 장점은 여러 로봇간에 data item 처리를 분할 할 수 있다는 것입니다.

#### Performer

Performer는 오케 스트레이터 큐에서 트랜잭션 item을 가져와 회사에서 필요에 따라 처리하는 데 사용되는 프로세스입니다.

처리 된 각 항목에 대해 오류 처리 및 재시도 메커니즘을 사용합니다.

reference : UIPATH academy
