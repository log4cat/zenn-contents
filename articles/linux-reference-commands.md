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

bashで確認しています。

## Linuxで最初に絶対に覚えるべき操作

```bash:マニュアル
man {command_name} # コマンドのマニュアル(qで閉じる)
man man # manコマンドのマニュアル
help # bashのビルトインコマンド
```

Linuxで最初に覚えるべきコマンドは`man`です。

```bash:ジョブ
Ctrl + c # コマンドを途中で終わらせてプロンプトに戻る
Ctrl + z # コマンドを一時停止してプロンプトに戻る(再開可能)
jobs # ジョブID
fg {job_id} # 一時停止したジョブをフォアグラウンドにする
bg {job_id} # 一時停止したジョブをバックグラウンドにする
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
hostname # ホスト名
cat /etc/os-release # Linuxディストリビューション
uname -a # Linuxカーネルの情報
cat /proc/version # Linuxカーネルの情報
```

```bash:コマンド環境
echo $LANG # 言語と文字コード
alias # エイリアス
echo $SHELL # Login Shell
cat /etc/shells # インストール済のShell
echo $0 # 現在のShell
ll /bin/sh # shのシンボリックリンク(一般的にbash)
umask # 現在のumask(ファイルやディレクトリ作成時の権限に影響する)
```

```bash:ネットワーク設定
ip address #IPアドレス "ip a"でも可能
ip route # ルーティング
cat /etc/resolv.conf # DNS
ping {host_name} # ホスト名 or IPアドレスを指定
```

## ファイルとディレクトリ

```bash:ディレクトリ
cd # ディレクトリ移動
pwd # カレントディレクトリ
ll # ファイルとディレクトリ一覧
ls -alhF {directory} # -a:隠しファイルも -l:長いフォーマット -h:サイズを読みやすく -F:タイプ識別子
ls -l --time-style=long-iso {directory} # タイムスタンプをISO(のような)形式で分単位まで
ls -l --time-style=full-iso {directory} # ナノ秒単位
ls --full-time {directory} # --full-timeは-l --time-style=full-isoと同じ
ls -alhF --time-style="+%Y%m%dT%H%M%S%z" # ISO 8601(基本形式)
ls -alhF --time-style="+%Y-%m-%dT%H:%M:%S%:z" # ISO 8601(拡張形式)
```

(lsの標準オプションがISO 8601と違うのが気になります……)

```bash:ファイル
find {directory} -name {file_name} # ファイル検索
cat {file_path} # 内容を表示
cat {file_path1} {file_path2} # 内容を連結して表示
less {file_path} # ページ送りで内容を表示(スクロール化)
more {file_path} # ページ送りで内容を表示
diff {file_path1} {file_path2} # 差分を表示
```

## 運用状況

```bash:サーバー
uptime # 起動時間
uptime -s # 起動時刻
date -u # システムクロックの表示(UTC)
TZ=JST-9 date # システムクロックの表示(JST)
TZ=Asia/Tokyo date # 同上
TZ=JST-9 date -Iseconds # -I[単位]でISO 8601形式
timedatectl status # タイムゾーン設定など
top # システムのリソース使用状況
ps -efl # プロセス
ps axu # プロセス(BSDシンタックス)
```

```bash:ユーザーとグループ
cat /etc/passwd # 存在するユーザーの一覧
cat /etc/group # 存在するグループの一覧
who # ログイン中のユーザー
last # ログイン履歴
history # 現在のユーザーのコマンド履歴
sudo cat /home/{ユーザー名}/.bash_history # 特定のユーザーのコマンド履歴(要権限)
```

## フィルタとパイプライン

```bash:フィルタ
grep {pattern} {file_path} # grep
head {file_path} # ファイルの先頭10行を表示
tail {file_path} # ファイルの末尾10行を表示
tail -f {file_path} # ファイルへ追記されるデータをリアルタイム表示
uniq {file_path} # 重複した行を削除する
cut {option} {file_path} # オプションで指定した方法で項目を切り出す
```

```bash:パイプ処理
{command} | grep {pattern} # コマンドの結果をgrep
{command} | sort # コマンドの結果をソート
diff <({command1}) <({command2}) # コマンドの結果をdiff
```

## apt(Debian系)

```bash:apt
apt list --installed # インストール済のパッケージ一覧
apt list # インストール可能／済のパッケージ一覧
apt list {glob_expression} # 1つ以上のglob表現でフィルタリング
apt list *{package_name}* # パッケージ名の部分一致検索
apt search {package_name} # 説明文から検索
```

## dnf(RHEL 8以降)

```bash:dnf
dnf list installed # インストール済のパッケージ一覧
dnf list available # インストール可能なパッケージ一覧
dnf list # インストール可能／済のパッケージ一覧
dnf list {glob_expression} # 1つ以上のglob表現でフィルタリング
dnf list *{package_name}* # パッケージ名の部分一致検索
dnf search {keyword} # パッケージ名と概要から検索
```

※なお、`dnf`も`yum`も`dnf-3`にリンクされている。

```bash
$ ll /bin/dnf
lrwxrwxrwx. 1 root root 5 Nov 15  2022 /bin/dnf -> dnf-3
$ ll /bin/yum
lrwxrwxrwx. 1 root root 5 Nov 15  2022 /bin/yum -> dnf-3
```

## yum(RHEL 7以前)

CentOS 7.3で確認。

```bash:yum
yum list installed # インストール済のパッケージ一覧
yum list available # インストール可能なパッケージ一覧
yum list # インストール可能／済のパッケージ一覧
yum list {glob_expression} # 1つ以上のglob表現でフィルタリング
yum list *{package_name}* # パッケージ名の部分一致検索
yum search {keyword} # パッケージ名と概要から検索
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

## Apache HTTP Server

```bash:httpd
httpd -v # バージョン
systemctl status httpd # 起動状態
```
