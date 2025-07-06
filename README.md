# docker-dtv-server

Dockerで構築するMirakurun + EDCB + KonomiTVなTV視聴・録画環境

## 技術スタック

- [Mirakurun](https://github.com/Chinachu/Mirakurun)
- [kazuki0824/recisdb-rs](https://github.com/kazuki0824/recisdb-rs)
- [tsukumijima/ISDBScanner](https://github.com/tsukumijima/ISDBScanner)
- [xtne6f/EDCB](https://github.com/xtne6f/EDCB)
- [EDCB_Material_WebUI](https://github.com/EMWUI/EDCB_Material_WebUI)
- [BonDriver_LinuxMirakc](https://github.com/matching/BonDriver_LinuxMirakc)
- [KonomiTV](https://github.com/tsukumijima/KonomiTV)
- [Docker](https://www.docker.com/)

## Getting Started

### 前提条件

- ホストPC上に以下のものが必要です：
  - Docker
  - px4_drvなどのチューナードライバ
- ホストOSはUbuntu 24.04 LTSを想定しています。

### インストール

#### recisdb 

```bash
wget https://github.com/kazuki0824/recisdb-rs/releases/download/1.2.3/recisdb_1.2.3-1_amd64.deb
sudo apt install ./recisdb_1.2.3-1_amd64.deb
rm ./recisdb_1.2.3-1_amd64.deb
```

#### ISDBScanner

```bash
sudo wget https://github.com/tsukumijima/ISDBScanner/releases/download/v1.3.2/isdb-scanner -O /usr/local/bin/isdb-scanner
sudo chmod +x /usr/local/bin/isdb-scanner

isdb-scanner --exclude-pay-tv ./scanned/
```

#### Mirakurun

```bash
cp ./scanned/Mirakurun/tuners.yml ./mirakurun/conf
cp ./scanned/Mirakurun/channels.yml ./mirakurun/conf
```

#### EDCB

```bash
cp ./EDCB/edcb/Common.ini.example ./EDCB/edcb/Common.ini
cp ./EDCB/edcb/EpgDataCap_Bon.ini.example ./EDCB/edcb/EpgDataCap_Bon.ini
cp ./EDCB/edcb/EpgTimerSrv.ini.example ./EDCB/edcb/EpgTimerSrv.ini
cp ./EDCB/edcb/RecName_Macro.so.ini.example ./EDCB/edcb/RecName_Macro.so.ini
cp ./scanned/EDCB-Wine/BonDriver_mirakc\(BonDriver_mirakc\).ChSet4.txt ./EDCB/edcb/Setting/BonDriver_LinuxMirakc\(LinuxMirakc\).ChSet4.txt
cp ./scanned/EDCB-Wine/BonDriver_mirakc_S\(BonDriver_mirakc\).ChSet4.txt ./EDCB/edcb/Setting/BonDriver_LinuxMirakc_S\(LinuxMirakc\).ChSet4.txt
cp ./scanned/EDCB-Wine/BonDriver_mirakc_T\(BonDriver_mirakc\).ChSet4.txt ./EDCB/edcb/Setting/BonDriver_LinuxMirakc_T\(LinuxMirakc\).ChSet4.txt
cp ./scanned/EDCB-Wine/ChSet5.txt ./EDCB/edcb/Setting/ChSet5.txt
```

EpgTimerSrv.iniの以下の項目はお使いのチューナーに合わせて変更してください。

```ini
[BonDriver_LinuxMirakc.so]
Count=0    ; チューナー数
GetEpg=0
EPGCount=0 ; EPG取得に使用するチューナー数
Priority=0

[BonDriver_LinuxMirakc_T.so] ; 地デジ
Count=2
GetEpg=1
EPGCount=1
Priority=1
[BonDriver_LinuxMirakc_S.so] ; BS/CS
Count=2
GetEpg=1
EPGCount=1
Priority=2
```

デフォルトの録画保存先ディレクトリは`EDCB/record`に設定されています。必要に応じてcompose.yamlのedcbサービスのボリューム設定を変更してください。

```yaml
      - type: bind
        source: "./EDCB/record" # ここを変更
        target: "/record"
```

#### KonomiTV

KonomiTV用に`KonomiTV`ディレクトリ以下にconfig.yamlを作成してください。

```bash
cp ./KonomiTV/config.example.yaml ./KonomiTV/config.yaml
```


#### 起動

設定が完了したら、以下のコマンドでDockerイメージをビルドし、コンテナを起動します：

```bash
docker compose up -d
```

## 紹介記事

fork元の紹介記事はこちら

[Dockerで構築！ミニPC + PX-W3U4でお手軽自宅録画サーバー](https://zenn.dev/nunawa/articles/ecb9ef2e237532)

## ライセンス

docker-dtv-serverは、MITライセンスのもとで公開されています。
