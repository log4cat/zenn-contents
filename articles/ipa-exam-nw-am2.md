---
title: "ネスペ午前Ⅱ対策メモ"
emoji: "✏"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [情報処理技術者試験,ネットワークスペシャリスト]
published: true
---

NWの午前Ⅱ試験で直前に読む用のメモ。
(一度覚えたことを思い出す用)

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

### フォワード誤り訂正

送信データに付加された誤り訂正符号を用いで受信側で誤りを検出して回復する。

### データグラム方式

コネクションレス通信。

## L2

### CSMA

キャリア信号を検出し，データの送信を制御する。(R03h)

#### CSMA/CA

無線LANで使用する。
DIFSに電波を検知しなければデータを送信し，SIFSの後にACKを返す。

#### CSMA/CD

有線LANで使用する。
フレームが衝突した場合はランダムな時間待ってから再送する。

### GARP(Gratuitous ARP)

自分自身のIPアドレスを指定したARｐを送る。
IPアドレスの重複を検出できる。
HSRPでの切り替わり通知。

### RARP(Reverse Address Resolution Protocol)

MACアドレスからIPアドレスを得る。

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
- オーセンティケーター: 仲介役(スイッチや無線LAN)。RADIUSクライアントとなる。
- オーセンティケーションサーバー: RADIUSサーバーなど

- EAP-MD5: ユーザー名とパスワード
- EAP-TLS: デジタル証明書を用いた相互認証
- EAP-TTLS: TLSでサーバー認証した後にトンネル内で各種認証を行う。
- EAP-FAST: EAP-TTLSのようなCisco独自プロトコル

### L2トンネリング

#### PPTP(PPP Tunneling Protocol)

PPPデータフレームをIPパケットでカプセル化して，インターネットを通過させるためのトンネリングプロトコル(R03h)

#### L2TP(Layer 2 Tunneling Protocol)

L2FとPPTPを統合して改良したデータリンク層のトンネリングプロトコル(R03h)

## L3

### CIDR(Classless Inter-Domain Routing)

クラスレスにサブネットを設定する方式。

### DHCP

クライアントに動的にIPアドレスを割り当てるプロトコル。

#### DHCPリレーエージェント

別セグメントのDHCPサーバーとルーターを介して通信する。

### NAT

IPアドレスを変換する。

### NAPT

ポート番号を変えて1つのグローバルIPと複数のプライベートアドレスを返還する。

### VRRP

仮想IPアドレスと仮想MACアドレスによってゲートウェイを冗長化する。
HSRPはCisco社の独自規格。

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
リンク状態のデータベースを使用している。(R01a)
トポロジ変更や障害を検出するとLSAの配信によって動的にルートが変更される。
コスト値に基づいて最適な経路を決定する。
負荷分散可能。
ネットワークをエリアに分割し，エリアをバックボーンで結ぶ。

##### OSPFv3

IPv6用に拡張されたOSPF

[RFC 5340](https://datatracker.ietf.org/doc/html/rfc5340)

##### IS-IS

リンクステート型

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

### MPLS

ラベルと呼ばれる識別子を挿入することによって，IPアドレスに依存しないルーティングを実現する，ラベルスイッチング方式を用いたパケット転送技術(R03h)

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

Aレコード複数で1ドメインに複数IPアドレスが可能。
CNAMEレコードで複数ドメインに1IPアドレスが可能。

[RFC 1035](https://datatracker.ietf.org/doc/html/rfc1035#section-3.2.2)
[RFC 3596](https://datatracker.ietf.org/doc/html/rfc3596)

### HTTP

#### Cookie

##### Secure属性

URL内のスキームがhttpsのときだけ，WebブラウザからCookieが送出される。(R01a)

##### Expires属性

Cookieに指定された有効期間を過ぎると，Cookieが無効化される。(R01a)

##### HttpOnly属性

JavaScriptによるCookieの読出しが禁止される。(R01a)

##### Path属性

WebブラウザがアクセスするURL内のパスとCookieによって指定されたパスのプレフィックスが一致するとき，WebブラウザからCookieが送出される。(R01a)

##### Domain属性

Cookieを送信できるドメインを指定する。

#### HSTS(HTTP Strict Transport Security)

WebサイトがWebブラウザに対して，指定された期間において，当該Webサイトへのアクセスをhttpsで行うように指示するHTTPレスポンスヘッダフィールド
↓
Strict-Transport-Security(H30a)

#### Webサービス

HTTP上のSOAPによってソフトウェア同士が通信して，ネットワーク上に分散したアプリケーションプログラムを連携させることができる。(R05h)

#### WebDAV

HTTPを拡張したプロトコルを使って，サーバ上のファイルの参照，作成，削除及びバージョン管理が行える。(R05h)
HTTPを使って，Webサーバのコンテンツのアップロードや更新を可能にするプロトコル。(R01a)

#### WebFTP

Webブラウザで"ftp://"から始まるURLを指定して，ソフトウェアなどの大きなファイルのダウンロードができる。(R05h)

#### WebSocket

通信はGETメソッドで始まり，クライアントとサーバ間でハンドシェイクをして接続が確立する。(H30a)

#### XMLSocket

HTTPを拡張したプロトコルであり，通信メッセージはXML形式で記述される。(H30a)

### SNMP

#### MIB(Management Information Base)

OIDの定義

#### Trap

機器の変化をマネージャーに伝える。。

#### RMON(Remote network MONitoring)

ネットワークのトラフィック管理において，測定対象の回線やポートなどからパケットをキャプチャして解析し，SNMPを使って管理装置にデータを送信する仕組み(R04h)

### ファイル転送

#### FTP

パッシブモード: クライアント→サーバーのデータコネクション古い。
アクティブモード: サーバー→クライアントのデータコネクション。古い。

いずれも制御用コネクションはクライアント→サーバー。

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

### TKIP(Temporal Key Integrity Protocol)

アクセスポイントが認証局と連携し，パスワードをセッションごとに生成する仕組み(R03h)

### WPS(Wi-Fi Protected Setup)

無線LANに接続する機器のセキュリティ対策に関するWPSの仕様(R03h)
機器同士を簡単に接続するための規格。

### ローカル5G

特定の敷地や建物に展開する5G通信網。
土地や建物の所有者は，電気通信事業者ではない場合でも，免許を取得すればローカル5Gシステムを構築することが可能である。(R05h)

## QoS

帯域制御

### アドミッション制御

通信を開始する前にネットワークに対して帯域などのリソースを要求し，確保の状況に応じて通信を制御する(R03h)

### ポリシング

入力されたトラフィックが規定された最大速度を超過しないか監視し，超過分のパケットを破棄するか優先度を下げる制御(R03h)

### シェーピング

パケットの送出間隔を調整することによって，規定された最大速度を超過しないようにトラフィックを平準化する制御(R03h)

### ストリーミング

#### RTSP(Real Time Streaming Protocol)

## ネットワーク管理

### OpenFlow

SDN(Software-Defined Networking)でコントローラーとOpenFlowスイッチ間の通信に使われる。
信頼性や安全性を確保するためにTCPやTLSを使用する。(R01a)
OpenFlowを用いたネットワークでは，ネットワーク機器がデータ転送を行うための情報はコントローラーから提供される。(H30a)

## IoT

### CoAP(Constrained Application Protocol)

IoT向けのアプリケーション層のプロトコル
パケット損失が発生しやすいネットワーク環境での，小電力デバイスの通信に向いている。
UDPで再送処理のサポートがある。

### MQQT(Message Queuing Telemetry Transport)

テキストベースのプロトコルであり，100文字程度の短いメッセージの通信に向いている。
パブリッシュ／サブスクライブ(Publish/Subscribe)型のモデル(R03h)

### 6LoWPAN シックスローパン

IEEE 802.15.4における低速・低消費電力のIPv6通信用の規格

### BLE(Bluetooth Low Energy)

低電力のBluetooth。
2.4GHz

### Wi-SUN(Wireless Smart Utility Network) ワイサン

マルチホップを使用して500mを超える通信が可能である。(R01a)

### ZigBee

IEEE 802.15.4

- ZigBeeコーディネータ: ネットワークに1台存在する制御端末
- ZigBeeルータ: 中継器
- ZigBeeエンドデバイス: 端末

## LDAP(Lightweight Directory Access Protocol)

X.500シリーズ(DAP)を単純化，軽量化している。
TCPを使う。

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

### ルートキット

OSなどに不正に組み込んだツールを隠蔽する。(R01a)

### 攻撃

#### SSL/TLSのダウングレード攻撃

暗号化通信を確立するとき，弱い暗号スイートの使用を強制することによって，解読しやすい暗号化通信を行わせる。(R01a)

#### ICMP Flood攻撃

pingコマンドを用いて大量の要求パケットを発信することによって，攻撃対象のサーバに至るまでの回線を過負荷にしてアクセスを妨害する。(H30a)

#### SYN flood攻撃

送信元IPアドレスがA，送信元ポート番号が80/tcpのSYN/ACKパケットを，未使用のIPアドレス空間であるダークネットにおいて大量に観測した場合(R01a)
↓
IPアドレスAを攻撃先とするサービス妨害攻撃

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

#### リフレクタ攻撃

DNS，Memcached，NTPが悪用される事が多い(R03h)

#### DNSリフレクタ攻撃

対策
DNSサーバをDNSキャッシュサーバと権威DNSサーバに分離し，インターネット側からDNSキャッシュサーバに問合せできないようにする。

#### OSコマンドインジェクション

Webアプリケーションの脆弱性を悪用する攻撃手法のうち，入力した文字列がPerlのsystem関数，PHPのexec関数などに渡されることを利用し，不正にシェルスクリプトを実行させる(R03h)

#### HTTPヘッダインジェクション

#### クロスサイトリクエストフォージェリ

#### セッションハイジャック

### 前方秘匿性(Forward Secrecy)

鍵交換に使った秘密鍵が漏えいしたとしても，過去の暗号文は解読されない。(R03h)

### ブロックチェーン

時系列データをチェーンの形で結び，かつ，ネットワーク上の複数のノードで共有するので，データを改ざんできない。(R03h)

### 公開鍵暗号方式

対となる二つの鍵の片方の鍵で暗号化したデータは，もう片方の鍵でだけ復号できる。(R03h)

### ハッシュ関数に求められる原像計算困難性

データに非可逆処理をして生成される固定長のハッシュ値からは，元のデータを推測できない。(R03h)

## メール

### MIME(Multipurpose Internet Mail Extensions)

ASCII以外の文字をSMTPで使うためのエンコード。

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

## システムの性能指標

### M/M/1の待ち行列モデル

並んでからサービスを受け終わるまでの平均時間

$$
\frac{\text{利用率}}{1 - \text{利用率}} \times \text{平均サービス時間} + \text{平均サービス時間}
$$

## 開発

### UML

#### ユースケース図

システムとアクタの相互作用を表現する。(R01a)

#### ステートチャート図

外部からのトリガに応じて，オブジェクトの状態がどのように遷移するかを表現する。(R01a)

#### クラス図

クラスと関連から構成され，システムの静的な構造を表現する。(R01a)

### DFD(Data Flow Diagram)

データの流れに注目してシステムの機能を表現する。(R01a)

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

### フールプルーフ

システムに規定外の無効なデータが入力されたとき，誤入力であることを伝えるメツセージを表示して正しい入力を促すことによって，システムを異常終了させない。(R04h)

### フェールセーフ

### フェールソフト

### フォールトトレランス

### FMEA(Failure Mode and Effects Analysis)

システムの構成品目の故障モードに着目して，故障の推定原因を列挙し，システムへの影響を評価することによって，システムの信頼性を定性的に分析する。

### FTA(Fault Tree Analysis)

障害と，その中間的な原因から基本的な原因までの全てを列挙し，それらをゲート(論理を表す図記号)で関連付けた樹形図で表す。(R03h)

### RCA(Root Cause Analysis)

障害に関するデータを収集し，原因について"なぜなぜ分析"を行い，根本原因を明らかにする。

### ODC分析(Orthogonal Defect Classification Analysis)

多角的で，互いに重ならないように定義したODC属性に従って障害を分類し，どの分類に障害が集中しているかを調べる。

## 著作権保護技術

### CPPM(Content Protection for Pre-Recorded Media)

再生メディアで使用される暗号化技術。

### CPRM(Content Protection for Recordable Media)

SDメモリカードに使用される著作権保護技術。(R01a)

### DTCP(Digital Transmission Content Protection)

ホームネットワークで使用される暗号化技術。

### HDCP(High-bandwidth Digital Content Protection)

HDMIやDisplay Portで使用される暗号化技術。
