# Hava Kalitesi Tahmin Modeli

Bu proje Makine Öğrenmesine Giriş dersi dolayısıyla geliştirilmiştir.

## Proje Amacı

Kirletici bileşenlere (CO, Ozone, NO₂, PM2.5) ait AQI değerlerini kullanarak, AQI kategorisini İyi (Good), Orta (Moderate), Kötü (Poor/Unhealthy) olarak 3 sınıfa indirgenmiş şekilde tahmin etmektir.

## Uygulanan Yöntemler

Bu projenin geliştirilmesi için veri hazırlama, özellik mühendisliği ve sınıflandırma modellerinin değerlendirilmesi için aşağıdaki adımlar uygulanmıştır:

### Veri Setini İnceleyelim

Veri setinin ilk 5 satırı bu şekildedir:

| Country            | City              | AQI Value | AQI Category | CO AQI Value | CO AQI Category | Ozone AQI Value | Ozone AQI Category | NO2 AQI Value | NO2 AQI Category | PM2.5 AQI Value | PM2.5 AQI Category |
|--------------------|-------------------|-----------|--------------|--------------|-----------------|------------------|---------------------|----------------|-------------------|------------------|----------------------|
| Russian Federation | Praskoveya        | 51        | Moderate     | 1            | Good            | 36               | Good                | 0              | Good              | 51               | Moderate             |
| Brazil             | Presidente Dutra  | 41        | Good         | 1            | Good            | 5                | Good                | 1              | Good              | 41               | Good                 |
| Italy              | Priolo Gargallo   | 66        | Moderate     | 1            | Good            | 39               | Good                | 2              | Good              | 66               | Moderate             |
| Poland             | Przasnysz         | 34        | Good         | 1            | Good            | 34               | Good                | 0              | Good              | 20               | Good                 |
| France             | Punaauia          | 22        | Good         | 0            | Good            | 22               | Good                | 0              | Good              | 6                | Good                 |

Veri seti boyutu: (23463, 12)

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 23463 entries, 0 to 23462
Data columns (total 12 columns):

| #  | Column              | Non-Null Count | Dtype  |
|----|----------------------|----------------|--------|
| 0  | Country             | 23036          | object |
| 1  | City                | 23462          | object |
| 2  | AQI Value           | 23463          | int64  |
| 3  | AQI Category        | 23463          | object |
| 4  | CO AQI Value        | 23463          | int64  |
| 5  | CO AQI Category     | 23463          | object |
| 6  | Ozone AQI Value     | 23463          | int64  |
| 7  | Ozone AQI Category  | 23463          | object |
| 8  | NO2 AQI Value       | 23463          | int64  |
| 9  | NO2 AQI Category    | 23463          | object |
| 10 | PM2.5 AQI Value     | 23463          | int64  |
| 11 | PM2.5 AQI Category  | 23463          | object |


dtypes: int64(5), object(7)
memory usage: 2.1+ MB

| Column             | Missing Values |
|--------------------|----------------|
| Country            | 427            |
| City               | 1              |
| AQI Value          | 0              |
| AQI Category       | 0              |
| CO AQI Value       | 0              |
| CO AQI Category    | 0              |
| Ozone AQI Value    | 0              |
| Ozone AQI Category | 0              |
| NO2 AQI Value      | 0              |
| NO2 AQI Category   | 0              |
| PM2.5 AQI Value    | 0              |
| PM2.5 AQI Category | 0              |

Veri setini incelediğimizde Country City gibi gereksiz sutünlar bulunmaktadır. Modelin daha iyi sonuç vermesi için kirletici bileşenlere ait Category sutünları yerine, daha hassas bilgi taşıyan Value sutünlarını girdi olarak kullanacağımız için Category sutünlarıda gereksiz olarak belirlenmiştir. Country ve City sutünlarında eksik veriler vardır ve bunlar genel verilerin çok düşük bir kısmı olduğu için dropna ile silinecektir.

### Veri Temizleme

Veri setindeki eksik değere sahip satırlar silindi.

### Feature Engineering

Ham AQI Category değerleri sadeleştirilerek Simple_AQI_Category adıyla yeni bir kategori oluşturuldu:
* Good → 0
* Moderate → 1
* Unhealthy, Very Unhealthy, Hazardous → Poor/Unhealthy → 2

Bu yeni kategori Simple_AQI_Num ile sayısallaştırıldı.
Yalnızca model için gerekli feature’lar (CO AQI Value, Ozone AQI Value, NO2 AQI Value, PM2.5 AQI Value) seçildi.

### Veri Görselleştirme

Kirletici seviyeleri ile hedef değişken arasındaki ilişki scatter plotlar ile analiz edildi.
Kirleticilerin artışı ile AQI kategorisinin kötüleşmesi arasındaki deterministik ilişki grafiklerle gözlemlendi.

### Eğitim Test Ayrımı ve Ölçeklendirme

Veri %80 eğitim, %20 test olacak şekilde ayrıldı.
Kirletici değerlerinin ölçek farkını azaltmak için StandardScaler uygulandı.

### Modelleme ve Performans Karşılaştırması

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

## Değerlendirme ve Sonuç

Modellerin doğruluk oranları incelendiğinde, Decision Tree ve Random Forest'ın %100 başarı göstermesi, kirletici değerleri ile AQI kategorisi arasındaki güçlü ve doğrudan ilişkiden kaynaklanmaktadır. Çünkü CO, Ozone, NO₂ ve PM2.5 seviyeleri zaten AQI hesaplamasında kullanılan temel unsurlardır.
Bu nedenle modelin hedef değişkeni tahmin etmesi görece kolay olmuş ve yüksek doğruluk oranları elde edilmiştir. Sonuç olarak, bu proje hava kalitesinin sınıflandırılması için makine öğrenmesinin etkili şekilde kullanılabileceğini göstermektedir.
