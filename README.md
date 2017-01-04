# REVIVE USB ロータリーエンコーダ対応 チャタリング対策版（PID004C）
[REVIVE USB チャタリング対策版（PID004A）](https://github.com/ushui/REVIVE_USB_Debounce)のロータリーエンコーダ対応版です。  
最大6つのロータリーエンコーダを動作させることができます（1つ増やすごとに使えるボタンが2つ減ります）。  

※ロータリーエンコーダはインクリメンタル・ノンクリックタイプを想定しています。エンコーダの仕様によって動作しないことがあるのでご了承ください。  

一度しかボタンを押していないのにダブルクリックになる、キーが複数回入力されるなどのケースを防ぐことができます。  
通常のファームウェアはリアルタイムでスイッチの状態を読み取っているのでわずかな信号の揺れも検知してしまいますが、これを一定時間ごとに読み取って解決しています。  
そのため遅延（デフォルトは最短3ms。1～44370msまで調整可）が生じることに注意が必要です。  
設定ツールによってしきい値を変更できるので、遅延を抑えたい場合やそれでもチャタリングが起きる場合にも対応できます。  
## 使い方
基盤の組み立て方や配線などは下記を参考にしてください。  
URL: <http://a-desk.jp/modules/forum_module/index.php?cat_id=3> 

1. 上記URLからファームウェア書き込みソフト「HIDBootLoader.exe」をダウンロードする。  

2. このリポジトリから「[REVIVE_USB_RENC_Debounce_latest.zip](https://github.com/ushui/REVIVE_USB_RENC_Debounce/raw/master/REVIVE_USB_RENC_Debounce_latest.zip)」をダウンロードし、解凍する。  
「readme.txt」「REVIVE_USB_RENC_Debounce_CT.exe」「REVIVE_USB_RENC_Debounce_FW.hex」の3つのファイルが解凍される。  

3. 手持ちのREVIVE USBのショートピンをBOOTにしてPCへ接続する。  

4. 「HIDBootLoader.exe」を起動して「Open Hex File」ボタンから「REVIVE_USB_RENC_Debounce_FW.hex」を選択し、  
「Program/Velify」ボタンを押してREVIVE USBのファームウェアを書き換える。  
6. REVIVE USBのショートピンをBOOTから戻して再度PCへ接続する。  

7. 「REVIVE_USB_RENC_Debounce_CT.exe」を起動して好みの設定に変更する。  

## 設定ツールについて
「REVIVE USB, Configuration Tool」に手を加え、サンプリング周期・一致検出回数・ロータリーエンコーダの設定項目を増やしたものです。  
当ファームウェア以外では動作しません。  
デフォルトでは「サンプリング周期＝3ms／一致検出回数＝1回」に設定していますが、必要であればこれらを変更することができます。  

![REVIVE USB RENC Debounce, Configuration Tool](https://raw.githubusercontent.com/ushui/REVIVE_USB_RENC_Debounce/master/revive_usb_renc_debounce_ct.png)  
### サンプリング周期
チャタリングノイズ除去の効果があります。  
1～174msまで設定可能で、その設定値ごとにスイッチのON/OFF読み出しを行います。  
長く設定すると長い間隔で読み出されるので、チャタリングが起こりにくくなる代わりに遅延が増加します。  
### 一致検出回数
ノイズ除去の効果があります。  
1～255回まで設定可能で、その回数分スイッチがONであれば正しい入力とみなして入力を行います。  
ONからOFFになる時にも回数分OFFであることを確認します。  
大きく設定するとチャタリングノイズ除去の精度向上や外来ノイズの除去が期待できますが、サンプリング周期に比例して遅延が増加します。  

ロータリーエンコーダは信号の変化が速いため、サンプリング周期や一致検出回数の設定値によっては入力が非常に不安定になります。  
エンコーダの仕様・使用環境・用途にもよりますが、この2つの適正設定値はシビアです。  
かなり少なくすることをおすすめします。  
1ピンと2ピンの設定を「マウス/上移動」「マウス/下移動」にしておくなどして、確認しながら少しずつ調整していい塩梅を見つけてください。  
動作確認を「EC12E2430803」で行ったため、デフォルト値はこれに合わせています。  

なお、遅延秒数は**サンプリング周期×一致検出回数**で求められます（最短の時間です。実際は後ろに少しブレます）。  
## 動作環境について
設定ツールはWindows XP以降のOSで動作します（Windows Vistaで動作を確認しています）。  
書き込み・設定をWindows上で行ってしまえば、ファームウェア（REVIVE USB）はMacやLinux/UNIX系のOSでも動作するはずです。
## 開発環境・開発言語について
### ハードウェア
REVIVE USB (PIC18F14K50)
### ファームウェア
開発環境：MPLAB IDE 8  
開発言語：C
### ソフトウェア（設定ツール）
開発環境：Visual C# 2010 Express  
開発言語：C#
## ソースコードについて
「Assembly Desk License」ライセンスに準拠します。  
「REVIVE USB ロータリーエンコーダ対応 チャタリング対策版」は「REVIVE USBロータリエンコーダ対応版」を参考にして作成しました。  
作成者である「阿部」氏に感謝いたします。

***  
2017/01/04 readme作成。
***
作成者： ushui（ゆーしゅい）  
Twitter: [@kaede_hrc](https://twitter.com/kaede_hrc)  
