# implementation_plan

## 位置づけ
- 本ファイルは実装順の要約。
- 詳細タスク/DoD/依存関係は `docs/action_plan.md` を参照。

## Step 1: docs固定
requirements/architecture/bom/wiring/gpio/power/safety/control/protocol/open_questions

## Step 2: firmware雛形
- PlatformIO
- SAFE_BOOT状態
- Serial JSON Lines

## Step 3: 単体試験
IMU -> Motor -> Encoder -> Battery -> Safety

## Step 4: 倒立
PDで短時間自立、ログベースで反復

## Step 5: 操作拡張
USB Serial -> UDP -> PS5 bridge
