# Makine-Ogrenmesi-Anomali-Tespiti-Vize-Kodlar-
Bu depo, 2026-2027 Güz Dönemi'nde yürütülen Makine Öğrenmesi ve Anomali Tespiti dersi kapsamında geliştirilen kodların paylaşılması amacıyla oluşturulmuştur.
# Gene Expression Analysis and Feature Selection

Bu depo, ALL_AML ve Armstrong (Arm2) gen ekspresyon verileri üzerinde yürütülen
veri analizi, filtre tabanlı özellik seçimi ve Information Gain + SVM-RFE
hibrit özellik seçimi çalışmalarını içerir.

## Depo içeriği

```text
    Makine-Ogrenmesi-Anomali-Tespiti-Vize/
├── README.md
├── requirements.txt
├── environment.yml
├── reproducibility.py
├── notebooks/
│   ├── dataAnalysisCodes.ipynb
│   └── featureSelectionMethod.ipynb
├── ArmstrongV2-dataset/
│   └── Arm2.csv                
└── Data_Analysis_Results/
│                 
└── Feature_Selection_Csv_Files/
              
```

## Çalışmanın kapsamı

### 1. `dataAnalysisCodes.ipynb`
- ALL_AML eğitim ve bağımsız test verilerinin yüklenmesi
- Armstrong/Arm2 verisinin yüklenmesi
- Veri kalitesi, eksik değer, sonsuz değer ve aykırı değer incelemesi
- Tanımlayıcı istatistikler ve sınıf dağılımları
- Ölçekleyici karşılaştırması (StandardScaler / RobustScaler) ve PCA
- Gen ekspresyonu ve örnek düzeyinde görselleştirmeler
- Uygun hücrelerde istatistiksel karşılaştırmalar ve çoklu test düzeltmesi

### 2. `featureSelectionMethod.ipynb`
- ALL_AML üzerinde ANOVA, Chi-Square, Information Gain ve Mutual Information
- Stratified 5-fold çapraz doğrulama ve Linear SVM
- Arm2 veri setinde aynı filtre tabanlı yöntemlerin değerlendirilmesi
- En iyi özellik sayısının (k) ve seçilen genlerin kaydedilmesi
- Information Gain + SVM-RFE + LinearSVC hibrit yaklaşımı
- GridSearchCV ile özellik sayısı optimizasyonu


## Kurulum

### Seçenek A — pip

Python 3.10–3.12 önerilir.

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS / Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
pip install -r requirements.txt
```

### Seçenek B — conda

```bash
conda env create -f environment.yml
conda activate gene-expression-analysis
```

## Çalıştırma

Jupyter Notebook'u başlatın:

```bash
jupyter lab
```

Ardından `notebooks/` içindeki notebook'ları sırasıyla açıp hücreleri çalıştırın.
Önce veri yollarını kendi ortamınıza göre düzenleyin. Notebook'lar Kaggle
ortamında çalıştırılacaksa mevcut `/kaggle/input/...` yolları ilgili veri
setlerinin Kaggle'a eklenmiş olmasını gerektirir.

Analiz çıktıları notebook'larda tanımlanan konumlara ve çalışma dizinine CSV/PNG
olarak yazılır. Yerel kullanımda `OUTPUT_DIR` değişkenini `results/` klasörüne
ayarlamanız önerilir.

## Tekrarlanabilirlik ve sabit random seed

Projede temel seed değeri **42** olarak belirlenmiştir. `reproducibility.py`
Python, NumPy ve (kuruluysa) TensorFlow/PyTorch için seed ayarlarını yapar:

```python
from reproducibility import set_global_seed

RANDOM_STATE = 42
set_global_seed(RANDOM_STATE)
```

Notebook'lardaki `RANDOM_STATE = 42` tanımları korunmalıdır. Çapraz doğrulama
için `StratifiedKFold(shuffle=True, random_state=RANDOM_STATE)`; SVM gibi
rastgelelik parametresi olan modeller için de aynı `random_state` kullanılmalıdır.
PCA gibi deterministik adımlar için seed gerekmez; t-SNE, train/test bölme veya
başka stokastik adımlarda `random_state=RANDOM_STATE` açıkça verilmelidir.

Not: Sabit seed tek başına her donanım, BLAS kitaplığı, işletim sistemi ve
kütüphane sürümünde bit düzeyinde aynı sonucu garanti etmez. Paket sürümlerini
sabitlemek ve aynı veri/kod ortamını kullanmak sonuçların tekrarlanabilirliğini
artırır.


## Beklenen temel çıktılar

Notebook'ların ilgili hücreleri çalıştırıldığında aşağıdaki türde çıktılar oluşur:

- Veri kalite raporları ve aykırı değer sayımları (`*_quality.csv`,
  `*_outlier_counts.csv`, `*_sample_outliers.csv`)
- Tanımlayıcı istatistikler ve görseller (CSV/PNG)
- Filtre tabanlı özellik seçimi sonuçları
  (`feature_selection_cv_results_train.csv`, `best_k_results_train.csv`,
  `selected_genes_train.csv`)
- Hibrit yöntem sonuçları
  (`IG_SVMRFE_gridsearch_all_results.csv`,
  `IG_SVMRFE_gridsearch_best_results.csv`,
  `IG_SVMRFE_ALL_AML_selected_genes.csv`,
  `IG_SVMRFE_Arm2_selected_genes.csv`)

## Atıf / kullanım

Bu depo, yüklenen Jupyter Notebook kodlarının düzenlenmiş ve çalıştırılabilir
bir proje yapısında sunulması için hazırlanmıştır. Veri setleri ve orijinal
çalışmalar için ilgili veri sağlayıcılarının atıf ve lisans koşullarına uyun.
