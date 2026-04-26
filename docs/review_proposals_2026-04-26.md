# 企画書レビュー提案（2026-04-26）

## 結論
現行企画書は非常に完成度が高く、**方向性は維持して問題なし**。ただし、成功率と安全性を上げるために以下の変更を推奨する。

## 変更提案（Change）
1. **制御ループ周波数目標を明文化**
   - 内側ループ（姿勢）: 250–500 Hz
   - 外側ループ（速度）: 50–100 Hz
   - 通信受信/送信: 20–100 Hz（用途別）
   - 理由: BNO08xは400 Hz級のレポート上限を持つため、制御周期設計を先に固定すると実装ぶれを防げる。

2. **BNO086とICM-42688-Pの役割分離を明示**
   - 初期成功優先: BNO086（rotation vector）
   - 最終性能優先: ICM-42688-P（低遅延生データ + 独自フィルタ）
   - 理由: フェーズ移行時の判断基準（遅延・ノイズ・復帰性）を明確化できる。

3. **フェイルセーフの状態機械を定義**
   - SAFE_BOOT → CALIBRATING → ARMED_READY → BALANCING → FAULT_LATCHED
   - FAULT_LATCHEDからは明示復帰のみ。
   - 理由: "転倒後の勝手な再起動"や"復帰条件の曖昧さ"を排除。

4. **ESP32-S3運用のシステム保護を追加**
   - Task Watchdog監視対象の明示
   - Brownout発生時のログ識別と復帰フロー
   - 理由: 実機デバッグ時の原因分離（制御崩壊 vs 電源問題）が容易になる。

5. **通信プロトコルに時刻・連番・TTLを追加**
   - command_seq, device_ts_ms, host_ts_ms, ttl_ms
   - 理由: UDP/Serial混在時に古いコマンド適用を防げる。

## 追加提案（Add）
1. `docs/open_questions.md` を初期から運用（購買前チェックと実測項目を分離）。
2. `docs/implementation_plan.md` を追加し、依存関係つきで実装順を固定。
3. 受入試験に「30分連続運用」「10回連続再起動」「低電圧境界試験」を追加。
4. ログ仕様に最低サンプリング項目と必須精度（ms単位時刻）を追加。

## 削除/後回し提案（Delete/Defer）
1. 初号機でのカメラ統合詳細設計は後回し（要件だけ残す）。
2. UI実装の技術選定は先送り（CLI優先でログ品質確保を先行）。

## オンライン知見（一次情報）
- ESP32-S3はBLEのみ対応（Classic非対応）。
  - https://docs.espressif.com/projects/esp-idf/en/v5.2.1/esp32s3/api-guides/bluetooth.html
  - https://documentation.espressif.com/api/resource/doc/file/rz94aWY3/FILE/esp32-s3_datasheet_en.pdf
- BNO08xの典型遅延・最大レポートレート。
  - https://docs.sparkfun.com/SparkFun_VR_IMU_Breakout_BNO086_QWIIC/assets/component_documentation/BNO080_085-Datasheet_v1.16.pdf
- ESP-IDFのWatchdog/Brownout（障害切り分けに重要）。
  - https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/system/wdts.html
  - https://docs.espressif.com/projects/esp-idf/en/v5.2/esp32s3/api-guides/fatal-errors.html
- Balancing機での80mm級ホイール実績（Balboa）。
  - https://www.pololu.com/docs/0J70/all

## 採否判断
- **採用推奨（高）**: 制御周期明文化、状態機械、通信TTL/連番、WDT/Brownout運用
- **採用推奨（中）**: 30分連続運用・再起動耐性試験
- **後回し推奨**: カメラ統合詳細、GUI最適化
