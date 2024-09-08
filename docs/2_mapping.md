# LiDARセンサーを用いたマッピング

マッピング用のノードを起動して、ロボットを手動で動かすことで環境マッピングを行います。

基本は[公式ガイドのSLAMセクション](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/#run-slam-node)にしたがってください。

## SLAMノードを起動

```bash
ros2 launch turtlebot3_cartographer cartographer.launch.py
```

## テレオペレーションノードの起動
   
別のターミナルを開いて以下のコマンドを実行します。
   
```bash
ros2 run turtlebot3_teleop teleop_keyboard
```

## ロボットの操作

ロボットを操作して部屋中を移動し、環境のマップを作成します。

## マップの保存

マップが完成したら、以下のコマンドでマップを保存します。

```bash
ros2 run nav2_map_server map_saver_cli -f ~/map
```

!!! tips
    「server」と「saver」の違いに注意！


!!! tips
    2回目以降同じパスを指定すると前回保存したマップが上書きされてしまうので、パスを変えて保存してください。


## マップの編集

思った通りにマップを作成できない場合、ペイントツールを用いてマップを直接編集して修正できます。

### GIMPのインストール

GIMPはマップで使われるPGM(Portable GrayMap)形式のファイルを開くことができるペイントツールです。

```bash
sudo apt install gimp
```
### GIMPでマップを編集

GIMPでマップファイルを開いてノイズを消したり障害物を編集したりします。

!!! tips
    マップの編集には「Tools>Paint Tools>Pencil」から選べる鉛筆ツールが便利です。


### 編集したマップを保存

編集したマップを保存する場合、Saveではなく「Files>Export as...」を選択してください。
ファイル名の拡張子を`.pgm`にすると自動的にpgm形式で保存されます。
保存するときPNMの保存形式を聞かれた場合「ASCII」を選択してください。

