---
title: "ログレベル色々"
emoji: "⚠"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [syslog, log4j]
published: false
---

## TL;DR

ERROR，WARN，INFO，DEBUGを使うと楽そう。
Syslogを監視する運用がある場合はその辺りも考慮しよう。

| Syslog | Log4j |
| ---- | ---- |
| 0:Emergency | - |
| 1:Alert | - |
| 2:Critical | FATAL |
| 3:Error | ERROR |
| 4:Warning | WARN |
| 5:Notice | - |
| 6:Informational | INFO |
| 7:Debug | DEBUG |
| - | TRACE |

## この記事について

何となく扱っているログレベルだが，言語やOSによって微妙に違いがあるので，調べてみました。
実装する上で運用しやすそうなレベルも少しだけ考えてみました。

※
私がそれなりに使った事あるのはSyslog, Java, .NETくらいなので，
「それ誰も使ってないよ」みたいなのはあるかもしれません。

## 様々なログレベル

なるべく公式っぽいドキュメントを探しました。

### SyslogのSeverity Levels

RFCで定められている。
いつも忘れるが読み方はセヴェリティ。

<https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.1>

- 0:Emergency
- 1:Alert
- 2:Critical
- 3:Error
- 4:Warning
- 5:Notice
- 6:Informational
- 7:Debug

※**Syslog出す時はFacilityにも気を配ろう**

### Log4j 2

おそらく最も有名なロギングライブラリ。
デファクトスタンダード。

<https://logging.apache.org/log4j/2.x/manual/architecture.html>

- FATAL
- ERROR
- WARN
- INFO
- DEBUG
- TRACE

- OFF
- ALL

### Logback

Log4jの後継を目指しているらしいです。

<https://logback.qos.ch/manual/architecture.html>

- ERROR
- WARN
- INFO
- DEBUG
- TRACE

### SLF4J

定めは無く、使用するライブラリ(Log4jやLogback)依存らしいです。

### Microsoft.Extensions.Logging(.NET)

<https://learn.microsoft.com/ja-jp/dotnet/api/microsoft.extensions.logging.loglevel?view=dotnet-plat-ext-7.0>

- Critical
- Error
- Warning
- Information
- Debug
- Trace

### log4net(.NET)

Log4jの.NET向けクローン。

<https://logging.apache.org/log4net/log4net-1.2.13/release/sdk/log4net.Core.Level.html>

- FATAL
- ERROR
- WARN
- INFO
- DEBUG

- OFF
- ALL

TRACEは無いんですね。

### java.util

<https://docs.oracle.com/javase/7/docs/api/java/util/logging/Level.html>

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

異色……

### Python

<https://docs.python.org/3/library/logging.html#levels>

- CRITICAL=50
- ERROR=40
- WARN=30
- INFO=20
- DEBUG=10

### Logger(Ruby)

<https://docs.ruby-lang.org/en/master/Logger.html>

- UNKNOWN
- FATAL
- ERROR
- WARN
- INFO
- DEBUG

### WEBrick(Ruby)

<https://docs.ruby-lang.org/ja/latest/class/WEBrick=3a=3aBasicLog.html>

- FATAL
- ERROR
- WARN
- INFO
- DEBUG

### PHP

<https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md#3-psrlogloggerinterface>

- emergency
- alert
- critical
- error
- warning
- notice
- info
- debug

### log crate(Rust)

<https://docs.rs/log/latest/log/>

- error
- warn
- info
- debug
- trace

### slog(Go)

<https://betterstack.com/community/guides/logging/logging-in-go/>

- Error
- Warn
- Info
- Debug

### loglevel(JavaScript)

<https://www.npmjs.com/package/loglevel>

- error
- warn
- info
- debug
- trace

### Swift

<https://www.swift.org/server/guides/libraries/log-levels.html>

- critical
- error
- notice
- info
- debug
- trace

<https://developer.apple.com/documentation/os/logger#3845747>

- notice
- debug
- trace
- info
- error
- warning
- fault
- critical

### android.util.Log(Kotlin)

<https://developer.android.com/reference/android/util/Log>

- ASSERT
- DEBUG
- ERROR
- INFO
- VERBOSE
- WARN

### Linux systemdのSeverity

```
$ man logger
```
とかで確認可能。

- 0:emerg
- 1:alert
- 2:crit
- 3:err
- 4:warning
- 5:notice
- 6:info
- 7:debug

Syslogと対応している。

### Linux kernel

<https://www.kernel.org/doc/html/next/core-api/printk-basics.html>

- 0:KERN_EMERG
- 1:KERN_ALERT
- 2:KERN_CRIT
- 3:KERN_ERR
- 4:KERN_WARNING
- 5:KERN_NOTICE
- 6:KERN_INFO
- 7:KERN_DEBUG

Syslogと対応している。

### Cisco IOS

<https://ww.cisco.com/c/en/us/td/docs/routers/access/wireless/software/guide/SysMsgLogging.html#wp1054785>

- 0:emergencies
- 1:alerts
- 2:critical
- 3:errors
- 4:warnings
- 5:notifications
- 6:informational
- 7:debugging

Syslogと対応している。

## 使いやすそうなログレベル

例えば，

- バックエンドのアプリはJavaでLog4j 2を使う
- Syslogサーバーでアプリから発生したエラーを監視をする

というシステムの場合，どう実装したら良いかを考えてみる。

Syslogで指定できるSeverityは以下。

- 0:Emergency
- 1:Alert
- 2:Critical
- 3:Error
- 4:Warning
- 5:Notice
- 6:Informational
- 7:Debug

Log4jで標準で指定できるログレベルは以下。

- FATAL
- ERROR
- WARN
- INFO
- DEBUG
- TRACE

両方にを良い感じに並べると以下となる。

| Syslog | Log4j |
| ---- | ---- |
| 0:Emergency | - |
| 1:Alert | - |
| 2:Critical | FATAL |
| 3:Error | ERROR |
| 4:Warning | WARN |
| 5:Notice | - |
| 6:Informational | INFO |
| 7:Debug | DEBUG |
| - | TRACE |

同じ事象に対してアプリとSyslogでログレベルが合っていた方が混乱が少なそうなので，
基本的にERROR，WARNWARN，INFO，DEBUGの4つを中心に方針を決めると良さそうである。

アプリにFATALが存在する場合，SyslogはCriticalに対応させるのが無難そう。
Log4jにもNorticeが欲しい……

## 運用

規模の大きい企業ではインフラ系の部署に運用監視専門オペレーターがいて，例えば「WARN以上はメール通知」「Critical以上は電話連絡」のような運用プロセスが決まっていたりする。
なので，アプリ開発中に運用監視プロセスとシステム運用者の要望を確認しておくと良い感じにロギングの実装ができる。
