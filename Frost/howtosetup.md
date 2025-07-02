# PC（Frost）セットアップまとめ

## OS:RockyLinux9.5のインストール
### ISOイメージのダウンロード
Linuxディストリビューションは, ISOイメージ形式(.isoファイル)で配布されており, ISOイメージが書き込み可能なメディア媒体(CD/DVD, USB)に書き込めばOSインストール用のメディアを作ることができる.
###  OSのインストール
新しいSSDをセットし, OSインストール用USBを挿してPCを起動させるとOSのインストールが始まる.

### その後の設定
- root, userアカウントのパスワード設定
- useridの設定（研究室内の他PCを考慮しつつ）
- PCの名前を設定
- ホームディレクトリを日本語設定から英語設定に変更 ``$ LANG=C xdg-user-dirs-gtk-update``


## Rocky Linux 9.5 日本語入力環境（IME）の構築
Rocky Linux (RHEL系)は企業用のosで最小限の機能しか備わっていないので, Ubuntsなどと違って自力でIMEをインストール・設定する必要がある.

### IMEを設定するために必要なパッケージ
| 用途               | 名称             | 役割                                     |
| ----------------- | -------------- | -------------------------------------- |
| 入力メソッドフレームワーク  | `IBus`         | 文字入力と変換を仲介する土台。複数のIMEを統一して扱う           |
| 日本語変換エンジン（IME） | `Mozc`         | 「にほんご」→「日本語」に変換する中核（Google日本語入力のオープン版） |
| 日本語ロケールパック     | `langpacks-ja` | UIや日付・数字フォーマットを日本語に対応させる               |

### パッケージインストール
```bash
sudo dnf install ibus ibus-mozc langpacks-ja
```
そのあと設定画面開いて Keyboard/入力ソース で「日本語(Mozc)」を選ぶ.

## SSH接続設定
研究室外のネットワークからリモート接続するために設定する. サーバー側にOpenSSHがインストールされていたら簡単にsshの設定が行えた.
#　手順
1. サーバー側の確認・準備
```bash
which sshd
```
2. SSHサーバーが起動中か確認
```bash
sudo systemctl status sshd
```
- 出力結果が緑色の丸が見えてactive(runing)ならOK!

3. ファイアーウォールでSSHを許可
```bash
sudo firewall-cmd --permanent --add-service==ssh
sudo firewall-cmd --reload
```
4. サーバーのIPアドレス確認
```bash
ip a
```
5. 確認したアドレスを元にクライアントの方からssh接続
```bash
ssh usrname@XXXX.XX.XX
```
- この時, クライアント側で`/etc/hosts`に`XXXX.XX.XX pcname`と編集すると接続が`usrname@pcname`と楽になる.

## Intel oneAPI(Base ToolKit, HPC ToolKit)のインストール
- BaseKitとHPCKitをインストールすることで, 高性能計算(HPC)や科学技術計算, AI/ディープラーニングの開発などの環境が整う.
- それぞれの役割としては, BaseKitがプログラム環境基盤, HPCKitがHPC環境構築
- ダクト内乱流や剪断流の計算でMPI通信を使うため上記2つのツールが必要

### インストール方法確認
Intel公式ページ
https://www.intel.com/content/www/us/en/developer/tools/oneapi/toolkits.html#gs.mew6kj
にアクセスして[Intel oneAPI Base ToolKit]をクリック
ダウンロード画面に飛んで各種の設定

参照サイト
- https://qiita.com/implicit_none/items/35bc4be8f2022903747a


## nvidia driver
