# AWS Hands-on：簡単NetApp Cloud Solution (ONTAP Cloud編) 

[TOC]

## 実施概要(ONTAP Cloud 編）
<!--
- フェーズ：awareness /Partner enablement
- 目標集客: 15 - 20 人(クラウドリソースの制限、エンドユーザ）
- 費用:12.5万（お弁当代除く）
-->

まずは「体験する」を目的とするハンズオンです。
細かい動作を確認するよりは **「なにができるのか」** を体験する内容です。
体験いただき、できることを確認してもらうのが本ハンズオンの目的です。

## 実施する完成形のシステム
今回のハンズオンの環境概要です。
VPC内部に２つのパブリックサブネットを準備し、各サブネット上にONTAP Cloud をデプロイします。
![](images/15059677571678.jpg)

## Hands-on overview

- OnCommand Cloud Manager をデプロイする
- OTANP Cloud をデプロイする
- ONTAP Cloud の操作を行う

<!--
時間に余裕があれば以下のハンズオンを準備しています。
- CloudSync を使ってONTAP Cloud のNFSのデータから S3へデータを転送しRedShift で分析
- NPS からデータを参照し分析(NFSをデータソースとする）
-->

## OCCM のセットアップ
配布しているIPリストのOCCM欄のIPを確認ください。
アクセス時には

- https://IPアドレス

でアクセスできます。

![](images/15059626889397.jpg)
OCCMの画面から「Storage System View」をクリックすると以下の画面になるので「Set up new Cloud Manager」をクリックします。

![](images/15042610212109.jpg)

初期セットアップの画面が表示されます。

### サイト情報の入力
![](images/15042610791521.jpg)

入力項目としては以下の２つとなります。

| 入力項目 |説明 |入力値 |
| --- | --- |--- |
| Site | 任意入力 |PoC|
| Company | 任意入力 |NetApp K.K.|

入力後、「Continue」をクリックしユーザ情報登録画面へ。
![](images/15042612140889.jpg)

### ユーザ情報の登録

| 入力項目 | 説明|入力値 |
| --- | --- |--- |
| First Name | 名前 |ご自身のお名前|
| Last Name | 名字 |ご自身の名字|
| Email address(username) | ユーザ名として使用するメールアドレス |ご自身のメールアドレス|
| Password | 任意のパスワード |NetApp1!|

![](images/15042612533127.jpg)

### AWS Permission 画面

| 入力項目 |説明|入力値 |
| --- | --- |--- |
| AWS Access Key | ユーザ作成時にダウンロードできるアクセスキー | ダウンロードファイルをご確認ください|
| AWS Secret Key | ユーザ作成時にダウンロードできるシークレットキー| ダウンロードファイルをご確認ください|
| AWS Cost S3 Bucket | オプション、課金レポートを保存するS3バケットを指定|今回は入力なし |

![](images/15042616263059.jpg)

### テナントの作成

| 入力項目 | 説明|入力値 |
| --- | --- |--- |
| Name | テナント名|ONTAP PoC01|
| Description | 概要記入|今回は入力なし|
| Cost Center | コストセンター|今回は入力なし|

![](images/15042616462494.jpg)

### NetApp サポートサイトの登録情報
| 入力項目 | 説明|入力値 |
| --- | --- |--- |
| NSS User ID | ネットアップサポートサイトID|今回は入力なし|
| NSS Password | サポートサイトのパスワード|今回は入力なし|

![](images/15042616561242.jpg)

### 設定情報の確認

今までの設定情報の確認ページが表示されます。
![](images/15042616708430.jpg)

「End User License Agreement」 を読み、承認したら「I have read 〜」にチェックを入れ、「Go」をクリック。
![](images/15042616799924.jpg)
以上でOCCMの設定が完了します。
約5分で以下の画面になり、ONTAP Cloud をデプロイできるようになりました。
![](images/15042619032199.jpg)

## ONTAP Cloud のデプロイ

 OCCM から ONTAP Cloud をAZ毎にデプロイします。

OCCMの画面の左上の Add Environmentをクリックします。
![AddOTC](images/15042622234154.jpg)

一番左のONTAP Cloudを選択しすると次の画面に遷移します。
![SelectOTC](images/15042622564118.jpg)

| 入力項目 |説明| 入力値 |
| --- | --- |--- |
| Working Environment Name | 作業環境 | ONTAP01|
| Tag Key | 識別子 | PoC1|
| Password | パスワード|NetApp1!|
| ConfirmPassword | 確認用パスワード|NetApp1!|

「Add Tags」 は　AWS　のタグとして使用される、任意入力。

![NewWorkingEnvironment](images/15042623795734.jpg)
入力項目を入力後、「Continue」をクリック。

![](images/15042624326104.jpg)

| 入力項目 |説明| 入力値 |
| --- | --- |--- |
| AWS region | デプロイするリージョン | Asia Pacific \| Tokyo を選択|
| VPC | デプロイするVPC| 10.0.0.0/16を選択| 
| SSH authentication method| SSH の認証方法 |Key Pair を選択|
|Key Pair| キーペア名| OTC を選択|

![](images/15059636243421.jpg)

暗号化手法、今回は暗号化なしで実施「None」を選択

暗号化の種別は３つ

- NONE: なし
- ONTAP Cloud Manage: OCCMで鍵管理サーバを構築して暗号化を実施
- AWS 機能を使った暗号化

![](images/15051118793525.jpg)


BYOLラインセンスを使用するかの選択、今回のハンズオンでは「No」を選択。

![](images/15042626835223.jpg)

プレコンフィグレーションの画面に遷移。
今回は体験を目的としているため、「POC and 〜」を選択してデプロイします。
![](images/15051119740378.jpg)
カーソルを載せると以下の画面になります。
![](images/15059637759818.jpg)

持っていればサポートサイトのIDを入力。
![](images/15051120684324.jpg)

| 入力項目 |説明| 入力値 |
| --- | --- |--- |
| Volume Name | 初期作成するボリューム名 | vol01|
| Size(GB) | 作成するボリュームのサイズ| 1| 
| Protocol| ONTAP Cloud で作成するシェアへのアクセスプロトコル |NFSを選択|
|Access control| 作成したボリュームにアクセスできるネットワーク| 変更なし、初期値でデプロイしたVPCのネットワークアドレスが指定されている|
|Snapshot policy| スナップショット取得の設定| 変更なし、初期値|
|Usage Profile|容量効率化を使うかの選択| 変更なし、Storage Efficiencyが設定されている|

「Continue」をクリック。

![](images/15059666052955.jpg)

確認画面が表示されるので、それぞれのタブで情報を確認「Overview」, 「Networking」、「Storage」をクリックして内容をみます。

Overview画面
![](images/15059667220630.jpg)

Networking画面
![](images/15059667583012.jpg)

Storage画面
チェックボックスをチェックし、「Go」をクリック
![](images/15059668105333.jpg)

ONTAP Cloud のデプロイが開始されます。
以下はデプロイ中に画面です。
![](images/15051124144070.jpg)

Timelineタブから進捗を確認できます。。
![](images/15059669887487.jpg)

約25分後にプロビジョニングが完了し以下の画面になります。
![](images/15051138315123.jpg)

以上で ONTAP Cloud のデプロイが完了です。

## ONTAP Cloud の機能に触れてみる

一通り設定できる項目について確認します。
中心の雲をクリックすると以下のような画面になります。

![](images/15051139190334.jpg)



### Volumeを追加する
「Add Capacity 」をクリックすると以下の画面になります。
![](images/15051141112145.jpg)
以下の入力項目があります。
それぞれ入力し、「Continue」をクリックします。

| 入力項目 |説明 |入力値 |
| --- | --- |---|
| Volume Name | 作成するボリュームの名前 | vol02|
| Size (GB)  | 作成するボリュームのサイズ| 1 (GB) |
| Protocol | 使用プロトコル| NFS|
| AccessControl | アクセスできるネットワークを指定| 10.0.0.0/16 (自動生成)|

![ボリューム作成画面](images/15059702559144.jpg)
注意点: 最初のサイズの1000% までしかデフォルトでは拡張できません、1000% が最大値となります。

画面では使用するEBSのタイプを選択します。
ここでは「General Purpose SSD」を選択し、「Go」をクリックします。
![作成ボリュームのプ選択](images/15059708144210.jpg)

ボリュームの作成が完了すると「Wokring Environment」画面に戻ります。
先程作成したボリュームを確認するため、中心にある「ONTAP01」の雲をクリックします。
以下の画面が右側に現れるので、「Resource」をクリックし存在するボリュームを確認します。

![Resource](images/15059710505185.jpg)

Resouce をクリックすると以下の画面が表示されるので、追加したボリュームが表示されていることを確認します。
![VoluemView](images/15059711761133.jpg)


ここからはボリュームへどのような操作ができるかを確認します。

![](images/15059712026819.jpg)

以降の手順でいくつかピックアップして試します。

### FlexCloneを実行してみる
ボリューム一覧画面からクローンするボリュームにカーソルを当てるとメニューバーが表示されるのでクリックをします。今回はvol01を対象とします。

![focuson](images/15059725218884.jpg)

「clone」をクリックします。
![VolumeMenu](images/15059725858750.jpg)

以下の画面に遷移します。

| 入力項目 | 説明 | 入力値 |
| --- | --- |--- |
| Clone Volume Name | クローン後のボリューム名 | vol01_clone (初期設定）|

![CloneName](images/15059750876360.jpg)

「clone」ボタンをクリックします。
クローンの速度が速いことを確認してください。
今回はデータが入っていない状況ですが、データが100GB, 1TB であっても同じ速度で実行できます。

クローンに成功するとボリューム一覧画面に戻ります。
「vol01_clone」がボリューム一覧に表示されていることを確認してください。
![VolumeList](images/15059728162817.jpg)

### SnapMirrorを試す！
非同期レプリケーション機能であるSnapMirrorを設定します。
SnapMirrorの使い所としては、AZ 間のデータ保護、災害対策用に遠隔地に保管するといった使い方が可能です。

また、オンプレミスとAWS 間でもSnapMirrorでデータをレプリケートできるためオンプレミスの災害対策先としてAWSを選択することも可能になります。

#### SnapMirrorのデータ転送先のONTAP Cloud （２つ目）をデプロイする
「Working Environments」タブをクリックし以下の画面に戻ります。
![WorkingEnv](images/15059731835382.jpg)

「Add environment」をクリックし２つ目のONTAP Cloudをデプロイします。

![OTCDeploy](images/15059732531763.jpg)

| 入力項目 | 説明 | 入力値 |
| --- | --- |--- |
| Working Environment Name | 作業環境 | ONTAP02|
| Tag Key | 識別子 | PoC2|
| Password |パスワード | NetApp1! |
| Confirm Password |確認用パスワード | NetApp1! |
![ConfigWorkingEnv](images/15059734064043.jpg)
「Continue」をクリック

| 入力項目 | 説明 | 入力値 |
| --- | --- |--- |
|AWS  region  | デプロイするリージョン | Asia Pacific  \| Tokyo |
| VPC | デプロイするVPC | 10.0.0.0/16 |
| Subnet | デプロイするSubnet| 10.0.1.0/24 | 
| Security Group | 適応するSecurityGroup | Use a generated security group (自動生成) | 
| SSH authentication method | SSH認証の方法 | Key Pair | 
| Key Pair | 使用するKey Pair| OTC | 

![DeployVPC](images/15059735571754.jpg)
「Continue」をクリック。

暗号化方式の選択、「NONE」を選択。
![EncryptionMethod](images/15059738790573.jpg)
「Continue」をクリック

BYOLライセンスを使用するか、「No」を選択
![BYOL](images/15059741368619.jpg)

ONTAP Cloud のエディションを選択、一番左の「Poc and small workloads」をクリック。
![OTCPreconfig](images/15059739730527.jpg)

ネットアップサポートサイトの認証情報、今回は何も入力せずに「Continue」をクリック。
![SupportSite](images/15059742167837.jpg)

ボリュームの作成は「Skip」。
![Skip](images/15059743337990.jpg)

確認画面で、チェックボックスにチェックをいれ「Go」をクリック。
![Confirmation](images/15059744033089.jpg)

デプロイ完了後に実施 (約25分)
以上でSnapMirrorの準備が整いました。

#### SnapMirrorの設定をする
Working Environments の画面で操作をします。
右側のONTAP01 をドラッグして、左側のONTAP02にドロップします。
![OnCommand_Cloud_Manage](images/OnCommand_Cloud_Manager.jpg)

送信元となるONTAP01のボリュームを選択する画面になります。
ここでは「vol01」をクリックします。
![SnapMirror_Sr](images/SnapMirror_Src.jpg)

次に送信先となるONTAP02 の設定になります。
送信先のボリューム作成画面になります。
ここでは「General Purpose SSD」を選択します。
![SnapMirrorDest](images/SnapMirrorDest.jpg)
「Continue」をクリックします。

転送速度のMax値を指定します。今回は特に指定せずそのまま「Continue」をクリックします。
![SnapMirrorLimit](images/SnapMirrorLimit.jpg)

データをミラーするのか、バックアップするかの選択画面となります。
今回はミラーの設定とするため、左側の「Mirror」を選択します。
![MirrorConfig](images/MirrorConfig.jpg)

ミラーのスケジュールが可能です、今回は一度のみのコピーとします。
「One-time copy」を選択します。
![SnapSchedule](images/SnapSchedule.jpg) 次の画面に遷移し、今まで設定した内容の確認画面となります。
内容が確認できたらチェックボックスにチェックを入れ「Go」をクリックします。
![Review](images/Review.jpg)

SnapMirrorの関係、データの転送を実行中の際には以下のような画面になります。
![SnapMirrorRelations](images/SnapMirrorRelations.jpg)
進捗状況は 「TImeline」から確認できます。

SnapMirror実施中はStatus欄が「In Progress」で表示されます。
![SnapMirrorStatus_InProgress](images/SnapMirrorStatus_InProgress.jpg)

SnapMirror関係、データ転送が完了すると Status 欄が「Complete」となります。
![SnapMirrorStatusComplete](images/SnapMirrorStatusComplete.jpg)

「Replication Status」をクリックするとSnapMirrorの状況を確認することができます。
![ReplicationStatus](images/ReplicationStatus.jpg)

SnapMirrorはドラッグ＆ドロップで実現することが可能です。

<!-- TODO phase 2 対応
## NPSを触ってみる

- CloudManager から接続

### Discover で NetApp Private Storage の LIF 
-->

### サーバからマウント
#### サーバのマウントコマンドの確認

Working EnvironmentsからONTAP01をクリックし、QuickNavigationから「Resources」を選択します。
ONTAP01のVolume 一覧画面から vol01のメニューを表示します。
![VolMenu](images/15059749450589.jpg)

その中から 「Mount Command」をクリックするとNFSであればサーバから実施するマウントコマンドを確認可能です。
![Mount](images/15059751261063.jpg)

### まとめ
以上で ONTAP Cloud    を実際触ってみるハンズオンでした。

以下のことを本ハンズオンでは実施しました。

-  OnCommand Cloud Manager (OCCM) のデプロイ
-  ONTAP Cloud のデプロイ
-  ONTAP Cloud でのボリュームの作り方
-  FlexCloneの仕方
- SnapMirrorの実施方法

今回はまずは触ってみるをテーマとしていますので、
もう少し触ってみたい、GUIからではなくAPIでやりたい、細かい使い方を知りたいという方はぜひご連絡いただければと思います。

<!--
## CloudSyncをためす！
Working Environments から ONTAP01をクリックし、、右の「Resources」→遷移後に「Sync to S3」をクリックします。
- なにができるか
- これまでのHands-onで作ったONTAP Cloud NFS -> S3
- S3 bucket は作成する
- S3 data lake architecture のファミリー（分析系がいいとおもう）
-->

## Appendix
本ハンズオンを実行するための事前準備をAppendixとしました。

### AWS VPC 設計
今回はハンズオンのため簡易的なネットワーク設計としています。

### AWS環境の作成 

- CloudFormation提供 作成・削除
- VPC/NAT gateway
- Jumphost/Linux hosts

### OnCommand Cloud manager のデプロイ

AWS Marketplace から OnCommand Cloud Manager をデプロイ
- https://aws.amazon.com/marketplace/pp/B018REK8QG

![MarketPlaceの画面](images/15059609255408.jpg)
「Continue」 をクリック

![](images/15059613471006.jpg)

「Launch with 1Click」をクリックすると OnCommand Cloud Manager のデプロイが開始されます。
![Launch](images/15059614100018.jpg)

EC2 コンソールページに移動して状況を確認します。

起動が完了したらわかりやすいように名前をOCCMと変更します。

![EC2Console](images/15042312154965.jpg)

### CloudManager IP の確認

EC2コンソールから対象のCloudManager をクリックします。下部のインスタンス情報にプライベートIP、パブリックIPが記載されます。本ハンズオンでは、CloudManager へ直接アクセスできるように設定をしています。（参考: Appendix: EIPを設定する）

![](images/15059625500188.jpg)

OCCMのIPアドレス：(メモ）

操作しているブラウザから上記でメモしたIPにアクセスします。

https://_IPアドレス_　をブラウザで起動
以下のような画面が表示されればデプロイ完了です。

![](images/15059626032327.jpg)

### 管理画面

#### Tabular View

![](images/15051140491614.jpg)

#### Write Speed

![](images/15051233762670.jpg)

#### ボリュームの追加
![](images/15051234338790.jpg)

#### インスタンスの一覧
![](images/15051234771995.jpg)


#### CloudSync のラインセンス確認
ラインセンス確認
![](images/15053811260646.jpg)


#### 注意点/Tips
- volumeの拡張は初期サイズの10倍まで
- システム時間がデフォルトはUTC なのでSystemManagerで日本時間へ変更する


#### Key Pair 作成
![](images/15059611217264.jpg)
pemファイルがダウンロードされるので大事に保管してください。今後の構築に使用します。

#### EIPを設定する
![](images/15059621744194.jpg)
![](images/15059621933478.jpg)

Actions -> Associate addressを選択
![](images/15059622426531.jpg)

NetworkInterfaceに対してEIPを設定
![](images/15059622869516.jpg)
![](images/15059623186296.jpg)

EIPを確認、左のメニュー「Instances」からOCCMを選択
Elastic IP を確認
![](images/15059623685418.jpg)

#### CloudFormationを実行する
本ハンズオンで使用するネットワーク構成はCloudFormationとして提供しています。
以下のURLからダウンロードして、CloudFormationを実行するとVPCの作成を実施します。

<!--
#### Pre confiugration 以外の方法
Create my own configuration だと任意のライセンスタイプやインスタンスタイプを選択可能

![](images/15042639955860.jpg)
 ![](images/15042640976870.jpg)
![](images/15042642013479.jpg)
![](images/15042642214835.jpg)
-->


