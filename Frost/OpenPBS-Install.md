# OpenPBS 簡易導入手順（Rocky Linux 9 / OpenHPC 3.2）
本ドキュメントは、単一ノード（管理ノード、計算ノードが同一）環境における OpenPBS のインストールとテストジョブ実行までの流れをまとめたものです。OpenHPC経由でインストールした理由ネットで調べると、 RockyLinux9.x にOpenPBS　をインストールするのはこの方法が簡単だという意見があったため。

## 構成
OS: Rocky Linux 9.5 (x86_64)

環境: 単一ノード構成

ユーザー: root（管理者ユーザー）、ando（一般ユーザー）

OpenHPC リポジトリ経由で openpbs-server-ohpc を導入


## 導入手順
1. OpenHPC リポジトリの追加

```bash
$ sudo dnf install https://repos.openhpc.community/OpenHPC/3/EL_9/x86_64/ohpc-release-3-1.el9.x86_64.rpm

$ dnf makecache
 ```

2. OpenPBS サーバのインストールと起動
```bash
# source /etc/profile.d/pbs.sh PBSコマンド用のパスと環境変数を設定
# dnf install -y openpbs-server-ohpc
# systemctl enable pbs
# systemctl start pbs　
```
- OpenPBS サーバが動いているか確認
```bash
# systemctl status pbs
```


3. qmgr サーバ設定
```bash
# qmgr -c "set server default_qsub_arguments = -V"
# qmgr -c "set server resources_default.place = scatter"
# qmgr -c "set server job_history_enable = True"
```

4. ノード登録（管理ノード自身を計算ノードとして使う）
```bash
# qmgr -c "create node frost"
```
--- 

**Note**

初期状態では誤って ```/usr/local/bin/qmgr```（古いPBSやTorqueの残骸）が優先され、以下のエラーが発生：
```
qmgr: cannot connect to server could not connect to trqauthd Unauthorized Request
```

解決策

```/usr/local/bin/qmgr``` を削除して```/opt/pbs/bin/qmgr```に変更する。
```bash
echo 'source /etc/profile.d/pbs.sh' >> ~/.bashrc
echo 'export PBS_SERVER=frost' >> ~/.bashrc
```
---

5. ジョブ実行（テスト）

- テストジョブスクリプト（test.sh）
```
#!/bin/bash
#PBS -N test_job
#PBS -l select=1:ncpus=1
#PBS -j o
hostname
sleep 5
```
- 実行
```bash
$ chmod +x test.sh
$ qsub test.sh
$ qstat
$ cat test.o0
```
- 出力結果

---


### 参考
1. [OpenHPC GitHub Wiki](https://github.com/openhpc/ohpc/wiki/3.x)

2. [OpenHPCインストールガイド（PDF）](./ref/Rocky9WarewulfOpenPBS3.3InstallGuide.pdf)
