---
title: "운영체제 요약 - 12. 대용량 저장장치 구조"
last_modified_at: 2021-04-07
show_date: true
classes: wide
excerpt: ""
categories:
  - operating_system
---

## 12.1. 대용량 저장장치의 개관

### 12.1.1. 자기 디스크(Magnetic Disks)
각 디스크의 플래터(platter)는 원형 평판 모양이다. 
플래터의 양쪽 표면은 자기 물질로 덮여 있다. 
정보를 플래터 상에 자기적으로 기록하여 저장한다. 

<figure style="width: 550px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/1.png' }}" alt=""> 
</figure> 

읽기/쓰기 헤드는 모든 플래터의 각 표면 바로 위에서 움직인다. 
헤드는 모든 헤드를 한꺼번에 이동시키는 디스크 암(disk arm)에 부착되어 있다. 

플래터의 표면은 원형인 트랙(track)으로 논리적으로 나뉘어져 있고, 이것은 다시 섹터(sector)로 나뉘어져 있다. 
동일한 암 위치에 있는 트랙의 집합은 하나의 실린더(cylinder)를 형성한다. 

디스크가 사용 중일 때, 드라이브 모터는 고속으로 회전한다. 
이는 분당 회전수(RPM) 단위로 표현된다. 

디스크 속도는 두 가지 구성요소로 이루어진다. 
전송률(transfer rate)는 드라이브와 컴퓨터간의 데이터 흐름 비율이다. 

임의 접근 시간(random access time)이라고도 하는 위치 잡기 시간(positioning time)은 
원하는 실린더로 디스크 암이 움직이는 탐색 시간(seek time)과 원하는 섹터로 디스크 헤드가 회전하는 데 필요한 시간인 회전 지연(rotational latency)으로 구성된다. 

디스크 드라이브는 입출력 버스라고 하는 회선들의 집합에 의해 컴퓨터에 장착된다. 
ATA(Advanced Technology Attachment), SATA(Serial ATA), USB(Universal Serial Bus) 등 여러 종류의 버스가 이용 가능하다. 

버스 상에서 데이터 전송은 제어기(controller)라고 하는 특별한 전자 처리기에 의해 수행된다. 
호스트 제어기는 버스의 컴퓨터 쪽 끝에 있다. 
디스크 제어기는 각각의 디스크 드라이브에 내장되어 있다. 

디스크 입출력 연산을 수행하기 위해, 컴퓨터는 호스트 제어기에 명령을 넣는다. 
이어 호스트 제어기는 메세지를 통해 명령을 디스크 제어기로 보내고, 이는 명령을 수행하기 위해 디스크 드라이브 하드웨어를 작동시킨다. 

보통 디스크 제어기는 내장 캐시를 가지고 있다. 
디스크 드라이브에서의 데이터 전송은 캐시와 디스크 표면 사이에서 일어나고, 데이터는 빠른 전자 속도로 캐시와 호스트 제어기 사이에서 전송된다. 

### 12.1.2. 반도체 디스크(Solid-State Disk)
SSD는 하드 디스크와 같은 특성을 갖고 있지만, 기계적 이동 부품이 없기 때문에 더 신뢰할 수 있으며, 탐색이나 지연 시간이 없기 때문에 속도가 더 빠르다. 
전력 소모도 적으나, 비싸고 수명이 상대적으로 짧다. 

SSD는 디스크 헤드가 없기 때문에, 일반적으로 디스크 스케줄링 알고리즘이 잘 적용되지 않는다(FCFS나 변형을 사용).  
그러나 처리량과 포맷팅 등은 일반적 디스크 특성과 부합된다. 

### 12.1.3. 자기 테이프(Magnetic Tapes)
디스크보다 무작위 접근이 매우 느려, 주로 예비용이나 자주 사용하지 않는 정보의 저장에 사용된다. 

## 12.2. 디스크 구조
자기 디스크들은 크기가 일정한 논리 블록(logical block)들의 일차원 배열처럼 취급된다. 
정보 전송은 항상 이 블록 단위로만 이루어지며, 각 논리 블록에는 주소가 붙여진다. 

논리 블록들을 가장 바깥쪽 실린더의 첫 번째 트랙의 첫 번째 섹터부터 매핑해가면, 
논리 블록 번호를 실린더 번호, 트랙 번호, 섹터 번호로 변환할 수 있다. 

하지만 이 변환에는 어려움이 있다. 
대부분의 디스크에는 결함이 생긴 섹터가 있으나, 매핑이 이를 다른 섹터로 진행하여 결함을 숨긴다. 
또한 일부 디스크는 트랙당 섹터의 수가 일정하지 않다. 
트랙이 디스크의 중심으로부터 멀어질 수록 트랙은 길이가 더 길어져 더 많은 섹터를 가질 수 있다. 

고정 선형 속도(CLV, Constant Linear Velocity)를 사용하는 장치(CD-ROM, DVD-ROM)에서는 트랙당 비트의 밀도가 일정하다. 
여기에서 헤드는 바깥쪽에서 안쪽 트랙으로 이동하면서, 헤드 아래를 통과 하는 데이터의 비율을 맞추기 위해 회전 속도를 높인다. 

하드 디스크는 고정 각 속도(CAV, Constant Angular Velocity)를 사용한다. 
이는 디스크의 회전 속도를 일정하게 유지하고, 안쪽 트랙에서 바깥쪽 트랙으로 갈수록 비트의 밀도를 줄인다. 

## 12.3. 디스크 부착(Dist Attachment)

### 12.3.1. 호스트 부착 저장장치(Host-Attached Storage)
호스트에 부착된 저장장치는 로컬 입출력 포트를 이용하여 접근된다. 
데스크톱 컴퓨터는 ATA, SATA 등을 사용한다. 
고성능 워크스테잉션과 서버는 일반적으로 광섬유 채널(FC)과 같은 더 정교한 입출력 구조를 사용한다. 

### 12.3.2. 네트워크에 부착된 저장장치(Network-Attached Storage)
네트워크에 부착된 저장장치(NAS)는 데이터 네트워크를 통해 원격으로 접속되는 저장장치이다. 
클라이언트는 유닉스의 NFS 또는 윈도우의 CIFS 같은 원격 프로시저 콜(RPC)을 통해 네트워크에 부착된 저장장치를 접근한다. 

원격 프로시저 콜은 IP 네트워크 상의 TCP 또는 UDP를 통해 전달되는데, 이 네트워크는 보통 클라이언트에게 모든 데이터를 전송하는 같은 LAN(Llocal-Area Network)이다. 
따라서 NAS를 하나의 저장장치 접근 프로토콜이라고 생각하기 쉽다. 
NAS는 보통 RPC 인터페이스를 구현한 소프트웨어를 가진 RAID(12.7절)로 구현된다. 

iSCSI는 네트워크로 연결된 저장장치 프로토콜이다. 이것은 SCSI프로토콜을 전송하기 위해 IP 네트워크를 사용한다. 
따라서 SCSI 케이블들이 아닌 네트워크가 호스트들과 호스트들의 저장장치를 연결한다. 

### 12.3.3. SAN(Storage-Area Network)
네트워크에 부착된 저장 시스템의 단점 중 하나는 저장장치의 입출력 연산 시 데이터 네트워크의 대역폭을 소비하는 점이고, 
이로 인해 네트워크 통신의 지연을 증가시키는 것이다. 

SAN은 서버들과 저장장치 유닛들을 연결하는 사유 네트워크(네트워크 프로토콜을 사용하지 않고 저장장치 프로토콜을 사용)이다. 
여러 호스트와 저장장치가 같은 SAN에 부착될 수 있고, 저장장치는 동적으로 호스트에 할당될 수 있다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/2.png' }}" alt=""> 
</figure> 

SAN 내부 연결에는 iSCSI, FC가 사용될 수 있다. 
다른 연결 구조는 InfiniBand 가 있고, 이는 서버들과 저장장치들을 연결하는 고속의 내부연결망을 지원하는 하드웨어와 소프트웨어를 제공한다. 

## 12.4. 디스크 스케줄링(Disk Scheduling)
디스크 스케줄링은 접근시간(탐색시간, 회전 지연)과 단위 시간당 전송되는 바이트 수를 나타내는 디스크 대역폭을 향상시킬 수 있다. 

### 12.4.1. 선입 선처리 스케줄링(FCFS, First-Come-First-Served Scheduling)
디스크 큐에 먼저 들어온 입출력 요청을 먼저 처리한다. 

### 12.4.2. 최소 탐색 시간 우선 스케줄링(SSTF, Shortest-Seek-Time-First Scheduling)
SSTF는 현재 헤드 위치에서 탐색 시간이 가장 적은, 즉 가장 가까운 요청을 먼저 처리한다. 

### 12.4.3. SCAN 스케줄링
SCAN에서는 디스크 암이 디스크의 한 끝에서 시작하여 다른 끝으로 이동하며, 가는 길에 있는 요청을 모두 처리한다. 
다른 한쪽 끝에 도달하면(또는 그쪽 방향에 더이상 대기하고 있는 요청이 없으면), 역방향으로 이동한다. 
따라서 헤드는 디스크 양쪽을 계속해서 가로지르며 왕복한다. 

<figure style="width: 450px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/3.png' }}" alt=""> 
</figure> 

### 12.4.4. C-SCAN 스케줄링
C-SCAN(Circular-SCAN)은 SCAN의 변형이다. 
한쪽 방향으로 헤드를 이동해 가면서 요청을 처리하지만, 한쪽 끝에 다다르면 디스크의 시작 지점으로 이동한다. 
시작 지점으로 돌아가는 동안 어떤 요청도 처리하지 않는다. 
C-SCAN은 실린더들을 마지막 실린더가 처음 실린더와 맞닿은 원형 리스트로 간주한다. 

<figure style="width: 450px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/4.png' }}" alt=""> 
</figure> 

### 12.4.5. LOOK 스케줄링
헤드를 디스크의 끝에서 끝으로 이동하는 것이 아니라, 그 방향에서 기다리는 요청이 없으면 헤드의 이동 방향을 즉시 반대로 바꾼다. 
이는 한 방향으로 계속 움직이기 전에 미리 요구가 있는지 확인한다. 

<figure style="width: 450px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/5.png' }}" alt=""> 
</figure> 

### 12.4.6. 디스크 스케줄링 알고리즘의 선택
어느 스케줄링 알고리즘을 사용하든, 그 성능은 요청의 형태와 횟수에 좌우된다. 
디스크 서비스는 파일을 디스크 공간에 어떻게 배열했느냐 와도 상관 관계가 많다. 
디렉터리와 색인 블록의 위치 또한 중요하다. 

스케줄링 알고리즘을 디스크 드라이브에 내장된 제어기 하드웨어 내에 구현할 수도 있다. 
운영체제가 제어기에게 요청을 보내면, 제어기는 이를 스케줄하여 탐색 시간과 회전 지연을 최적화 한다. 

성능만을 디스크 입출력에서 고려해야 할 유일한 사항이라면 디스크 스케줄링을 모두 디스크 하드웨어가 할 수도 있다. 
하지만 성능 이외에도 작업들의 우선순위라든가, 디렉터리가 파일보다 먼저 디스크에 쓰여지는 신뢰성 등 고려할 것이 많다. 

## 12.5. 디스크 관리

### 12.5.1. 디스크 포맷팅(Disk Formatting)
디스크는 자료를 저장하기 전에 섹터들로 나누어져야 하며, 이 과정을 저수준 포맷팅(low-level formatting) 또는 물리적 포맷팅이라고 부른다. 
저수준 포맷팅은 섹터를 구분하기 위해 디스크를 적절한 자료구조로 채우는 것이며, 섹터의 자료구조는 보통 헤더(header), 자료 구역(보통 512B data area), 트레일러(trailer)로 구성된다. 

헤더와 트레일러는 섹터 번호와 오류 수정 코드(ECC, Error-Correcting Code)와 같은 정보를 가지고 있다. 
ECC는 한두 개의 비트만 잘못되었을 때는 이를 교정할 수 있는 정보를 가지고 있다. 

디스크 제어기가 섹터에 데이터를 쓸 때, 데이터 값으로부터 ECC를 구해서 같이 기록한다. 
나중에 이 섹터가 읽혀질 때, 그 데이터로부터 ECC가 다시 계산되고 이것이 저장되었던 ECC값과 비교된다. 
비교된 값들이 서로 다르면, 섹터에 문제가 생긴 것이다. 

디스크에 파일을 저장하기 전에, 운영체제는 디스크에 운영체제의 자료구조를 기록한다. 
먼저, 디스크를 여러 실린더로 이루어지는 파티션(partition)으로 나눈다. 
운영체제는 각 파티션을 각각 별도의 디스크처럼 취급한다. 

그 후 논리적 포맷(logical formatting), 즉 파일 시스템을 만든다. 
운영체제는 FAT, inode, 가용 공간 정보, 초기의 빈 디렉토리 구조 등을 포함한 파일 시스템 자료구조를 디스크에 저장한다. 

효율성을 위해 대부분의 파일 시스템들은 블록들을 클러스터(cluster)라고 하는 묶음으로 그룹화한다. 
디스크 입출력은 블록 단위로 수행되나, 파일 시스템의 입출력은 클러스터 단위로 수행된다. 
입출력이 순차 접근은 많은 반면, 임의 접근은 적기 때문이다. 

어떤 운영체제는 디스크 파티션을 운영체제의 파일 시스템없이 논리 블록들의 배열처럼 사용할 수 있도록 해주기도 한다. 
이와 같은 논리 블록들의 배열을 raw disk라 하고, 이 배열에 대한 입출력을 raw 입출력이라 한다. 
데이터베이스 시스템과 같은 특정 응용 프로그램은 자료가 저장되어 있는 위치를 자신이 제어하기에 raw 입출력을 더 선호한다. 

### 12.5.2. 부트 블록(Boot Block)
대부분의 시스템에서 ROM에는 부트스트랩 프로그램을 디스크로부터 적재하는 일만 하는 부트스트랩 로더(bootstrap loader) 프로그램을 저장한다. 
따라서 부트스트랩 프로그램은 쉽게 변경이 가능하다(새로운 버전을 디스크에 저장). 

부트스트랩 프로그램은 디스크의 고정된 위치인 부트 블록에 저장된다. 
부트 파티션을 가지고 있는 디스크는 부트 디스크 또는 시스템 디스크라 불린다. 

부트스트랩 로더는 디스크 제어기에게 부트스트랩 프로그램을 메모리에 올리도록 지시하고, 그 프로그램을 실행한다. 
부트스트랩 프로그램은 디스크 내에 저장되어있는 운영체제 커널을 찾고 그것을 시작한다. 

Windows에서는 부트 파티션이라는 한 파티션에 운영체제와 디바이스 드라이버들을 저장한다. 
Windows의 부트 코드는 하드디스크에 첫 번째 섹터에 저장된다(master boot record, MBR). 
MBR에는 부트 코드뿐만 아니라 하드디스크에 대한 파티션 테이블과 어느 파티션에서 부트가 되어야 하는가에 대한 표시도 있다. 

<figure style="width: 350px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/6.png' }}" alt=""> 
</figure> 

ROM의 코드는 시스템이 MBR에 있는 부트 코드를 읽도록 한다. 
시스템이 부트 파티션을 찾으면, 해당 파티션의 첫 번째 섹터(부트 섹터)를 읽는다. 
그리고 여러 서브시스템과 시스템 서비스들을 적재하는 등의 부트 프로세스의 남은 일을 재개한다. 

### 12.5.3. 손상된 블록(Bad Blocks)
일반적으로 손상된 섹터는 아래와 같이 처리된다. 
1. 운영체제가 논리 블록 87을 읽으려고 시도한다. 
2. 디스크 제어기가 ECC를 계산하고 손상 여부를 운영체제에게 알린다. 
3. 시스템이 재부팅될 때, 특별한 명령어가 제어기로 하여금 손상된 섹터를 예비 섹터로 교체하도록 한다. 
4. 그 후 논리 블록 87이 요청될 때마다, 변경된 새로운 섹터 주소로 간다. 

제어기가 재배치를 처리하면 디스크 스케줄링 알고리즘에 의한 최적화가 무산될 수 있다. 
따라서 일부 디스크들은 포맷팅할 때, 예비 섹터를 각 실린더마다 배치하고 예비 실린더도 배치한다. 
그리고 섹터가 손상되면, 가능한 동일한 실린더에서 예비 섹터를 찾아 대체한다. 

예비 섹터를 관리하는 다른 방안으로서 일부 제어기는 섹터 밀어내기(sector sliping)을 한다. 
논리 블록 17에 결함이 생겼고, 첫 번째 예비 블록이 202 다음에 있다고 하면, 
섹터 17~202를 한 칸씩 18~예비 블록으로 밀어낸다. 

## 12.6. 스왑 공간 관리
스왑 공간 관리는 운영체제가 수행하는 또 다른 하위 수준의 작업이다. 
가상 메모리는 디스크를 주 메모리의 확장된 공간으로 사용한다. 

### 12.6.1. 스왑 공간 사용
시스템에 따라서는 코드와 데이터 전체, 즉 프로세스 이미지(image) 전체를 스왑 공간에 가지고 있도록 할 수도 있다. 
페이징을 사용하는 시스템은 주 메모리에서 밀려나는 페이지들을 이 공간에 저장시킬 수도 있다. 

몇몇 운영체제는 파일 및 전용 스왑 파티션을 포함하여 여러 개의 스왑 공간을 가질 수 있다. 
이들 스왑 공간은 보통 여러 다른 디스크에 할당되어 페이징와 스와핑에 의한 입출력 부하를 입출력 대역폭에 고르게 분산시킨다. 

### 12.6.2. 스왑 공간 위치
보통 스왑 공간은 별개의 디스크 파티션에 둔다. 
그리고 이 파티션에는 일반 파일/디렉터리가 저장되지 않고, 별도의 스왑 관리 루틴에 의해 스와핑을 하는 데에만 사용된다. 
스왑 관리 루틴은 공간 효율성보다는 속도 효율성을 최적화하기 위한 알고리즘을 사용한다. 

### 12.6.3. 스왑 공간 관리: 예
Solaris의 스왑 공간은 anonymous 메모리 페이지(프로세스의 스택, 힙, 초기화되지 않은 데이터 영역 등에 할당된 메모리)들을 위한 공간으로서의 역활만 한다. 
Solaris는 가상 메모리가 처음 생성되었을 때가 아닌 한 페이지가 물리 메모리에서 쫓겨날 때, 스왑 공간을 할당한다. 

Linux는 스왑 공간을 anonymous 메모리에만 사용한다. 
Linux는 한개 이상의 스왑 영역을 허용한다. 
스왑 영역은 파일 시스템 혹은 전용 스왑 파티션으로 있을 수도 있다. 

## 12.7. RAID 구조
시스템은 많은 수의 디스크를 가질 수 있고, 그 시스템이 병렬적으로 운영된다면 데이터 읽기, 쓰기 비율을 향상시킬 수 있다. 
더욱이 중복 정보가 여러 디스크에 저장되기 때문에 데이터 저장의 신뢰성을 높일 수 있다. 
RAID(Redundant Array of Inexpensive Disk or Redundant Array of Independent Disk)라 불리는 디스크 구성 기술은 성능과 신뢰성 이슈를 해결하는데 쓰인다. 

### 12.7.1. 중복으로 신뢰성 향상
신뢰성의 문제 해결 방안은 중복을 허용하는 것이다. 
보통 필요하지 않는 정보지만, 디스크 오류 시 분실된 정보를 재구축에 사용될 수 있는 정보를 저장한다. 

중복의 가장 간단한 방법은 모든 디스크의 복사본을 만드는 것이다. 
이 기술은 미러링(mirroring)이라고 불린다. 
하나의 논리 디스크는 두 개의 물리 디스크로 구성되고, 모든 쓰기 작업은 두 디스크 모두에서 실행된다. 
이러한 결과를 미러드 볼륨이라 한다. 

미러드 시스템은 자연 재해는 해결할 수 없지만 더 높은 신뢰성을 제공해 준다. 
전력 문제는 첫 번째 디스크에 먼저 쓰고, 다음 디스크에 쓰는 해결책을 쓸 수 있다. 
또는 비휘발성 메모리(Nonvolatile RAM, NVRAM) 캐시를 RAID 배열에 둘 수 있다. 

### 12.7.2. 병렬성을 이용한 성능 향상
여러 디스크에 걸쳐 데이터 스트라이핑(data striping)을 사용하여 전송 비율을 향상시킬 수 있다. 
예를 들어 8개의 디스크를 가지고 있다면, i번 디스크에 각 바이트의 i번째 비트를 쓰는 것이다. 
모든 디스크가 읽기나 쓰기 접근에 참여하면, 각 접근은 같은 시간에 하나의 디스크보다 8배의 데이터를 읽을 수 있다. 

스트라이핑을 비트, 블록, 섹터 단위로 할 수 있고, 블록 레벨의 스트라이핑이 가장 일반적이다. 

### 12.7.3. RAID 레벨(RAID Levels)

<figure style="width: 750px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/7.png' }}" alt=""> 
</figure> 

RAID Level 0은 블록 레벨로 스트라이핑을 하는 디스크 구성이며, 미러링이나 패리티(parity) 비트 같은 어떤 중복 정보도 가지고 있지 않다. 
---
RAID Level 1은 디스크 미러링을 사용한다. 원래 디스크 수의 두 배가 필요하다. 
---
RAID Level 2는 메모리 스타일 오류 정정 코드 구조(memory-style error-correcting-code(ECC) organization)으로도 알려져 있다. 
메모리 시스템은 패리티 비트를 사용하여 에러를 검출할 수 있다. 
각 바이트는 자신과 연관된 하나의 패리티 비트를 갖는다. 
이는 한 바이트를 구성하는 8비트 중 값이 1인 비트의 수가 짝수인지 홀수인지를 기록한다. 
따라서 패리티 비트를 통해 바이트가 손상되었는지 알 수 있다. 

RAID 2는 유사하게, 스트라이핑을 사용하는 디스크에 추가적인 디스크를 두고, 여기에 ECC를 저장한다. 
디스크 중 하나가 고장나면, ECC를 읽음으로써 손상된 데이터를 복구한다. 
---
RAID Level 3은 bit-interleaved parity organization이라 불리며, 메모리 시스템과는 달리 한 섹터가 정확히 읽혔는지를 디스크 컨트롤러가 탐지할 수 있다는 사실을 이용하여 level 2의 기능을 향상시킨 것이다. 
따라서 싱글 패리티 비트가 오류 정정은 물론 오류 탐지를 위해 사용될 수 있다. 

비트레벨 스트라이핑을 쓰는 RAID 3의 아이디어는 다음과 같다. 
하나의 섹터가 손상을 입는다면, 정확히 어떤 섹터가 손상을 입었는지 알 수 있따. 
또한 다른 디스크의 섹터로부터 해당 비트의 패리티를 계산함으로써 손상된 섹터의 비트가 1인지 0인지 알 수 있다. 

RAID 3는 RAID 2만큼 좋고, 더 적은 수의 디스크를 사용한다. 
그러나 패리티 기반의 공통적인 문제인 패리티 계산 비용이 있다. 

이를 해결하기 위해, RAID는 패리티를 연산하는 하드웨어를 가지는 하드웨어 컨트롤러를 포함한다. 
패리티 연산을 CPU 대신 RAID가 한다. 
또한 RAID는 NVRAM 캐시를 가지고 있는데, 이는 패리티가 계산되는 동안 블록을 저장하고, 컨트롤러에서부터 디스크까지 쓰기 작업을 버퍼링한다. 
---
RAID Level 4는 block-interleaved parity organization이라 하고, RAID 3와 유사하다. 
블록 단위 스트라이핑을 사용하며, N개의 디스크 블록에 대한 패리티 블록을 다른 디스크에 저장한다. 
---
RAID Level 5는 block-interleaved distributed parity라 불린다. 
RAID 4와 달리 데이터와 패리티를 모든 디스크에 분산시킨다. 
예를 들어 5개의 디스크로 구성된 RAID에서 n번째 블록에 대한 패리티는 (n mod 5) + 1 디스크에 저장된다. 
이는 RAID 4에서 발생 가능한 하나의 패리티 디스크에 대한 과도한 집중을 막을 수 있다. 
---
RAID Level 6는 P + Q 중복 기법이라고도 불린다. 
RAID 5와 유사하지만, 여러 디스크 오류에 대비하여 추가의 중복 정보를 저장한다. 

패리티 비트를 사용하는 대신에 Reed-Solomon Codes 같은 에러 교정 코드가 사용한다. 
RAID 5와 다르게 데이터 4비트마다 2비트의 중복 데이터를 저장한다. 
---

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-02-13-operating_system_12/8.png' }}" alt=""> 
</figure> 

RAID Level 0 + 1과 RAID Level 1 + 0은 신뢰성을 제공하는 RAID 1과 높은 성능을 제공하는 RADI 0을 결합한 것이다. 
일반적으로 RAID 5보다 성능이 좋지만, RAID 1처럼 디스크의 수가 2배 필요하다. 

RAID 0 + 1은 디스크를 스트라이프한 후 미러링한다. 
즉, 그룹이 두개 있으며, 데이터는 그룹 내에서 스트라이프된다. 
이후 그룹이 미러링된다. 

RAID 1 + 0은 미러링을 한 후 스트라이프한다. 
즉, 디스크를 두개씩 그룹짓고, 그룹안에서 미러링된다. 
이후 데이터가 그룹을 거쳐서 스트라이프된다. 

RAID 1 + 0이 그룹이 더 많아 신뢰성이 더 높다? 
예를 들어 RAID 0 + 1은 하나의 디스크가 고장이 난다면, 그 스트라이프는 전부 접근 불가능하다. 
RAID 1+ 0 은 하나의 디스크가 오류나도, 미러드된 또 하나의 디스크와 다른 모든 디스크는 사용가능하다. 

디스크 복구 시 그룹 마다 복구가 되는데, RAID 0 + 1은 그룹 별 디스크 개수가 많아 복구가 더 느리다.
---
이외에도 많은 변형 RAID 기법들이 존재한다. 

RAID의 다른 계층에서 구현, 변형된 형태들도 있다.
* 커널 혹은 시스템 소프트웨어 계층 내에서 저장소 관리 소프트웨어(Volume-management software)가 RAID를 구현할 수 있지만 느리다. 
* HBA(host bus-adapter) 하드웨어에 구현될 수 있다. HBA에 직접 연결된 디스크들만 RAID의 일부가 된다. 
* 저장장치 배열에 구현될 수 있다. 장치 배열들은 다수의 연결을 가지거나 SAN의 일부일 수 있다. 
* 디스크의 가상 디바이스들을 통해 SAN 내부 연결 계층에 구현될 수 있따. 

스냅샷(snapshot)이나 복제와 같은 다른 특징들이 상기의 계층에 구현될 수 있다. 
스냅샷은 마지막 갱신이 일어나기 전의 파일 시스템의 모습이다. 
복제는 중복성과 복구를 위해 자동적으로 분리된 지역에 같은 내용을 쓰는 것이다. 

### 12.7.4. RAID 레벨 선택
복구는 RAID 1이 가장 쉽다. 다른 레벨에서는 오류 디스크의 정보를 복구하기 위해 다른 모든 디스크에 접근해야 한다. 

RAID 0은 데이터 분실이 없을 때 높은 성능을 보인다. 

데이터베이스 같은 곳에는 높은 성능과 신뢰성이 높은 RAID 1 + 0이 자주 사용된다. 
디스크 공간이 부족할 경우 RAID 5도 종종 사용된다. 

### 12.7.5. 확장
RAID의 개념은 테이프나 무선 시스템에 데이터를 브로드캐스트 하는 일에도 적용될 수 있다. 
데이터 브로드캐스트의 경우 데이터의 블록은 작은 유닛으로 나누어지고 패리티 유닛과 함께 브로드캐스팅된다. 
데이터의 일부분을 수신하지 못하더라도 다른 유닛으로부터 복구가 가능하다. 

### 12.7.6. RAID의 문제점들
RAID는 물리적 매체의 오류는 보호하지만, 다른 하드웨어나 소프트웨어 에러는 보호하지 못한다. 
이를 해결하기 위해 Solaris ZFS 파일 시스템은 데이터 일관성 검사를 위해 체크섬(checksum)을 사용한다. 

RAID는 유연성도 부족하다. 
디스크 사용과 요구 조건이 시간에 따라 변화한다. 
이에, ZFS는 디스크 또는 디스크의 파티션을 RAID 집합을 통해 집합적으로 저장장치의 풀을 구성한다. 
이 풀은 하나 또는 그 이상의 ZFS 파일 시스템을 저장할 수 있다. 
결과적으로 저장장치를 사용하는 데 있어 인위적인 제한을 두지 않으며 볼륨 간에 파일 시스템을 재배치하거나 볼륨의 크기를 재조정할 필요가 없다. 

## 12.8. 안정적인 저장장치 구현
안정된 저장장치를 만들려면 각자 독립적으로 고장나는 여러 대의 저장장치에 복사본을 만들어야 한다. 
그리고 저장장치를 갱신하는 동안에 고장이 발생하더라도 모든 복사본인 손상되는 일은 없어야 한다. 

디스크 쓰기는 다음 3가지 결과 중 하나이다. 
1. 성공: 자료가 디스크에 올바르게 기록됨
2. 부분적 실패: 섹터 중 몇 개만이 새로운 자료가 기록되었고, 고장 당시 기록 중이던 섹터는 손상되었을 수도 있다. 
복구 프로시저를 불러 블록이 일관된 상태를 갖도록 복원해야한다. 
3. 완전 실패: 디스크가 쓰기 시작 전에 고장나서 디스크의 데이터가 그대로 있다. 

