# Yapay Zeka ile Satış Ciro Tahmini

Bu proje, **100.000 satırlık** mağaza/e-ticaret satış verisini analiz ederek zaman serisi (time-series) modellemesi ile **gelecekteki ciro ve talep tahminini (Sales Forecasting)** gerçekleştirmek amacıyla oluşturulmuştur.

Projede zaman serisi dinamiklerini (haftalık/yıllık sezonsallık, tatil günleri ve trendler) otomatik olarak yakalayabilen Meta/Facebook tarafından geliştirilmiş **Prophet** kütüphanesi kullanılmıştır.

## 🛠️ Kullanılan Teknolojiler ve Kütüphaneler

* **Dil:** Python 3.x
* **Veri İşleme & Manipülasyon:** `pandas`, `numpy`
* **Zaman Serisi & Modelleme:** `prophet`
* **Model Değerlendirme (Metrics):** `scikit-learn` (`mean_absolute_error`)
* **Görselleştirme:** `matplotlib`

---

## 🔄 Veri Hazırlığı ve Ön İşleme (Data Preprocessing)

1. **Veri Temizliği & Tip Dönüşümü:** `DATE_` sütunu `datetime64` formatına dönüştürüldü.
2. **Toplulaştırma (Aggregation):** Ham sipariş bazlı 100.000 satırlık gürültülü (noisy) veri, günlük bazda gruplanarak (`groupby`) günlük toplam ciro (`TOTALPRICE`) zaman serisine dönüştürüldü.
3. **Prophet Veri Standartlaştırması:** Sütun isimleri Prophet mimarisine uygun şekilde `ds` (Tarih) ve `y` (Hedef Değişken - Ciro) olarak yeniden adlandırıldı.

---

## 🎯 Model Mimarısı ve Test Stratejisi

Zaman serilerinde veri sızıntısını (data leakage) önlemek amacıyla **kronolojik sıralama korunarak** veriset ayrımı yapılmıştır:

* **Train Set (%85):** Modelin geçmiş trendleri, haftalık ve yıllık sezonsallıkları öğrenmesi için kullanıldı.
* **Test Set (%15):** Modelin hiç görmediği güncel son dönem verileri üzerinde tahmin başarısını ölçmek için saklandı.
* **Dış Faktörler (Regressors):** Türkiye resmi tatil ve bayram günleri (`add_country_holidays(country_name='TR')`) modele eklenerek dönemsel ciro dalgalanmaları optimize edildi.

---
