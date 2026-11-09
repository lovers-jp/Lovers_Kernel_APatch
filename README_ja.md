# Lovers Kernel APatch

**自動学習によるチューニングと連携する、Pixel向けカスタムカーネル。**

| 項目 | 内容 |
|---|---|
| 対象シリーズ | Pixel 11 / Pixel 11 Pro / Pixel 11 Pro XL |

[English](README.md) | [日本語](README_ja.md)

Lovers Kernel APatchは、**Pixel 11シリーズ**を対象に、CPUスケジューラ・メモリ管理・ストレージ・ネットワークの変更をまとめたカスタムカーネルです。APatch-Baseによるroot環境を想定し、**Lovers Kernel Tuner**による学習・設定管理と連携します。

## 主な機能

| 分野 | 内容 |
|---|---|
| CPUスケジューラ | Android EEVDF上のBORE、CASSによるCPU配置 |
| IRQ負荷分散 | CPUの処理能力を考慮するSBalanceと稼働統計 |
| CPU周波数・アイドル | Schedutil調整、TEOの予測統計修正、CPU latency QoS設定 |
| メモリ管理 | DAMON/DAMOSによる適応回収、mTHP、MGLRU調整 |
| ZRAM | ZRAM/ZSMALLOC組み込み、LZ4主圧縮と低頻度アクセスページのZSTD再圧縮制御 |
| ストレージ | ADIOS I/Oスケジューラ、TMPFS調整、F2FS／EROFSの変更 |
| ネットワーク | BBRv3、CAKE、fq_codel対応 |
| 電力関連 | RCU_LAZY、省電力向けworkqueue設定 |
| SUSFS・保護機能 | APatch向けに移植した独立SUSFS、Baseband Guard（BBG） |

実際に働く機能は、端末・使用中のCPUFreqガバナー・実行時設定によって異なります。Schedutilの設定はschedutil使用時が対象です。CASSの初期モードはAndroid vendor hookの有効なCPU選択を優先します。CAKE対応だけで、通信インターフェースのキューが自動的に切り替わるわけではありません。

## 連携ツール

### Lovers Kernel Tuner

**Lovers Kernel Tuner**は、このカーネルの制御口を利用する学習・統計表示・設定管理に対応しています。

インストール後にアプリを開き、root権限を許可してください。新規導入時はAIと再起動後の自動起動が初期ONです。対応機能を観測し、負荷などの比較条件が揃うと、1項目ずつ候補を試して設定を調整します。手動指定と保存済みのAI OFF設定を優先します。

従来の制御に加え、CASS・SBalance・schedutil・CPU Idle・ZRAM再圧縮タイミングの管理に対応しています。**充電上限90%**などの設定は、端末が対応する充電制御口を公開している場合に利用できます。

CPU配置・IRQ分散・再圧縮はカーネルが実行し、アプリは対応する設定方針を選びます。このリリースについて、実機の性能・FPS・電池持ち・長期的なバッテリー寿命の改善は未測定です。

### APatch SUSFS Control WebUI

**APatch SUSFS Control WebUI**は、独立SUSFS **`v2.3.0-ap1 / APatch-GKI`**向けの日本語／英語対応設定モジュールです。

APatch／FolkPatch ManagerでモジュールZIPを導入し、再起動後に管理アプリからWebUIを開きます。ルールの保存・起動時適用・状態表示・設定のバックアップ／復元に対応します。一部のルール変更には再起動が必要です。このWebUI版の実機動作は未検証です。

## 導入方法

| 配布物 | 使用方法 |
|---|---|
| カーネル（`boot.img`） | APatch-Baseなどの対応する管理アプリでパッチを適用し、パッチ済みの`boot.img`を生成 |
| SUSFS制御モジュールZIP | 対応するAPatch系管理アプリのモジュールとして導入 |
| Lovers Kernel Tuner APK | Androidアプリとしてインストールし、初回起動時にrootを許可 |

## クレジット・ライセンス

Linux kernel、Android Common Kernel、Google Pixel kernel、SUSFS、Baseband Guard、BBR、BORE、ADIOS、Sultan kernelなど、本プロジェクトの基礎となった開発者・貢献者の皆様に感謝します。Sultan関連の変更はAndroid 6.12向けに調整した移植です。
