# pro_hand_book_python
書籍出納システムの構築

xArm7コマンドにコマンドが色々載っている．

#　構成
Retrieval_* 系: 取り出し
Storage_* / Strage_integration.py: 収納
infer.py: マスター連携・連続実行
linear_lift.py: リフト制御
barcode, detection, xarm7, Dynamixel_win_pro_hand_book, rs_d435i, ros2_ws: 各サブシステム
という構成

## 出庫関連（取り出し）
- Retrieval_by_Detection.py
    XArm7 と RealSense で本の両端を検出し、グリッパで取り出すシーケンス

- Retrieval_by_Detection_A4.py

    UR5e ロボット版の書籍取り出しシーケンス
    A4 専用と思われる初期姿勢 / 収束処理が含まれる

- Retrieval_by_Detection_offset.py

    UR5e 版で、座標オフセット付きの取り出し処理
    取出し位置補正や回転補正が入っている

- Retrieval_integration.py

    ROS2 と連携した統合出庫シーケンス棚 ID 受信、バーコード、自律位置合わせなどを含む複雑統合処理

- Retrieval_integration_editing.py

    Retrieval_integration.py の編集版・改良版と思われるこちらも統合出庫シーケンスを扱う

- retrieval_test.py

    手動入力で書籍端の UV 座標を与え、UR5e で取り出しテストを行うデバッグ用の簡易版

## 収納関連
- Storage_by_Detection2.py
    XArm7 と RealSense で本を棚に収納するシーケンスHandBook_Retrieval のグリッパ制御も使っている
    
- Strage_integration.py
    収納の統合版ROS2、バーコード読み取り、ウェイポイント再生、ログ記録などを含むフル統合処理

## 補助・テスト
- infer.py
    マスター JSON を読み込み、書籍名情報から連続的に処理を進める想定
    linear_lift.TargetPublisher を使い、上下機構制御と連動する構造

- linear_lift.py
    ROS2 ノード
    /target_mm というトピックに目標値を publish するだけの上下機構制御用ノード

## 重要なフォルダ
- barcode
    棚バーコード検出・認識や、バーコード関連のサンプルShelfBarcodeDetection.py など

- detection
    SAM や点群解析を使った書籍認識処理
    pro_handbook/sam_py_demo/ 以下に多くの認識モジュールがある

- xarm7
    xArm7 ロボット固有の制御コード
    control 下に動作シーケンス、Waypoint、座標変換など多数

- Dynamixel_win_pro_hand_book
    Dynamixel グリッパ・手制御用ライブラリ
    HandBook_Retrieval.py / HandBook_Storage.py など

- rs_d435i
    RealSense カメラで本の位置を取得する処理
    get_book_position.py が中心
    
- ros2_ws
    ROS2 のワークスペース
    ノードやパッケージ、UDP ブリッジなどが入っている

- librealsense
    Intel RealSense SDK のソースコード一式
    これは外部ライブラリをそのまま含めた形

## その他
- master_20260216.json 
- master_20260225_tohan.json 
- master_2026107.json 
- master_2026109.json
    書籍マスターや棚情報を管理するデータファイル
    requirements*.txt
    Python の依存パッケージ一覧
- xArm7コマンド
    xArm7 向けのコマンド集・操作メモのように見える

# 主要な通信
- xArm7 → ロボット：TCP/IP ドライバ通信（192.168.2.197）
- Dynamixel（グリッパ）→ シリアル通信：モーター制御
- RealSense（カメラ）→ USB：RGB/Depth ストリーム
- ROS2 ノード間：DDS（分散データサービス）
