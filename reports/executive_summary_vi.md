
# 🌫️ BÁO CÁO DỰ ÁN: DỰ BÁO CHẤT LƯỢNG KHÔNG KHÍ PM2.5 TẠI BẮC KINH

**Ngày báo cáo**: 07/01/2026

---

## 📋 TỔNG QUAN DỰ ÁN

### Mục tiêu
Xây dựng hệ thống dự báo nồng độ PM2.5 theo giờ tại Bắc Kinh, Trung Quốc để:
- Cảnh báo sớm ô nhiễm không khí cho cộng đồng
- Hỗ trợ ra quyết định quản lý môi trường
- So sánh hiệu quả giữa các phương pháp Machine Learning và Time Series

### Dữ liệu
- **Nguồn**: Beijing Air Quality Dataset 2013-2017
- **Trạm**: Aotizhongxin (1 trong 12 trạm)
- **Khoảng thời gian**: 2013-03-01 đến 2017-02-28 (4 năm)
- **Tần suất**: Hourly (theo giờ)
- **Tổng samples**: ~31,900 sau feature engineering
- **Train/Test split**: 2013-2016 (train) / 2017 (test)

### Biến số
- **Target**: PM2.5 (μg/m³)
- **Features**: PM10, SO2, NO2, CO, O3, nhiệt độ, áp suất, độ ẩm, mưa, hướng gió, lag features, rolling statistics

---

## 🎯 KẾT QUẢ CHÍNH

### Q1: So sánh Baseline Models
| Model | RMSE | MAE | R² |
|-------|------|-----|-----|
| **Random Forest** | 25.33 | 12.32 | 0.949 |
| ARIMA(1,0,1) | 104.10 | 77.69 | - |

✅ **Kết luận Q1**: Random Forest vượt trội ARIMA **75%** về RMSE nhờ tận dụng nhiều features.

### Q2: Model Improvement
| Model       |    RMSE |      MAE |       R² |   Improvement (%) |
|:------------|--------:|---------:|---------:|------------------:|
| Baseline RF | 25.3267 | 12.3232  | 0.949151 |            0      |
| XGBoost     | 17.6039 |  8.68602 | 0.976668 |           30.4927 |
| LightGBM    | 16.1613 |  8.31411 | 0.980335 |           36.1889 |

🏆 **Best Model**: **LightGBM** với RMSE=16.16

✅ **Kết luận Q2**: LightGBM cải thiện **36.2%** so với baseline, đạt R²=0.980

---

## 📊 INSIGHTS QUAN TRỌNG

1. **Gradient Boosting > Random Forest > ARIMA** cho bài toán dự báo PM2.5 với nhiều biến
2. **Lag features** (đặc biệt lag 1h, 24h) là yếu tố quan trọng nhất
3. **LightGBM** cân bằng tốt giữa độ chính xác và tốc độ
4. **ARIMA** không hiệu quả khi có nhiều external factors (thời tiết, pollutants khác)
5. **Thời tiết** (nhiệt độ, gió, mưa) có tương quan mạnh với PM2.5

---

## 💡 KHUYẾN NGHỊ

### Cho hệ thống production:
✅ Deploy **LightGBM** làm model chính  
✅ Retrain model định kỳ (hàng tháng)  
✅ Monitor drift: track RMSE/MAE trên production data  
✅ Setup alert system khi dự báo PM2.5 > 150 μg/m³ (unhealthy)

### Cải thiện tiếp theo:
- Ensemble nhiều models (stacking)
- Thêm data từ 11 trạm còn lại
- Feature engineering nâng cao (interaction terms)
- Multi-step forecasting (dự báo 24h, 48h tiếp theo)

---
