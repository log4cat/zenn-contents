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

## ネットワーク

### アーラン

1回線100%を1アーラン。

### 光ファイバー

シングルモード: 速くて長距離に向いている 
マルチモード: 安価で曲げやすい

## L2

### GARP(Gratuitous ARP)

自分自身のIPアドレスを指定したARｐを送る。
IPアドレスの重複を検出できる。
HSRPでの切り替わり通知。

### STP

IEEE 802.1D
BPDU(Bridge Protocol Data Unit): 制御フレーム

#### RSTP(Rapid Spanning Tree Protocol)

[IEEE 802.1w](https://www.ieee802.org/1/pages/802.1w.html)

STPより収束が早い。

#### MSTP(Multiple Spanning Tree Protocol)

複数のVLANを一つにまとめた単位でスパニングツリーを実現するプロトコル(R05h)

### IEEE 802.1Q

送信元MACアドレスとタイプ/長さフィールドの間に入る。
VLANをサポートしない機器でもフレームを転送できる。

### EAP(Extensible Authentication Protocol)

IEEE802.1Xに基づくユーザー認証規格。

- サプリカント: クライアント
- オーセンティケーター: 仲介役(スイッチや無線LAN)
- オーセンティケーションサーバー: RADIUSサーバーなど

- EAP-MD5: ユーザー名とパスワード
- EAP-TLS: デジタル証明書を用いた相互認証
- EAP-TTLS: TLSでサーバー認証した後にトンネル内で各種認証を行う。
- EAP-FAST: EAP-TTLSのようなCisco独自プロトコル

## L3

### IPv4

- 10.0.0.0/8: プライベートIPアドレス(クラスA)
- 172.16.0.0/12: プライベートIPアドレス(クラスB)
- 192.168.0.0/16: プライベートIPアドレス(クラスC)
- 224.16.10.255/8: クラスD。マルチキャスト用。
- 127.0.0.1-127.255.255.255: ループバックアドレス

### ICMP

メッセージ
- Destination Unreachable: ルーティング失敗。
- Redirect: より適切な経路を通知。
- Time Exceeded: フラグメントの再組立て中にタイムアウトが発生
- Source Quench: 受信側バッファ溢れ

### IGMP

マルチキャストグループに関する制御。

- Membership Query: グループへの参加
- Membership Report: グループの状態の通知
- Leave Group: グループから離脱

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
クラスフルな(CIDRを使わない)ため，サブネットマスクを含まない。

##### RIP2

##### OSPFv2(Open Shortest Path First)

エリアの概念を取り入れたリンクステート方式(R05h)
自律システム(AS)内で使用する。
トポロジ変更や障害を検出するとLSAの配信によって動的にルートが変更される。
コスト値に基づいて最適な経路を決定する。
負荷分散可能。
ネットワークをエリアに分割し，エリアをバックボーンで結ぶ。

##### OSPFv3

IPv6用に拡張されたOSPF

[RFC 5340](https://datatracker.ietf.org/doc/html/rfc5340)

#### EGP(Exterior Gateway Protocol)

AS間で使用される。

##### BGP-4(Border Gateway Protocol)

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

[RFC 4271](https://datatracker.ietf.org/doc/html/rfc4271)

##### BGP4+

IPv6などマルチプロトコルに拡張されたBGP

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

## L7

### DNS

レコード
- SOA(Start Of Authority): ゾーンに関する情報
- A(Address): ホスト名 対 IPv4アドレス
- AAAA: ホスト名 対 IPv6アドレス
- CNAME(Canonical NAME): 他の名称へのエイリアス。
- MX: ドメイン名 対 メールサーバーホスト名
- NS(Name Server): ドメイン名 対 DNSサーバーのホスト名
- PTR(PoinTeR): IPアドレス 対 ホスト名
- TXT: コメント，SPF用のメールサーバーのIPアドレス，DKIM用の公開鍵

[RFC 1035](https://datatracker.ietf.org/doc/html/rfc1035#section-3.2.2)
[RFC 3596](https://datatracker.ietf.org/doc/html/rfc3596)

### HTTP

#### Webサービス

HTTP上のSOAPによってソフトウェア同士が通信して，ネットワーク上に分散したアプリケーションプログラムを連携させることができる。(R05h)

#### WebDAV

HTTPを拡張したプロトコルを使って，サーバ上のファイルの参照，作成，削除及びバージョン管理が行える。(R05h)

#### WebFTP

Webブラウザで"ftp://"から始まるURLを指定して，ソフトウェアなどの大きなファイルのダウンロードができる。(R05h)

### SNMP

#### MIB(Management Information Base)

OIDの定義

#### Trap

機器の変化をマネージャーに伝える。。

#### RMON(Remote network MONitoring)

ネットワークのトラフィック管理において，測定対象の回線やポートなどからパケットをキャプチャして解析し，SNMPを使って管理装置にデータを送信する仕組み(R04h)

## 無線通信

### IEEE 802.11

| 規格 | 二次変調方式 | 周波数帯 |
| ---- | ---- | ---- |
| IEEE 802.11a | OFDM | 5GHz |
| IEEE 802.11b | DSSS/CCK | 2.4GHz |
| IEEE 802.11g | OFDM | 2.4GHz |
| IEEE 802.11n(Wi-Fi 4) | OFDM | 2.4GHz, 5GHz |
| IEEE 802.11ac(Wi-Fi 5) | OFDM | 5GHz |
| IEEE 802.11ax(Wi-Fi 6) | OFDMA | 2.4GHz, 5GHz |

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

### ローカル5G

特定の敷地や建物に展開する5G通信網。
土地や建物の所有者は，電気通信事業者ではない場合でも，免許を取得すればローカル5Gシステムを構築することが可能である。(R05h)

## IoT

### CoAP(Constrained Application Protocol)

IoT向けのアプリケーション層のプロトコル
パケット損失が発生しやすいネットワーク環境での，小電力デバイスの通信に向いている。
UDPで再送処理のサポートがある。

### MQQT(Message Queuing Telemetry Transport)

テキストベースのプロトコルであり，100文字程度の短いメッセージの通信に向いている。

## セキュリティ

### DDoS攻撃とNTPサーバー

NTPを使った増幅型のDDoS攻撃に対して，NTPサーバが踏み台にされることを防止する対策
↓
NTPサーバの設定変更によって，NTPサーバの状態確認機能(monlist)を無効にする。(R05h)

### IPSとIDS

- IPS(Intrusion Prevention System): 不正侵入を検知すると遮断する。
- IDS(Intrusion Detection System): 不正侵入を検知すると通知する。

- シグネチャ型: 攻撃パターンに合致した通信を検出する。
- アノマリ型: 正常なふるまいを登録して逸脱した通信を検出する。

- プロミスキャスモード: スイッチのミラーポートに接続する。
- インラインモード: 監視対象の通信経路上に設置する。

### デジタルフォレンジックス

犯罪に関する証拠となり得るデータを保全し，調査，分析，その後の訴訟などに備える。(R04h)

### 攻撃

#### ポリモーフィック型マルウェア

感染ごとに自身のコードを異なる鍵で暗号化するなどの手法によって，過去に発見されたマルウェアのパターンでは検知されないようにする。(R05h)

#### マルチプラットホーム型マルウェア

複数のOS上で利用できるプログラム言語で作成され，複数のOS上で動作する。(R05h)

#### ステルス型マルウェア

ルートキットを利用して自身を隠蔽し，マルウェア感染が起きていないように見せかける。(R05h)

#### ハニーポット

脆弱性があるホストやシステムをあえて公開し，攻撃の内容を観察する。(R04h)

#### RLO(Right-to-Left Override)

文字の表示順を変える制御文字を利用し，ファイル名の拡張子を偽装する。(R04h)

#### サイドチャネル攻撃

暗号化装置における暗号化処理時の消費電力を測定するなどして，当該装置内部の秘密情報を推定する。(R04h)

#### DNSリフレクタ攻撃

対策
DNSサーバをDNSキャッシュサーバと権威DNSサーバに分離し，インターネット側からDNSキャッシュサーバに問合せできないようにする。

## メール

### SMTP

コマンド
- EHLO: SMTPセッション開始
- MAIL: メールトランザクション開始
- RCPT: 宛先の指定
- DATA: メールデータ送信開始

### OP25B(Outbound Port 25 Blocking)

- ISP管理下の動的IPアドレスから，
- ISP管理外のネットワークへの，
- SMTP通信(ポート25番)
を遮断する。

## IP電話

### MOS値(Mean Opinion Score)

ユーザーが体感した品質。
5段階評価。

### ジッタ

音声や映像の乱れの指標

### パケット損失率

### R値

IP電話の音声品質を表す指標のうち，ノイズ，エコー，遅延などから算出される。(R04h)
ジッタやパケット損失率から求める。

## コンピューター

### メモリインタリーブ

主記憶を複数のバンクに分けて，CPUからのアクセス要求を並列的に処理できるようにする。(R05h)

### ディスクバッファ

主記憶と磁気ディスク装置との間にバッファメモリを置いて，双方のアクセス速度の差を補う。(R05h)

### DMA制御方式

主記憶と入出力装置との間でCPUを介さずにデータ転送を行う。(R05h)

### 量子コンピューター

#### 量子アニーリング方式

膨大な選択肢の中から最適な選択肢を探すアルゴリズムに特化している。(R04h)

## クラウド

### IaaS(Infrastructure as a Service)

利用者は，演算機能，ストレージ，ネットワークなどをクラウドに配置して使用することができる。

### PaaS(Platform as a Service)

### SaaS(Software as a Service)

利用者は，クラウドサービス事業者が提供するアプリケーションプログラムを使用することができる。

### DaaS(Desktop as a Service)

利用者は，仮想化したデスクトップ環境を遠隔地の端末から使用することができる。

### FaaS(Function as a Service)

利用者は，プログラムの実行環境であるサーバの管理を意識する必要がなく，その実行環境を使用することができる。

## 開発

### MathML

Webページに，画像を使用せずに数式を表示するために用いられる，XMLマークアップ言語(R05h)

### SysML

システムの設計及び検証を行うために用いられる，UML仕様の一部を流用して機能拡張したグラフィカルなモデリング言語(R05h)

### SystemC

ハードウェアとソフトウェアとの協調設計(コデザイン)に用いられる，C言語又はC++言語を基としたシステムレベル記述言語(R05h)

### VHDL・Verilog-HDL

論理合成してFPGAで動作させるハードウェア論理の記述に用いられる，ハードウェア記述言語(R05h)

### マッシュアップ

店舗案内のWebページ上に，他のサイトが提供する地図検索機能を利用して得た情報を表示する。(R05h)
