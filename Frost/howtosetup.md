# 研究のためのPCセットアップ

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
