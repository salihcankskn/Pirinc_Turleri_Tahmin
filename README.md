Pirinç Türlerini Sınıflandırma - Derin Öğrenme Projesi
Bu çalışmada, farklı pirinç türlerini sınıflandırmak için bir derin öğrenme modeli geliştirilmiştir. Modelin geliştirilme süreci, veri ön işleme, model kurulumu, eğitim, iyileştirme ve sonuç aşamalarını kapsamaktadır. Aşağıda, her adımın kodları ve açıklamaları yer almaktadır.
1. Veri Setinin Colab Ortamına Çekilmesi
Projede kullanılan veri seti büyük olduğundan dolayı Colab ortamında işlemler yapılmıştır. Veri setini yükleme uzun zaman alacağından dolayı Kaggle platformunun sunduğu kolaylık sayesinde gerekli kodlarla Colab ortamına çekilmiştir.
 
 
2. Veri Setinin Yüklenmesi ve Keşfi
 Kullanılan veri seti çeşitli pirinç türlerinin resimlerinden oluşmaktadır. Veri seti Colab ortamına çekildikten sonra veriyi görebilmek ve dengesizliği ölçebilmek için gerekli işlemler yapılmıştır. Bu sayede verimizin nelerden oluştuğu, kaç adet olduğu vb. özellikleri görerek nasıl ilerleneceğine karar verilmiştir.
  
 
3. Model için Hazırlama ve Ön İşleme
Bu adımda, pirinç türü görsellerini modelin öğrenebileceği formata getirdik.
1.	Fotoğraf Bilgileri: Tüm pirinç görsellerinin nerede olduklarını ve hangi türe ait olduklarını bir listede topladık. Tür isimlerini sayılara çevirdik.
2.	Veriyi Bölme: Veri setini %80 eğitim ve %20 test olacak şekilde ikiye ayırdık. 
3.	Etiketleri Düzenleme: Tür sayılarını, modelin daha iyi anlayacağı bir formata çevirdik. (one-hot encoding)
4.	Veri Yükleme Sistemi: Büyük bir veri setimiz olduğu için, tüm görselleri aynı anda belleğe yüklemek yerine, eğitim sırasında küçük gruplar halinde yükleyecek özel bir yapı (RiceImageGenerator) oluşturduk. Bu yapı, görselleri şu şekilde hazırlıyor: 
o	Renk formatını RGB ye çeviriyor.
o	Resim boyutlarını 150x150 px yapıyor.
o	Piksel değerlerini 0 ile 1 arasına normalleştiriyor.
o	Her eğitim turunda veriyi karıştırarak modelin ezber yapmasını engelliyor.
  

4. Modelin Oluşturulması
Model Sequential API ile oluşturulmuş, Conv2D ve MaxPooling2D katmanları ile derinleştirilmiştir. Aktivasyon fonksiyonu olarak ReLU, çıkış katmanında softmax kullanılmıştır. Dropout katmanları ile overfitting önlenmeye çalışılmıştır. Model 'adam' optimizer ile derlenmiş, loss fonksiyonu olarak 'sparse_categorical_crossentropy' kullanılmıştır. 
 
 
5. Modelin Eğitilmesi
.Model denenen ve en iyi sonucu veren epoch sayısı boyunca eğitilmiş, eğitim ve doğrulama doğrulukları ile kayıpları görselleştirilmiştir. Modelin aşırı öğrenmesini engellemek için kerasta bulunan callbacksler kullanılmıştır.  
 

6. Modelin İyileştirilmesi ve Kaydedilmesi
Farklı parametreler denenmiştir:
- Katman sayıları değiştirilmiştir (örneğin 2 yerine 3 konvolüsyonel katman).
- Dropout oranı artırılmış/azaltılmıştır.
- Epoch sayısı artırılarak daha uzun süre eğitim denenmiştir.
- Optimizer olarak Adam dışında SGD test edilmiştir.
- Learning rate değerleri değiştirilerek performans karşılaştırılmıştır.
Yapılan bu denemeler sonucunda en iyi doğruluk sağlayan kombinasyon seçilmiştir. Tüm test verisi için tahmin yapılmış ve karmaşıklık matrisi görüntülenmiştir. Ardından modelin raporu görüntülenmiş ve modeli kaydetme işlemine geçilmiştir.
    


7. Sonuç ve Değerlendirme
Elde edilen model, test verisi üzerinde başarılı bir sınıflandırma performansı göstermiştir. Modelin doğruluk oranı kabul edilir seviyededir ve model aşırı öğrenme göstermemiştir. Dışarıdan yüklenen resimler ile test edilmiş ve resim gerekli koşulları sağladığınıda doğru tahminleri vermiştir.   

