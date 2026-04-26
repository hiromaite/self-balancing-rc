# communication_protocol

## 形式
- JSON Lines（Serial）
- JSON over UDP

## 推奨フィールド
- cmd, vx, wz
- command_seq
- host_ts_ms
- device_ts_ms
- ttl_ms

## 安全
- ttl切れコマンドは破棄
- 通信断時は自動停止
