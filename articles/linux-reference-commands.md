---
title: "Linux参照コマンドメモ"
emoji: "👀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Linux]
published: true
---

以下のような状況で使用できるLinuxコマンドのメモです。

- サーバーを引き渡された時にスペックや環境を確認
- 稼働状況の確認

情報を参照するコマンドだけなので安心して使えます。(多分)

## 確認した環境

- Ubuntu 22.04.3 LTS(Debian系)
- AlmaLinux 9.3(Fedora系, RHEL 9.3相当)

## マニュアル

```bash:マニュアル
man {command_name} # コマンドのマニュアル 
man man # manコマンドのマニュアル
```

## スペックや構成の確認

```bash:スペック
lscpu # CPUの情報
cat /proc/cpuinfo # CPUの情報(コア数が多いと見づらい)
free -h # メモリの情報
df -hl # ストレージの情報
arch # マシンのアーキテクチャ (x86_64とかが表示される)
uname -m # archと同じ
```

```bash:OS情報
cat /etc/os-release # Linuxディストリビューション
uname -a # Linuxカーネルの情報
cat /proc/version # Linuxカーネルの情報
```

```bash:コマンド環境
echo $LANG # 言語と文字コード
alias # エイリアス
echo $SHELL # デフォルトのShell
cat /etc/shells # インストール済のShell
```

```bash:ネットワーク設定
ip addr #IPアドレス
ip a # ip addr showと同じ
ip route # ルーティング
cat /etc/resolv.conf # DNS
ping {host_name} # ホスト名 or IPアドレスを指定
```

```bash:ディレクトリ関連
pwd # カレントディレクトリ
ll # ファイルとディレクトリ一覧
ls -lhF --all --full-time / # -l:長いフォーマット -F:タイプ識別子 -h:サイズを読みやすく --full-time:完全な日時 /:ルートディレクトリ
```

## 運用状況

```bash:サーバー関連
uptime # 起動時間
top # システムのリソース使用状況
ps -efl # プロセス
ps axu # プロセス(BSDシンタックス)
```

```bash:ユーザーとグループ
cat /etc/passwd # 存在するユーザーの一覧
cat /etc/group # 存在するグループの一覧
who # ログイン中のユーザー
last # ログイン履歴
```

## パッケージ関連(apt)

Debian系で使用。

```bash:apt
apt list --installed # インストール済のパッケージ一覧
apt list # インストール可能／済のパッケージ一覧
apt list {package_name} # パッケージ名から検索
apt search {package_name} # 説明文から検索
```

## パッケージ関連(yum)

Fedora系で使用。

```bash:yum
yum list installed # インストール済のパッケージ一覧
yum list available # インストール可能なパッケージ一覧
yum list # インストール可能／済のパッケージ一覧
yum list | grep {package_name} # パッケージ名から検索
yum search {package_name} # 説明文から検索
```

## Docker

```bash:Docker
sudo docker version # バージョン
sudo docker system info # 環境
sudo docker system df # ディスク使用状況
sudo docker image ls # イメージ一覧
sudo docker image inspect {image_name} # イメージ詳細
sudo docker container ls # コンテナ一覧
sudo docker container stats # コンテナ稼働状況
sudo docker network ls # ネットワーク一覧
```
