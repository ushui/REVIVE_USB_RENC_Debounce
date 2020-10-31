# REVIVE USB ロータリーエンコーダ対応 チャタリング対策版（PID004C）
[REVIVE USB チャタリング対策版（PID004A）](https://github.com/ushui/REVIVE_USB_Debounce)のロータリーエンコーダ対応版です。  

## 特徴
REVIVE USB ロータリーエンコーダ対応 チャタリング対策版のファームウェア(以下、対策版ファームウェア)は、REVIVE USBのファームウェアを公式マニュアルに沿って書き換えることで使用できるマイコン用プログラムです。  
REVIVE USB RENC Debounce, Configuration Tool(以下、対策版設定ツール)は、対策版ファームウェアで使用できるWindowsアプリケーションです。  
公式ファームウェア/公式設定ツールに対し、以下の機能を実装しています。  

### チャタリング防止機能
公式ファームウェアでは対応していない、チャタリング防止やノイズ除去を実装した機能です。  
一度しかボタンを押していないのにダブルクリックになる、キーが複数回入力されるなどのケースを防ぐことができます。  
また、遅延を抑えたい場合やそれでもチャタリングが起きる場合に、対策版設定ツールにはチャタリング防止機能のしきい値を変更できる機能を備えています。  

### 遅延秒数の計算機能
チャタリング防止機能の設定により、入力遅延が発生する秒数が変わります。  
そこで、現在の設定では平均でどの程度遅延が発生するか、自動で計算する機能を備えています。  

### ロータリーエンコーダ設定機能
公式ファームウェアでは使用できない、ロータリーエンコーダが使用できる機能です。  
1P～12PのピンにロータリーエンコーダのA相/B相を接続して、対策版設定ツールで接続したピンに設定をすると、ロータリーエンコーダの回転による入力が行えるようになります。  
最大6つまで接続可能です。  

## 対策版ファームウェアの適用手順
1. ファームウェア書き込みソフトのダウンロード  
[公式リポジトリのFirmWareフォルダ](https://github.com/bit-trade-one/REVIVE-USB/tree/master/FirmWare)からファームウェア書き込みソフト「HIDBootLoader.exe」をダウンロードしてください。  

1. 対策版ファームウェア(.hex)、対策版設定ツール(.exe)のダウンロード  
このリポジトリから「[REVIVE_USB_RENC_Debounce_latest.zip](https://github.com/ushui/REVIVE_USB_RENC_Debounce/raw/master/REVIVE_USB_RENC_Debounce_latest.zip)」をダウンロードし、展開してください。  

1. BOOTモードのREVIVE USBを接続  
手持ちのREVIVE USBのショートピンをBOOTにしてPCへ接続してください。  

1. ファームウェアの書き換え  
「HIDBootLoader.exe」を起動して「Open Hex File」ボタンから「REVIVE_USB_RENC_Debounce_FW.hex」を選択し、  
「Program/Velify」ボタンを押してREVIVE USBのファームウェアを書き換えてください。  

1. 通常モードのREVIVE USBを接続  
REVIVE USBのショートピンをBOOTから戻して再度PCへ接続してください。  

1. 対策版設定ツールの起動  
「REVIVE_USB_RENC_Debounce_CT.exe」を起動してください。  
右下に「FW Version: RDx.x」と表示されていれば対策版ファームウェアが正しく適用されています。

## 対策版設定ツールの追加変更箇所
![REVIVE USB RENC Debounce, Configuration Tool](https://raw.githubusercontent.com/ushui/REVIVE_USB_RENC_Debounce/master/revive_usb_renc_debounce_ct.png "REVIVE USB RENC Debounce, Configuration Toolのデジタル設定画面")
#### チャタリング防止設定 - サンプリング周期
指定した時間の一定間隔をおいて入力値の受信・送信を行います。  
例えば4msに設定した場合は、REVIVE USBが4msごとに受信した入力値を反映し、USBで接続した機器に送信します。  
ON/OFFが確定するまでの不安定な信号の揺れを安定化する効果があります。  
1～174msまで設定可能で、デフォルト値は4ms、推奨値は1～10msです。  

#### チャタリング防止設定 - 一致検出回数
ON/OFFのどちらかを採る入力判定が、指定した回数分、同じだったときに入力値を反映します。  
例えば2回に設定した場合は、REVIVE USBが2回連続でONとして判定したとき、入力値をUSBで接続した機器に送信します。  
ON/OFFが確定している間の不安定な信号の揺れを無効化する効果があります。  
1～255回まで設定可能で、デフォルト値は2回、推奨値は1～3回です。  

#### ロータリーエンコーダ設定
「1P/2P」から「11P/12P」までがペアになったチェックボックスのうち、チェックを入れたピンをロータリーエンコーダのA相/B相として扱います。  
チェックを入れなければ通常のデジタル入力として扱います。  

#### チャタリング防止設定 - 一致検出回数(ロータリーエンコーダ)
「ロータリーエンコーダ設定」に設定したピンは、この設定値を使用します。  
1～255回まで設定可能で、デフォルト値は1回、推奨値は1～2回です。  

### 平均遅延秒数
チャタリング防止設定で設定したサンプリング周期、一致検出回数から自動で遅延する秒数を計算します。  
ゲーム用途などで操作デバイスの遅延秒数を知りたい場合は、この平均遅延秒数を遅延する秒数として扱ってください。  
設定ツールで使用している計算式は`サンプリング周期 * 一致検出回数 - (サンプリング周期 / 2)`です。  

　*※1 ロータリーエンコーダは、一致検出回数が一致検出回数(ロータリーエンコーダ)になります。*  
　*※2 以下に示すツール上で算出できない遅延は計算に含めていません。*  
　　　*・ディスプレイの遅延*  
　　　*・一致検出回数を基準とした処理における一致しなかった場合の遅延*  
　*※3 誤差は`±サンプリング周期 / 2`です。*  

## その他ドキュメントについて
 - [FAQ.md](https://github.com/ushui/REVIVE_USB_Debounce/blob/master/FAQ.md)
   - 公式FAQに対策版のFAQを追加しました。
 - [公式リポジトリの設定ツールのReadme.md](https://github.com/bit-trade-one/REVIVE-USB/blob/master/App/Readme.md)
   - 公式ドキュメントです。対策版とUIが異なりますが、基本的な使用方法は変わりません。

#### 作例
[REVIVE USBを使ったインテリアな自作ボルテコン](http://gurochoro.blogspot.com/2017/01/self-made-sdvx-controller.html)

----

開発環境(ファームウェア)： MPLAB IDE v8.92、MPLAB C for PIC18 v3.47 Standard-Eval Version  
開発環境(設定ツール): Microsoft Visual C# 2010 Express  
作成者： ushui（ゆーしゅい）  
Twitter: [@kaede_hrc](https://twitter.com/kaede_hrc)  
