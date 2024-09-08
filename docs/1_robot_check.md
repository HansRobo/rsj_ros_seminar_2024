# ロボットの動作確認

キーボード操作でロボットを動かし、正常に動作するか確認します。

## 手順

1. TurtleBot3の電源を入れ、ROS 2の環境を設定します。
   ```
   source /opt/ros/humble/setup.bash
   export TURTLEBOT3_MODEL=burger
   ```

2. ロボットとPCを同じネットワークに接続し、SSH接続を確立します。
   ```
   ssh ubuntu@{TURTLEBOT_IP}
   ```

3. ロボット側でノードを起動します。
   ```
   ros2 launch turtlebot3_bringup robot.launch.py
   ```

4. PC側でキーボード操作ノードを起動します。
   ```
   ros2 run turtlebot3_teleop teleop_keyboard
   ```

5. キーボードを使用してロボットを操作し、前後左右の動きを確認します。


## 動作しない場合の確認ポイント

ロボットが期待通りに動作しない場合、以下の点を確認してください：

1. ネットワーク接続
   - pingコマンドでロボットとの通信を確認:
     ```
     ping {TURTLEBOT_IP}
     ```
     正常な出力例:
     ```
     64 bytes from 192.168.1.100: icmp_seq=1 ttl=64 time=0.5 ms
     64 bytes from 192.168.1.100: icmp_seq=2 ttl=64 time=0.6 ms
     ```
     この出力が得られない場合、ネットワーク接続を確認してください。

2. ROS 2の環境設定
   - ROS 2のバージョンとTurtleBot3モデルの設定を確認:
     ```
     printenv | grep ROS
     printenv | grep TURTLEBOT3_MODEL
     ```
     正常な出力例:
     ```
     ROS_VERSION=2
     ROS_PYTHON_VERSION=3
     ROS_DISTRO=humble
     TURTLEBOT3_MODEL=burger
     ```
     これらの環境変数が正しく設定されていない場合、再度sourceコマンドやexportコマンドを実行してください。

3. ノードの起動状態
   - 実行中のノードを確認:
     ```
     ros2 node list
     ```
     正常な出力例:
     ```
     /turtlebot3_node
     /teleop_keyboard
     ```
     これらのノードが表示されない場合、対応するlaunchファイルやrunコマンドを再実行してください。

4. トピックの通信
   - `/cmd_vel`トピックの通信を確認:
     ```
     ros2 topic echo /cmd_vel
     ```
     キーボード操作時の正常な出力例:
     ```
     linear:
       x: 0.1
       y: 0.0
       z: 0.0
     angular:
       x: 0.0
       y: 0.0
       z: 0.0
     ---
     ```
     この出力が表示されない場合、テレオペレーションノードが正しく機能していない可能性があります。

5. ロボットの状態
   - バッテリー状態の確認:
     ```
     ros2 topic echo /battery_state
     ```
     正常な出力例:
     ```
     voltage: 11.1
     percentage: 75.0
     ```
     電圧が9V以下の場合、バッテリーの充電が必要です。

   - モーターの状態確認:
     ```
     ros2 topic echo /joint_states
     ```
     正常な出力例:
     ```
     name: [wheel_left_joint, wheel_right_joint]
     position: [1.5, -1.5]
     velocity: [0.1, -0.1]
     ```
     位置や速度が更新されない場合、モーターやエンコーダーに問題がある可能性があります。

6. エラーメッセージの確認
   - ROSのログを確認:
     ```
     cat ~/.ros/log/latest/rosout.log
     ```
     エラーメッセージやWARNINGを探し、それに基づいて対処してください。

7. パーミッションの確認

   - USBデバイスのパーミッションを確認:
     ```
     ls -l /dev/ttyUSB*
     ls -l /dev/ttyACM*
     ```
     出力例:
     ```
     crw-rw---- 1 root dialout 188, 0 May 1 12:00 /dev/ttyUSB0
     ```
     'dialout'グループに読み書き権限があることを確認してください。


8. ROSトピックとサービスの詳細な確認
   - 利用可能なすべてのトピックを表示:
     ```
     ros2 topic list -t
     ```
     この出力で、期待されるトピック（例：/cmd_vel, /odom）が存在するか確認します。

   - 特定のトピックの詳細情報を確認:
     ```
     ros2 topic info /cmd_vel
     ```
     出力例:
     ```
     Type: geometry_msgs/msg/Twist
     Publisher count: 1
     Subscription count: 1
     ```
     これにより、トピックの型、パブリッシャーとサブスクライバーの数が分かります。

   - 利用可能なサービスを表示:
     ```
     ros2 service list -t
     ```
     TurtleBot3の重要なサービス（例：/spawn）が表示されることを確認します。

9. ノードの詳細情報
   - 特定のノードの情報を確認:
     ```
     ros2 node info /turtlebot3_node
     ```
     出力例:
     ```
     /turtlebot3_node
      Subscribers:
        /cmd_vel: geometry_msgs/msg/Twist
      Publishers:
        /odom: nav_msgs/msg/Odometry
        /tf: tf2_msgs/msg/TFMessage
      Services:
        /turtlebot3_node/describe_parameters
        /turtlebot3_node/get_parameter_types
        /turtlebot3_node/get_parameters
        /turtlebot3_node/list_parameters
        /turtlebot3_node/set_parameters
        /turtlebot3_node/set_parameters_atomically
     ```
     これにより、ノードのサブスクライバー、パブリッシャー、サービスの詳細が分かります。

10. パラメーターの確認
    - ノードのパラメーターを確認:
      ```
      ros2 param list /turtlebot3_node
      ```
      出力例:
      ```
      /turtlebot3_node:
        use_sim_time
        wheel_radius
        wheel_separation
      ```

    - 特定のパラメーターの値を確認:
      ```
      ros2 param get /turtlebot3_node wheel_radius
      ```
      出力例:
      ```
      Double value is: 0.033
      ```
      パラメーターが期待値でない場合、適切に設定し直してください。

