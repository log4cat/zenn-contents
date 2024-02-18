---
title: "ネスペ午前Ⅱ対策メモ"
emoji: "✏"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [情報処理技術者試験,ネットワークスペシャリスト]
published: false
---

NWの午前Ⅱ試験で直前に読む用のメモ。


## 凡例

関係ある過去問がある場合は以下の表記をする。
(H24h): 平成24年春期
(R04a): 令和4年秋期

## L2

### GARP(Gratuitous ARP)



### STP

IEEE 802.1D
BPDU(Bridge Protocol Data Unit): 制御フレーム

#### RSTP(Rapid Spanning Tree Protocol)

[IEEE 802.1w](https://www.ieee802.org/1/pages/802.1w.html)

#### MSTP(Multiple Spanning Tree Protocol)

複数のVLANを一つにまとめた単位でスパニングツリーを実現するプロトコル(R05h)

## L3

### IPv6

#### ICMPv6

IPv6でMACアドレスを解決するプロトコル(R05h)

#### DHCPv6

IPv6でホストにIPアドレスを割り当てるプロトコル


### ルーティングプロトコル

#### IGP

AS内で使用される。

##### RIP1(Routing Information Protocol)

ディスタンス・ベクタ・アルゴリズム
定期的に経路を確認する。
UDPのブロードキャスト。

##### RIP2

##### OSPF(Open Shortest Path First)

エリアの概念を取り入れたリンクステート方式(R05h)
自律システム(AS)内で使用する。
トポロジ変更や障害を検出するとLSAの配信によって動的にルートが変更される。
コスト値に基づいて最適な経路を決定する。
負荷分散可能。
ネットワークをエリアに分割し，エリアをバックボーンで結ぶ。

#### EGP(Exterior Gateway Protocol)

AS間で使用される。

##### BGP(Border Gateway Protocol)

パスベクトル型ルーティング・プロトコル

メッセージ
- OPEN: セッション開始する。(AS番号，ルータID，BGPバージョン)
- KEEPALIVE: 生存確認(応答はしない)
- UPDATE: 経路情報を更新(新規，削除)する。
- NOTIFICATION: 切断やエラー検知を通知する。

データベース
- BGPネイバーテーブル
- BGPテーブル
- ルーティングテーブル

状態
- Idle
- Connect
- Active
- Open Sent
- Open Confirm
- Established

##### EGP

使われていない。
名前がややこしい。

### IPsec

ネットワーク層の暗号化

#### AH(Authentication Header)

#### ESP(Encapsulating Security Payload)

オリジナルIPヘッダーからESPトレーラーまでを暗号化する。

#### SA

#### ISAKMP

## L4

### TCP

#### フロー制御

受信側のウインドウサイズを超えないように送信データをコントロールする。

#### 輻輳制御

ネットワークのキャパシティを超えないように送信データをコントロールする。

##### CUBIC

パケットの破棄を元に決定。

##### BBR(Bottleneck Bandwidth and Round-trip propagation time)

TCPの輻輳制御アルゴリズム(R05h)
Googleが開発したTCPの輻輳制御アルゴリズム

#### MTU(Maximum Transmission Unit)

データリンク層で1フレームが転送できる最大のデータサイズ。
イーサネットでは1500オクテット。

MTUが1500オクテットの場合，TCPのデータ長は最大1460オクテット。
MTUからIPヘッダー(20)とTCPヘッダー(20)を除く。
MACヘッダーとFCSはMTUの外。

## 無線通信

### OFDM(Orthogonal Frequency Division Multiplexing)

直交周波数分割多重。
データ信号を複数のサブキャリアに分割し，各サブキャリアが互いに干渉しないように配置する方式。(R05h)

### CCK(Complementary Code Keying)

相補型符号変調。
IEEE802.11bで採用されている。

### CDM(Code Division Multiplexing)

複数の通信を同じ周波数帯上で同時に送信するための技術。
受信側でユーザーごとに異なるコードで復調できる。

### TDM(Time Division Multiplexing)

時分割多重化。
一つの伝送路で複数の異なる信号を短い単位時間(タイムスロット)で細切れにして送ることで通信を多重化する。

## IoT

### CoAP(Constrained Application Protocol)

IoT向けのアプリケーション層のプロトコル
パケット損失が発生しやすいネットワーク環境での，小電力デバイスの通信に向いている。
UDPで再送処理のサポートがある。

### MQQT(Message Queuing Telemetry Transport)

テキストベースのプロトコルであり，100文字程度の短いメッセージの通信に向いている。
