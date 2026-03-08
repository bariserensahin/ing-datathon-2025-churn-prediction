# ING Hubs Datathon - Customer Churn Prediction
**Yazar:** Barış Eren Şahin

Merhaba, bu repoda ING Hubs Datathon için geliştirdiğim müşteri kaybı tahmin çözümünü bulabilirsiniz. Bu projede ana hedefim, yaklaşık 176 bin müşterinin demografik ve finansal verilerini analiz ederek, referans tarihinden sonraki altı ay içinde bankayı bırakma ihtimali yüksek olan müşteri profillerini belirlemekti.

### Değerlendirme metriği ve Yöntem

Bize verilen metrikte direkt Accuracy veya AUC ile değil; Gini katsayısı, Recall @ %10 ve Lift @ %10 metriklerinin ağırlıklı birleşimiyle ölçüm yapılıyor. 
**Bunun sebebi ise şu:**
- Accuracy (Doğruluk): Veri dengesiz olduğu için (müşterilerin çoğu kalıcıdır), model "kimse gitmeyecek" tahmini yapsa bile yüksek puan alır ama pratikte hiçbir işe yaramaz.

- Gini: Finans sektöründe riskli müşteri ile risksiz müşteriyi birbirinden ne kadar net ayırdığını gösteren en standart ölçüttür.

- Recall @ %10: Bankanın çağrı/kampanya bütçesi sadece en riskli %10'luk kitleyi hedeflemeye yetiyorsa, modelin bu dar kitle içine gerçekten kaçacak olanların yüzde kaçını sığdırabildiğini ölçer.

- Lift @ %10: Rastgele müşteri seçmek yerine bu modeli kullanmanın bankaya kaç kat daha fazla isabet (ve maliyet avantajı) sağladığını gösterir.

Kısacası: Accuracy ve AUC teorik başarıyı ölçerken; Gini, Recall ve Lift modelin bankaya sağladığı gerçek ticari faydayı, aksiyon alınabilirliğini ve bütçe verimliliğini ölçer.
Bu sayede modelin kararlarını doğrudan iş hedeflerine hizalamış oldum.

### Keşifsel Veri Analizi ve Özellik Mühendisliği

Veri hazırlığı süreci, projenin üzerinde en fazla zaman harcadığım ve düşündüğüm bölümü oldu. Müşterilerin yalnızca yaş veya cinsiyet gibi sabit özelliklerine odaklanmak, modelin etkili öğrenmesi için yeterli değildi. Bu yüzden istatistiksel testlerle (Chi-Square, Cramér's V) bu değişkenlerin ağırlığını ölçtükten sonra asıl odak noktamı davranışsal verilere kaydırdım.

Geçmiş müşteri verilerini kullanarak RFM (Recency, Frequency, Monetary) mantığıyla yeni özellikler ürettim. 3, 6 ve 12 aylık zaman pencerelerinde müşterilerin işlem hacimlerini, ürün kullanım trendlerini ve bu kullanımlardaki düşüş ivmelerini modelledim. Tüm bu işlemleri yaparken, geleceğe dair verilerin eğitime sızmasını (data leakage) engellemek için referans tarihlerini çok sıkı bir şekilde kontrol ettim.

### Modelleme ve Açıklanabilirlik

Veri setindeki yaklaşık %14'lük churn oranının yarattığı dengesizliği yönetebilmek için 5-Fold Stratified Cross-Validation stratejisini kullandım. Kategorik değişkenleri doğal bir şekilde işleyebilmesi ve aşırı öğrenmeye karşı dirençli yapısı nedeniyle CatBoost algoritmasını tercih ettim.

Sadece tahmin yapmakla kalmayıp, modelin mantığını da anlamlandırabilmek için SHAP değerlerini kullandım. Bu sayede modeli bir kara kutu olmaktan çıkarıp, hangi değişkenin müşteri kaybında ne kadar tetikleyici olduğunu iş birimlerine açıklanabilir bir formatta görselleştirdim.

### Gelecek Adımlar (To-Do)

Proje şu anki haliyle tutarlı sonuçlar üretse de, performansı daha da artırmak için aklımda olan ve yakın zamanda uygulamayı planladığım adımlar şunlar:

Optuna veya Hyperopt kullanarak Bayesian Optimization ile daha geniş bir arama uzayında hiperparametre optimizasyonu yapmak.

Sadece CatBoost ile sınırlı kalmayıp; LightGBM, XGBoost gibi farklı modelleri de eğiterek tahminleri ensemble etmek.

Veri kalitesini artırmak için extreme outlier değerleri 99. yüzdelik dilimde sınırlandırmak (capping) ve eksik sektör bilgilerini iş tiplerine göre daha akıllı bir şekilde doldurmak (imputation).

SHAP bazlı özellik seçimi ile daha agresif bir eleme yaparak, modelin genelleme yeteneğini artırmak.

Vakit ayırıp incelediğiniz için teşekkürler. Herhangi bir sorunuz veya geri bildiriminiz olursa iletişime geçebilirsiniz.
