컴퓨터 운영구조 및 운영체제 핵심 이론 요약

본 정리는 하드웨어 아키텍처의 물리적 제약 조건을 소프트웨어가 어떻게 극복하고 자원을 배분하는지에 대한 논리적 연결성을 중심으로 기술합니다.

1. 컴퓨터 시스템 구조 (Computer Architecture)

근거: 프로그램의 실행 속도는 CPU의 연산 능력뿐만 아니라, 데이터가 이동하는 경로(Bus)와 저장 장치의 속도 차이(Latency)에 의해 결정됩니다.

1.1 폰 노이만 구조와 시스템 버스

- 내장 프로그램 방식: 명령어와 데이터를 구별 없이 메모리에 저장하여 CPU가 순차적으로 처리.

- 시스템 버스: 데이터 버스(Data), 주소 버스(Address), 제어 버스(Control)로 구성.

- 병목 현상: CPU와 메모리 간의 속도 차이로 인해 발생하는 성능 저하(Von Neumann Bottleneck).

1.2 명령어 실행 사이클 (Instruction Cycle)

- Fetch: PC(Program Counter)가 가리키는 주소에서 명령어를 가져와 IR(Instruction Register)에 저장.

- Decode: 제어 장치가 명령어를 해석.

- Execute: ALU를 통해 연산 수행 및 결과 저장.

- Interrupt Check: 하드웨어/소프트웨어적 인터럽트 발생 여부 확인.

2. 메모리 계층 및 관리 (Memory Management)

- 근거: 고속의 프로세서와 저속의 저장장치 사이에서 데이터 접근의 '지역성(Locality)'을 활용해야만 전체 시스템의 처리율(Throughput)을 최적화할 수 있습니다.

2.1 메모리 계층 구조

- 레지스터 → 캐시(L1~L3) → 주기억장치(RAM) → 보조기억장치(SSD/HDD)

- Locality: 시간 지역성(최근 참조 데이터 재참조)과 공간 지역성(인접 데이터 참조) 원리.

2.2 가상 메모리 (Virtual Memory)

- 목적: 물리 메모리 크기 이상의 프로그램을 실행하고, 프로세스 간 메모리 보호를 수행.

- Paging: 물리 메모리를 고정 크기(Page Frame)로 나누어 외부 단편화 해결.

- Demand Paging: 필요한 페이지만 메모리에 적재하여 I/O 비용 감소.

- Page Fault: 참조 페이지가 메모리에 없을 때 발생하는 인터럽트로, OS가 디스크에서 페이지를 가져옴.

3. 프로세스 및 스레드 관리 (Process & Thread)

- 근거: 멀티프로그래밍 환경에서 CPU 이용률을 극대화하기 위해 '문맥 교환(Context Switching)'을 통한 시분할 처리가 필수적입니다.

3.1 프로세스 제어 블록 (PCB)

- OS가 프로세스를 관리하기 위해 필요한 정보를 저장하는 자료구조.

- PID, 프로세스 상태, PC, 레지스터 상태, 메모리 관리 정보 포함.

3.2 CPU 스케줄링

- 선점형(Preemptive): RR(Round Robin), SRTF, Multi-level Queue. (현대 OS 표준)

- 비선점형(Non-preemptive): FCFS, SJF, Priority.

3.3 프로세스 동기화

- Race Condition: 둘 이상의 프로세스가 공유 자원에 동시 접근할 때 데이터 불일치가 발생하는 현상.

- 해결책: 상호 배제(Mutual Exclusion), 진행(Progress), 한정 대기(Bounded Waiting) 조건을 만족해야 함 (Semaphore, Mutex, Monitor).

4. 교착 상태 (Deadlock)

- 근거: 제한된 자원을 비선점 방식으로 점유하려는 프로세스 간의 환형 대기는 시스템 전체의 정지(Hang)를 초래하므로 엄격한 관리가 필요합니다.

4.1 발생 4대 조건 (Coffman's Conditions)

- 상호 배제(Mutual Exclusion): 자원은 한 번에 한 프로세스만 사용 가능.

- 점유와 대기(Hold and Wait): 자원을 가진 상태에서 다른 자원을 추가 대기.

- 비선점(No Preemption): 강제로 자원을 뺏을 수 없음.

- 환형 대기(Circular Wait): 프로세스 간 대기 관계가 원형을 이룸.

4.2 대응 전략

- 예방(Prevention): 4조건 중 하나를 원천적으로 거부.

- 회피(Avoidance): 은행원 알고리즘(Banker's Algorithm) 등을 통해 안전 상태 유지.

- 탐지 및 복구(Detection & Recovery): 주기적 체크 후 프로세스 강제 종료 또는 자원 회수.

5. 입출력 및 파일 시스템 (I/O & File System)

- 근거: I/O 장치는 CPU에 비해 매우 느리므로, CPU의 간섭을 최소화하는 하드웨어 가속 방식이 필요합니다.

5.1 데이터 전송 방식

- Programmed I/O: CPU가 상태 비트를 계속 확인(Polling). 비효율적.

- Interrupt-driven I/O: 작업 완료 시 장치가 CPU에 알림.

- DMA(Direct Memory Access): CPU 개입 없이 메모리와 I/O 장치 간 직접 데이터 전송. 블록 단위 처리 후 인터럽트 1회 발생.

5.2 파일 할당 방식

- 연속 할당: 물리적으로 인접한 블록에 저장 (빠르나 외부 단편화 발생).

- 불연속 할당: 연결 리스트(Linked Allocation) 또는 인덱스 블록(Indexed Allocation, i-node) 사용.
