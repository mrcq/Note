# 固件释放checklist

## Normal

- 扫描是否正确（long H/V）
- noise是否正常（频点）
- TPS余量是否够
- 补偿前数据是否在目标值附近（不是则调整vstim等参数）
- step是否合理（1cneg差值）

## MNT

- 补偿前数据是否在目标值附近（不是则调整vstim等参数）
- step是否合理（1cneg差值）

## LPWG

- LPWG时序是否正确
- 补偿前数据是否在目标值附近（不是则调整vstim等参数）
- step是否合理（1cneg差值）

## Others

- mapping调整

- 挖孔补偿
- 是否导入滤波算法（是否生效）
- 产测配置是否遗漏（跳帧、sram等）
- panel 行列是否正确