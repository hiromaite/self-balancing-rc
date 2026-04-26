# architecture

## システム構成
- 車体: M5StampS3A + IMU + Encoder + 2x Motor Driver
- 操作: USB Serial / Wi-Fi UDP
- 将来: PS5はPC/RPi中継

## 制御ループ目標
- 姿勢ループ: 250-500Hz
- 速度ループ: 50-100Hz
- 通信: 20-100Hz

## 状態機械
SAFE_BOOT -> CALIBRATING -> ARMED_READY -> BALANCING -> FAULT_LATCHED
