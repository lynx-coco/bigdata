---
language:
- zh
- en
license: cc-by-4.0
size_categories:
- n<1K
task_categories:
- time-series-forecasting
tags:
- remote-sensing
- ecology
- meteorology
- california
- ndvi
- transformer
- 遥感
- 生态学
---

# 加州 NDVI 与多源气象融合数据集 (2016-2025)

## 1. 数据集简介
本数据集提供了一个统一的、以 16 天为间隔的加利福尼亚州植被归一化指数（NDVI）时间序列，并集成了多源气象驱动变量。该数据集专为评估深度学习模型（如 LSTM、Transformer）在气候波动下预测植被动态的能力而设计。

## 2. 数据来源
数据集通过 **Google Earth Engine (GEE)** 云计算平台对以下权威数据源进行聚合构建：
- **NDVI**: MODIS (MOD13A2.061) 16天合成产品，空间分辨率 1km。
- **气象数据 (gridMET)**: 来自 NOAA 的每日地表气象数据（包括降水、最高/最低温），已聚合至 16 天窗口。
- **土壤湿度**: NASA SMAP (SPL3SMP_E) L3 级土壤湿度（AM）产品。

## 3. 数据集结构
数据集以 `.csv` (或 `.parquet`) 格式提供，包含 **227 个时间步**。每一行代表一个 16 天的合成周期。

### 特征定义：
| 字段名称 | 描述 | 单位 |
| :--- | :--- | :--- |
| `date` | 16天合成周期的起始日期 | YYYY-MM-DD |
| `NDVI` | 区域面积加权平均 NDVI 值 | 无量纲 (-1 到 1) |
| `precipitation` | 16天窗口内的累计降水量 | mm |
| `soil_moisture` | 地表 (0-5 cm) 平均土壤含水量 | m³/m³ |
| `temp_max` | 每日最高气温的平均值 | °C (已从开尔文转换) |
| `temp_min` | 每日最低气温的平均值 | °C (已从开尔文转换) |
| `smap_count` | 窗口内有效的 SMAP 观测天数 | 次数 |

## 4. 预处理逻辑
1. **时间对齐**：所有每日气象数据均经过重采样和聚合，以匹配 MODIS NDVI 的 16 天固定时间戳。
2. **空间聚合**：所有数据均在加利福尼亚州行政边界内进行了空间平均处理。
3. **缺失值处理**：对于 SMAP 的数据空隙采用了线性插值；NDVI 噪声在 MODIS 合成过程中已通过质控位掩膜最小化。
4. **标准化建议**：在进行深度学习建模前，建议对所有特征进行归一化（Min-Max Scaling）处理。


## 5. 引用与致谢
如使用本数据集，请注明本项目及原始数据提供方：
- NASA LP DAAC (MODIS NDVI)
- University of Idaho/NOAA (gridMET)
- NASA NSIDC (SMAP)