# マップを用いたナビゲーション

## ナビゲーション

!!! note
    TURTLEBOT3_MODEL環境変数の設定は省略しています。.bashrcに設定を以下のコマンドで記述しておきましょう。
    ```bash
    echo "export TURTLEBOT3_MODEL=burger" >> ~/.bashrc
    ```

### TurtleBot側の立ち上げ
``` bash
ssh ubuntu@192.168.xx.xxx
ros2 launch turtlebot3_bringup robot.launch.py
```

### PC側でのナビゲーションの立ち上げ

``` bash
ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=$HOME/map.yaml
```

!!! note
    `map.yaml`のパスは自分の環境に合わせて変更してください。

###  初期位置の設定

読み込まれた地図と、現実世界を照らし合わせて、TurtleBot3の初期位置を設定します。

1. Rvizの「2D Pose Estimate」ボタンをクリックします。
2. マウスでロボットの現在位置をクリックし位置を設定、引き続きドラッグして向きも設定します。
3. キーボード操作でロボットを動かし、自己位置推定を収束させます。
   1. `ros2 run turtlebot3_teleop teleop_keyboard`

### ゴール位置の設定

1. Rvizの「2D Nav Goal」ボタンをクリックします。
2. 初期位置設定と同じ要領で、ゴールをクリック＆ドラッグでゴールの位置と向きを設定します。

### ナビゲーションで遊ぼう

- 障害物の裏に回り込むようにゴールを設定してみましょう。
- 地図にない障害物を追加して、障害物の回避を確認してみましょう。


<!-- 作成したマップを使用して、TurtleBot3の自律ナビゲーションを行います。

基本は[公式ガイドのNavigationセクション](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#navigation)にしたがってください。


=== "実機"

    ``` bash
    ```

=== "シミュレーション"

    ``` bash
    ``` -->