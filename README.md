
# SME Dynamic CASA Optimization (Data-Driven KOBİ Bankacılığı)

> **Yönetici Özeti (Executive Summary):** 
> Anlık POS verisi ışığında kurgulanan, bankanın fonlama maliyetini minimize ederek Net Faiz Marjını (NIM) artıran dinamik CASA ve sadakat modeli.

## 🎯 İş Problemi ve Değer Önerisi
Geleneksel KOBİ bankacılığında risk ve sadakat yönetimi, genellikle gecikmeli bilgi veren yıl sonu finansal tablolarına dayanmaktadır. Bu proje, statik veriler yerine işletmelerin **anlık nakit akışını ve pazar davranışlarını yansıtan POS verilerini** kullanarak banka kârlılığını artırmayı hedefleyen veri odaklı (data-driven) bir karar destek sistemidir.

Projenin temel amacı; homojen ve maliyetli pazarlama kampanyaları yerine, her KOBİ segmentinin banka bilançosuna olan etkisini (P&L) optimize edecek dinamik Çapraz Satış ve CASA (Vadesiz Mevduat) stratejileri geliştirmektir.

## 📊 Metodoloji: Pareto ve RFM Matrisi
*   **Veri Seti:** +400.000 satırlık anonimize edilmiş KOBİ POS işlem verisi.
*   **Kümeleme:** Müşteriler; Yenilik (Recency), Sıklık (Frequency) ve Parasal Değer (Monetary) metrikleri üzerinden Python ile 10 farklı davranışsal segmente ayrılmıştır.
*   **Konsantrasyon Analizi:** Geleneksel 80/20 Pareto kuralının aksine, portföyde "Müşterilerin %40'ının, Cironun %60'ını" getirdiği tespit edilmiş ve bankanın "Konsantrasyon Riski"nin düşük olduğu kanıtlanmıştır.
*   İlgili analiz sonucunda elde edilen tablolar aşağıdadır..
*   ### Proje Çıktısı: RFM Segmentasyon Tablosu
Aşağıdaki tablo, KOBİ portföyünün RFM skorlarına göre nasıl dağıldığını göstermektedir:

| Segment | Kişi Sayısı | Ortalama Yenilik (Recency) | Ortalama Sıklık (Frequency) | Ortalama Ciro (Monetary) |
| :--- | :--- | :--- | :--- | :--- |
| Şampiyonlar | 361 | 1.00 | 134.70 | 976.160 TL |
| Sadık Müşteriler | 714 | 1.00 | 132.78 | 984.336 TL |
| Potansiyel Sadıklar | 720 | 1.00 | 70.52 | 516.160 TL |
| Umut Verenler | 169 | 1.00 | 38.48 | 251.758 TL |
| Yeni Müşteriler | 184 | 1.00 | 37.78 | 235.245 TL |
| Dikkat Gerekenler | 171 | 1.00 | 81.58 | 623.842 TL |
| Uyku Adayları | 381 | 1.00 | 48.09 | 351.888 TL |
| Uyuyanlar | 702 | 19.08 | 46.36 | 386.730 TL |
| Riskli | 721 | 3.07 | 98.08 | 866.116 TL |
| Kaybedilemez | 377 | 2.45 | 164.11 | 1.533.403 TL |

### Pareto Analizi (80/20 Konsantrasyon Testi)
Portföyün gelir konsantrasyonu incelendiğinde, geleneksel 80/20 kuralının aksine daha tabana yayılmış ve konsantrasyon riski düşük bir yapı tespit edilmiştir. Ana taşıyıcı kolonlar şöyledir:

| Odak Segmentler | Müşteri İçindeki Payı (%) | Toplam Ciro İçindeki Payı (%) |
| :--- | :--- | :--- |
| **Şampiyonlar** | **% 8.0** | **% 10.8** |
| Sadık Müşteriler | % 16.5 | % 21.8 |
| Riskli KOBİ'ler | % 16.6 | % 19.3 |
| Kaybedilemez KOBİ'ler | % 8.7 | % 17.9 |
| **Toplam (Kritik Kitle)** | **% 49.8** | **% 69.8** |

> *Çıkarım: Portföydeki müşterilerin ~%50'si, bankanın KOBİ POS cirosunun ~%70'ini oluşturmaktadır. Geriye kalan %50'lik kitle (uyuyanlar, yeniler vb.) ise cironun sadece %30'unu getirmektedir. Bu durum banka için hem pozitif bir risk dağılımına işaret eder hem de kârlılık ve sadakat stratejilerinde odaklanılması gereken ana kitleyi netleştirir.*

## 💼 Segment Bazlı P&L Stratejileri

### 1. Kâr Maksimizasyonu ve Sıfır Maliyetli Fon (Şampiyonlar & Sadıklar)
Portföy cirosunun %32'sini oluşturan bu sadık kitlede temel hedef **Net Faiz Marjını (NIM) artırmaktır.**
*   **Aksiyon:** Rakip saldırılarına karşı POS komisyonunda **%35 indirim**.
*   **CASA Şartı:** Aylık cironun **minimum %10'unun** vadesiz hesapta (CASA) tutulması.
*   **Risk Kontrolü:** POS alacaklarına **7 gün valör (bloke)** uygulanması.
*   **Bilanço Etkisi:** Banka, kredi satışı için ihtiyaç duyduğu kaynağı piyasadan yüksek maliyetle toplamak yerine, bu kurgu sayesinde müşterinin valör ve CASA havuzundan **sıfır maliyetle** karşılamaktadır.

### 2. Zararına Liderlik (Loss Leader) Operasyonu (Riskli Segment)
Cironun %37'sini oluşturan ancak terk etme eğiliminde olan (Churn) yüksek hacimli kitlede hedef, **Müşteri Kazanım Maliyetini (CAC) optimize etmektir.**
*   **Aksiyon:** Çarkı tekrar bankamıza döndürmek için ilk 3 ay komisyonda **%50 Şok İndirim**.
*   **Esnek Şart:** Sadece %5 CASA bakiyesi ve 3 gün valör.
*   **Bilanço Etkisi:** İlk aylarda komisyondan edilen zarar, piyasadan yeni bir KOBİ bulmak için harcanacak CAC (Müşteri Kazanım Maliyeti) bütçesinden çok daha düşüktür. Amaç, KOBİ'nin nakit akışını tekrar bankaya bağlayıp uzun vadeli Müşteri Yaşam Boyu Değerini (CLV) güvence altına almaktır.

## 🧮 Finansal Simülasyon (Örnek Vaka)
Aylık 1.000.000 TL ciro yapan bir Şampiyon KOBİ senaryosunda stratejimizin finansal çıktısı:
*   **Banka Feragati:** %35 indirim sonucu komisyondan aylık **~10.000 TL** feragat edilir.
*   **Yaratılan Kaynak:** 7 gün valörden doğan günlük ~233.000 TL transit bakiye + %10 CASA kuralından gelen 100.000 TL = Banka kasasına giren toplam **333.000 TL sıfır maliyetli taze fon**.
*   **NIM Etkisi:** Banka bu parayı piyasadan %4 maliyetle toplasaydı ödeyeceği **13.333 TL** faiz masrafından kurtulur. 
*   **Sonuç:** Banka indirim yapmış gibi görünürken, tek bir KOBİ'den aylık net **+3.333 TL ekstra kâr** elde eder ve "Çıkış Bariyeri" örer.

## 🛠️ Teknik Altyapı
*   **Dil:** Python
*   **Kütüphaneler:** Pandas (Veri manipülasyonu), NumPy, Matplotlib & Seaborn (Görselleştirme)
*   ---
### 📂 Proje Kodları ve Veri Seti Hakkında Bilgilendirme
Bu analizde kullanılan veri temizleme, RFM skorlaması ve segmentasyon işlemlerinin tüm teknik altyapısına ait **Python (Jupyter Notebook)** kodlarına doğrudan repoda bulunan dosya üzerinden erişebilirsiniz. 

*(Not: Veri seti bankacılık gizlilik ve KVKK kuralları gereği anonimize edilmiştir.)*
