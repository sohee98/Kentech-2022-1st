# Wireless Network

## Protocal & MAC (Medium Access Control)

### Slots 

<img src="md-images/image-20220308103503995.png" alt="image-20220308103503995" style="zoom:67%;" />

* A slot is either Idle, Success or Collision
  * Idle : No arrival during the previous slot time
  * Success : Exactly one arrival
  * Collision : More than one arrival





## Wireless Links & WLAN (Wireless Local Area Network)

#### Wireless Characteristics

* Signal quality of wireless links is much worse than that of wired links

* Rout cause : Attenuation of wireless medius is 
  *  <img src="md-images/image-20220308103734904.png" alt="image-20220308103734904" style="zoom:50%;" />



#### IEEE 802.11 WLAN Standard

<img src="md-images/image-20220308104140589.png" alt="image-20220308104140589" style="zoom: 50%;" />

* Transmission Rates

  * AMC: Adaptive Modulation & Coding

  <img src="md-images/image-20220308104740973.png" alt="image-20220308104740973" style="zoom: 80%;" />



#### Topology

* Infrastructure Mode
  * all packets pass through the **Access Point (AP)**
  * No direct communications between **Mobile Stations (MS**, or client/host)
* Ad-hoc mode
  * Peer-to-peer (station-to-station) direct communications

#### 802.11 Architecture

<img src="md-images/image-20220308105018932.png" alt="image-20220308105018932" style="zoom:80%;" />



#### Collision Detection

* In wired links, we can be almost sure that a frame is delivered successfully in case of no collision -> ACK is not necessary if CD is equipped

* Wireless CD is impossible

  ![image-20220308105338987](md-images/image-20220308105338987.png)

#### Reliability & ACK(acknowledgement code) 응답 코드

1. How do you know a frame is successfully transmitted? => use ACK

   <img src="md-images/image-20220308105610392.png" alt="image-20220308105610392" style="zoom: 50%;" />

2. No ACK => **Retransmission** 재전송

* ACK should be transmitted before any other framesS



#### Carrier Sensing 매체 사용중 감지 (유 / 무선)

<img src="md-images/image-20220308105820697.png" alt="image-20220308105820697" style="zoom:80%;" />

#### Hidden / Exposed Terminal

* **Hidden Terminal** Problem : C cannot sense B->A transmission and attempts to send a frame (더 중요함)
  * <img src="md-images/image-20220308110256848.png" alt="image-20220308110256848" style="zoom:67%;" />
* Exposed Terminal Problem : 
  * C can send to D without disturbing A->B transmission
  * But, C thinks the medium is busy
  * <img src="md-images/image-20220308110152256.png" alt="image-20220308110152256" style="zoom:67%;" />



#### RTS/CTS & Virtual CS

* RTS (송신 요구) \- 송신측이 수신측에게 본격적인 데이터를 전송할 의사가 있음을 알리는 신호
* CTS (송신가능) - 수신측이 송신측에게 데이터를 받을 준비가 되어서 전송 허락 신호

<img src="md-images/image-20220308110357746.png" alt="image-20220308110357746" style="zoom:80%;" />

> 송신 노드는 채널이 빈 것(idle)을 알고난 후, DIFS 만큼 기다린 후, RTS 송출
> 수신 노드는 RTS 수신 후, SIFS만큼 기다린 후, CTS 송출
> 송신 노드는 SIFS 만큼 기다린 후, 데이터 송출 시작
> 수신 노드는 SIFS 만큼 기다린 후, 확인응답(ACK) 송출

#### RTS/CTS & Hidden Terminal

<img src="md-images/image-20220308111152162.png" alt="image-20220308111152162" style="zoom:80%;" />



#### Atomicity 원자성

> 모두 성공하거나 또는 모두 실패해야함

* DATA & Control frames - ACK, RTS, CTS
* Atomic transaction 
  * Transmissions including both DATA and its ACK
  * RTS-CTS-DATA-ACK
* How to guarantee Atomicity?
  * Use **different CS (Carrier Sense) time**



#### Inter-Frame Space (IFS)

> Importance of ACK > Data
>
> How to prioritize ACK transmission?

* IFS : Minimum time that the medium should be idle to trigger frame transmission
* **SIFS** (short IFS) : For all immediate response actions (CTS, ACK, Poll response)
* **DIFS** (distributed coordination function IFS) : Used as minimum delay for DATA, RTS frames contending for access
* PIFS (point coordinaion function IFS) : Used by the centralized controller in PCF scheme when issuing polls

> If SIFS < DIFS, then data frames cannot intervene DATA-ACK transaction



#### DCF Design

<img src="md-images/image-20220308112132835.png" alt="image-20220308112132835" style="zoom:80%;" />

* First Carrier Sensing - A new Data frame arrives and the first Carrier Sensing
* First CS = IDLE - Should wait at least DIFS to protect ACK, CTS and etc
* Fist CS = BUSY - Should wait extra in addition to DIFS 
                            - The extra waiting time should be randomized
* Transmission result - Success or Collision

> In the case of collision, should reduce attempt rate

<img src="md-images/image-20220308112531113.png" alt="image-20220308112531113" style="zoom:67%;" />



#### BEB (Binary Exp. Backoff)

* Collision = Overloaded

* Contention Window (CW)

* Backoff delay = Random (0 .. CW)

* At the beginning, CW = CW_min

* For each unseccessful transmission, double CW up to CW_max

  ![image-20220308112844000](md-images/image-20220308112844000.png)



​	<img src="md-images/image-20220308113350497.png" alt="image-20220308113350497" style="zoom:80%;" />



#### DCF - CSMA/CA

> (Carrier Sense Multiple Access / Collision Avoidance)
> collision을 최대한 피하는 방식

<img src="md-images/image-20220308113640569.png" alt="image-20220308113640569" style="zoom: 80%;" />



#### AP Discovery

* To use AP - 1. Discovery 2. Authentication 3. Association
* Scan
  * Discover APs in transmission range 
  * Active scan, Passive scan
* Passive scan
  * Clients listen for “Beacon” frames that AP transmits periodically
* Active scan
  * Clients probe APs by sending “Probe request” frames
  * AP replies with “Probe response” frame
  * “Probe response” frame is approximately same as a Beacon frame



#### Power Saving

* Energy is a scarce resource in battery powered devices
* Network interfaces consume sizable amount of energy

>  How to Conserve Energy?

* Turn off idle network interfaces
* **PSM (Power Saving Mode)**
  * Sleep state
  * Listen
* To Sleep
  * signals to the AP not to send frames
* How to resume communication (Wakeup)?
  * Transmission (Station => AP)
  * Reception (AP => Station)



* Packet Reception 수신 - PSM

  ![image-20220310104104886](md-images/image-20220310104104886.png)

  * AP transmits beacons every beacon interval (-100msec)
    * Defer if channel is busy
    * All PS nodes wake up for beacon reception
  * TIM(Traffic Indication Map)
    * indicate the existence of backlogged unicast traffic to PS nodes
    * PS node may request packet transmission
  * Broadcast packets are announced by a Delivery TIM(DTIM) and are sent immediately afterwards
    * DTIM interval is a multiple of TIM interval

* Delivery in PS Mode

  * If TIM indicates frames buffered

    * STA sends PS-Poll to AP and stays awake to receive data

    ![image-20220310104230342](md-images/image-20220310104230342.png)

  * Learn − U-APSD (Unscheduled Automatic Power Saving Delivery)