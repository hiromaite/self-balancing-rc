# wiring

## 電源
2S LiPo -> Main SW -> Fuse -> (Motor branch, 5V Buck branch)

## 方針
- Motor系と5V制御系を分岐
- GND共通
- IMU線と高電流線を分離
- ADC分圧は8.4V入力を安全範囲に収める
