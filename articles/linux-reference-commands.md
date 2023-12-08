---
title: "Linux参照コマンドメモ"
emoji: "👀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---
Linuxの仮想サーバーを引き渡された時にスペックなどを確認するために使ったコマンドのメモです。
(Fedora特有のもの含む)

AlmaLinux 9.1とCentOS 7.3で実行しました。

## スペックや構成の確認

```bash
lscpu # CPUの情報
cat /proc/cpuinfo # CPUの情報(コア数が多いと見づらい)
free -h # メモリの情報
df -hl # ストレージの情報
ip addr show #IPアドレス

cat /etc/os-release # ディストリビューション
uname -a # Linuxカーネルの情報
cat /proc/version # Linuxカーネルの情報
arch # マシンのアーキテクチャ (64ビットならx86_64とか表示される)
uname -m # archと同じ
echo $LANG # 言語と文字コード

ls -l / # ルートディレクトリ直下の確認
cat /etc/group # 存在するグループの確認
```

## パッケージの確認(apt)

```bash
apt list --installed # インストール済のパッケージ
apt list {package_name} #
apt search {package_name} # 
```

## パッケージの確認(yum)

```bash
yum list installed # インストール済のパッケージ
yum list # インストール可能／済のパッケージ
yum search {package_name} #
```

## 運用状況の確認に使えそう

```bash
uptime # 起動時間
last # ログイン履歴
top # システムのリソース使用状況
```
