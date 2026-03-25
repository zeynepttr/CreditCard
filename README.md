# Kredi Kartı Sahtekarlık Tespiti (Credit Card Fraud Detection)


## Proje Özeti
Bu proje, kredi kartı işlemlerindeki sahtekarlıkları makine öğrenmesi algoritmaları ile tespit etmeyi amaçlamaktadır. Binlerce işlem arasından sahte olanları yakalayarak finansal kayıpları engellemek, günümüz makine öğrenmesi problemlerinin en kritik örneklerinden biridir. Bu portfolyo projesi kapsamında, dengesiz sınıf problemiyle başa çıkılarak çeşitli sınıflandırma modelleri kurulmuş ve karşılaştırılmıştır.

## Veri Seti Hakkında
- **Problem Türü:** İkili Sınıflandırma - *Normal İşlem (0) veya Sahtekarlık (1)*
- **Satır Sayısı:** 284,807 farklı cüzdan/kart işlemi
- **Sütun Sayısı:** 31 özellik (`Time`, `Amount`, V1-V28 PCA bileşenleri ve `Class`)
- **Dengesizlik Dağılımı:** Sahtekarlık vakaları verilerin sadece **%0.17**'sini oluşturmaktadır. Verinin bu derece dengesiz oluşu, model değerlendirmesinde **Accuracy (Doğruluk)** yerine **Precision (Hassasiyet), Recall (Duyarlılık)** ve **F1-Score** metriklerinin kullanılmasını zorunlu kılmıştır.

## Kullanılan Teknolojiler
- **Programlama Dili:** Python
- **Veri Analizi ve İşleme:** Pandas, NumPy
- **Makine Öğrenmesi Modelleri:** Scikit-learn (Logistic Regression, Random Forest, kNN, Naive Bayes)
- **Görselleştirme:** Matplotlib, Seaborn

## Veri Ön İşleme Aşamaları
- Veri setinde hiçbir eksik (null) değer bulunmamaktaydı, bu yüzden doldurma (imputation) işlemine gerek duyulmadı.
- `Amount` (tutar) özelliği çok fazla sapan değere (outlier) sahip olabileceği için modelleri yanıltmaması adına `RobustScaler` kullanılarak ölçeklendirildi. `Time` değişkeni de benzer şekilde ölçeklendirildi (V1-V28 bileşenleri halihazırda PCA ile standardize edilmişti).
- Veriler modele sokulurken **Train (%80)** ve **Test (%20)** kümelerine ayrıldı. Verideki sahtekarlık etiketleri çok az olduğu için `stratify=y` özelliği kullanılarak Train ve Test kümelerinde eşit oranda sahtekarlık etiketinin (%0.17) korunması sağlandı.

## Makine Öğrenmesi Modelleri ve İstatistiksel Bulgular
Veri hazırlanıp ayrıldıktan sonra, 4 farklı makine öğrenmesi modeli eğitilmiş ve test kümesi üzerinde başarı oranları hesaplanmıştır. **Karşılaştırmalı sonuç tablosu aşağıdaki gibidir:**

| Model | Accuracy (Doğruluk) | Precision (Hassasiyet) | Recall (Duyarlılık) | F1-Score | F2-Score |
| :--- | :c: | :c: | :c: | :c: | :c: |
| **Logistic Regression** | %99.91 | %81.82 | %64.29 | %72.00 | %67.16 |
| **Random Forest** | **%99.96** | **%94.05** | %80.61 | **%86.81** | **%82.98** |
| **k-Nearest Neighbors (kNN)**| %99.95 | %91.67 | %78.57 | %84.62 | %80.88 |
| **Naive Bayes** | %97.64 | %5.88 | **%84.69** | %10.99 | %23.00 |

### Modellerin Değerlendirilmesi ve En İyi Model
Finansal güvenlik uygulamalarında sahtekarlığı tespit edebilmek kadar, yanlış tespitte bulunup müşterilerin meşru işlemlerini reddetmemek de (*False Positive - Yanlış Alarm*) önemlidir. 

- **Random Forest**, hem F1-Skoru (**%86.81**) hem de Precision (**%94.05**) değerleriyle **en başarılı model** olmuştur. Yüksek Precision, modelin sahte olduğunu iddia ettiği işlemlerin %94'ünün gerçekten sahte olduğu anlamına gelir ki bu, müşteri memnuniyeti için bankacılık sektöründe istenen temel özelliktir.
- **k-Nearest Neighbors (kNN)** modeli de Random Forest'a çok yakın bir performans sergileyerek (%84.62 F1-Score) ikinci tercih edilebilir model olmuştur.
- **Naive Bayes**, en yüksek Duyarlılığa (Recall - %84.69) ulaşıp olabildiğince çok *sahtekarı yakalamaya* eğilimli davransa da, Precision değerini %5.88'e kadar düşürerek binlerce normal işlemi sahte olarak işaretlemiş ve model bütünlüğünü kaybetmiştir.

## Proje Dizin Yapısı (Nasıl Kullanılır?)
Projeyi lokalinizde test etmek için hücreleri baştan sona çalıştırabilirsiniz.
```text
archive (1)/
├── creditcard.csv                 # İndirilen ham Kaggle veri seti
├── notebooks/
│   ├── 01_eda.ipynb               # Aşama 1: Keşifsel Veri Analizi (EDA) ve Keşif
│   ├── 02_preprocessing.ipynb     # Aşama 2: Ölçeklendirme (Scaling) ve veri ayrımı
│   └── 03_model_comparison.ipynb  # Aşama 3: ML Modellerinin kurulup başarı metriklerinin çıkarılması
├── outputs/
│   ├── figures/                   # Karşılaştırma grafikleri ve Confusion Matrix çıktıları (PNG)
│   └── tables/                    # İşlenmiş veriler ve ML Model sonuç tablosu (CSV)
└── README.md                      # Proje genel raporu (Bu dosya)
```
  
