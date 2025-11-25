# Hava Kalitesi Tahmin Modeli
----
Bu proje Makine Öğrenmesine Giriş dersi dolayısıyla geliştirilmiştir.

## Proje Amacı
----
Kirletici bileşenlere (CO, Ozone, NO₂, PM2.5) ait AQI değerlerini kullanarak, AQI kategorisini İyi (Good), Orta (Moderate), Kötü (Poor/Unhealthy) olarak 3 sınıfa indirgenmiş şekilde tahmin etmektir.

## Uygulanan Yöntemler
----
Bu projede veri hazırlama, özellik mühendisliği ve sınıflandırma modellerinin değerlendirilmesi için aşağıdaki adımlar uygulanmıştır:

### Veri Temizleme
----
Veri setindeki eksik değere sahip satırlar silindi.

### Feature Engineering
----
Ham AQI Category değerleri sadeleştirilerek Simple_AQI_Category adıyla yeni bir kategori oluşturuldu:
* Good → 0
* Moderate → 1
* Unhealthy, Very Unhealthy, Hazardous → Poor/Unhealthy → 2

Bu yeni kategori Simple_AQI_Num ile sayısallaştırıldı.
Yalnızca model için gerekli feature’lar (CO AQI Value, Ozone AQI Value, NO2 AQI Value, PM2.5 AQI Value) seçildi.

### Veri Görselleştirme
----
Kirletici seviyeleri ile hedef değişken arasındaki ilişki scatter plotlar ile analiz edildi.
Kirleticilerin artışı ile AQI kategorisinin kötüleşmesi arasındaki deterministik ilişki grafiklerle gözlemlendi.

### Eğitim Test Ayrımı ve Ölçeklendirme
----
Veri %80 eğitim, %20 test olacak şekilde ayrıldı.
Kirletici değerlerinin ölçek farkını azaltmak için StandardScaler uygulandı.

### Modelleme ve Performans Karşılaştırması
----
Aşağıdaki üç makine öğrenmesi modeli eğitildi ve karşılaştırıldı:
* Logistic Regression
* Decision Tree Classifier
* Random Forest Classifier

Elde edilen doğruluklar:

| Model               | Accuracy |
| ------------------- | -------- |
| Logistic Regression | %96.01   |
| Decision Tree       | %100.00  |
| Random Forest       | %100.00  |


