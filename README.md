# markaTahmin-ddi

### Projeyi Hazırlayanlar;
UTKU YURT  213405075                  
MUHAMMED ABDÜLBAKİ ÇEKEN  213405023

# ARABAM.COM SİTESİNDEN İLAN VERİLEN ARAÇLARIN MODEL TAHMİNİ
Bu projede arabam.com sitesinden alınmış farklı modeldeki araç(Renault,Kia,wolswogen,audi,bmw vb.) ilanları ile modelimizi eğitiyoruz ve modele bir ilan yazdığımızda o ilanın hangi model araç olduğunu tahmin ediyor.

#### Kullanılan Kütüphaneler
-Pandas      
-Numpy  
-Pyplot  
-Scikit-Learn  
-BeautifulSoup  

##### Veri Setinin Oluşturulması
Veri setimiz arabam.com da web scrapping işlemi yaparak farklı modeldeki araçlardan oluşan ilanlardan oluşuyor. Modelimizi doğru eğitebilmemiz için verilerimizin düzgün, eksiksiz veriler olması gerekir. Bu yüzden ilanlar bazı kriterlere dikkat edilerek çekilmiştir. Örneğin paylaşılan ilanın metninde araba modelinin isminin geçmesi. İlanları çekebilmek için web scrapping işlemi yapan BeatufilSoup kütüphanesi kullanılmıştır.
Veri setimizde yaklaşık 4000 veri vardır.

##### Veri Setinde Bulunan Araç ve Marka Sayıları
![Veri Seti](https://github.com/UtkuYURT/markaTahmin-ddi/blob/main/images/veri-seti.png)  
Bu verileri çekmek ortalama 45-50 dakika sürmüştür.

Veri setindeki kolonlarımız aşağıdaki gibidir;  
-Araba İlanları  
-Marka Etiketi  
Veri setinde ilanlar markaları ile ekleniyorlar. Bunun sebebi oluşturulacak olan modeli eğitmek için hangi kategoriye ait olduğunu bilmemiz içindir.

**Oluşturulmuş Veri Seti**  
![Oluşturulmuş Veri Seti](https://github.com/UtkuYURT/markaTahmin-ddi/blob/main/images/olusturulmus-veri-seti.png)

##### İlanların Temizlenmesi ve Lemmatization İşlemi
Veri seti oluşturulduktan sonra modelin daha iyi çalışması ve başarı oranının daha yüksek olması için ilanların temizlenmesi gerekmektedir. İlanların içerisinde emojiler, noktalama işaretleri, stop wordsler, linkler gibi istenmeyen ve modelin başarısını düşürecek veriler ilanların içerisinden temizleniyor.

**Oluşturulmuş Temiz İlan Gösteremi**  
![Oluşturulan Temiz İlan](https://github.com/UtkuYURT/markaTahmin-ddi/blob/main/images/olusturulan-temiz-ilan.png)

##### Modelin Oluşturulması ve İlanların Kategorilendirilmesi
###### Model Seçimi
Yapılacak sınıflandırma işleminin hangi modelde daha yüksek başarı oranı vereceğini tespit etmek amacıyla araştırma yapılıp aynı zamanda bazı modeller üzerinde de test edilmiştir.2 model üzerinde denemeler yapılmıştır. Bu modeller Naive bayes ve Decision Tree modelleridir. Projedeki test veri seti sonuçlarına bakıldığında;
  Naive Bayes Modeli Başarısı:0.651
  Decision Tree Modeli Başarısı:0.7889
Sonuçlar incelendiğinde Decision Tree modelinin projede kullanılan veri seti için daha doğru sonuç verdiği görülmüştür bu yüzden projeyi Decision Tree modeli üzerinde yapmaya karar verdik.

##### Modelin Oluşturulmaya Başlanması
###### Etiketleme
Temizlenmiş ilanları  
df["Etiket"] = df.apply(lambda row:  etiketle(row), axis=1)   
koduyla etiketleyip   
df.to_csv("arabaModel.csv", index=False) ile   
arabaModel.csv tablosuna aktarıyoruz.  
İlanların hangi markaya ait olduğu kelime şeklinde kayıtlı olduğu için bu markaları 0,1,2,3,… gibi bilgisayarın anlayacağı bir formata dönüştürüyoruz. Etiket adında yeni bir kolon açıp markaya göre (veri setimizde 91 marka var) bu yüzden 0‘dan 91'e kadar etiketleme yapıyoruz. Örneğin renault için 0 honda için 4 etiketlemesi yapıyoruz.

**Etiketlenmiş Veri Seti**  
![Etiketlenmiş Veri Seti](https://github.com/UtkuYURT/markaTahmin-ddi/blob/main/images/veri-seti-son.png)

##### Veri Setinin Parçalanması
Model başarısını doğru şekilde değerlendirebilmek için, eğitim ve test aşamalarında farklı veri kümeleri kullanmak önemlidir. Modelin eğitiminde kullanılan veriler, test aşamasında tekrar kullanılmamalıdır çünkü bu durum modelin başarısını yanıltıcı bir şekilde yüksek gösterebilir. Bu sebeple, veri setini bölmek gereklidir. Bu proje için, veri setinin %80'i modelin eğitimi için kullanılacak, geri kalan %20'si ise modelin test edilmesi için ayrılacaktır.	
Veri setini parçalamak için train_test_split() fonksiyonu kullanılmıştır.

##### İlanların Vektörel Matrisinin Çıkarılması
TF-IDF vektörleme yöntemi kullanılarak, her bir ilandaki kelimelerin belge içindeki önemi istatistiksel olarak hesaplanmıştır. Bu süreçte, yaygın stopwords'lar gibi anlam taşımayan kelimeler filtrelenerek, model için gereksiz olan unsurların etkisi azaltılmıştır. Bu sayede, anlamlı ve bilgi taşıyan özellikler belirginleştirilmiştir, modelin doğruluğunu artırmak adına daha sağlam bir temel oluşturulmuştur.

#### Modelin Eğitilmesi
Decision Tree metoduyla modelimiz eğitilmiştir. Modeli eğitmek için sklearn kütüphanesinden faydalanılmıştır.
##### Modelin Başarısının Hesaplanması
Hesaplama aşağıdaki görseldedir  
![Model Başarısı](https://github.com/UtkuYURT/markaTahmin-ddi/blob/main/images/model-basarisi.png)

### SONUÇ
Özetle yaptığımız projede seçilen modelin ne kadar önemli olduğu görülmektedir. Başlangıçta test edilmiş olan Naive Bayes modelinin bu veri seti için Decision Tree modeline göre uygunluğu daha azdır. Ancak model seçiminin önemi kadar veri setinin de güzel bir şekilde hazırlanmış olması, güzel temizlenmesi, veri setindeki veri miktarı, veri setinin dengesiz olup olmaması, eğitim-test setinin parçalanma oranı vb. faktörler modelin başarısına etki edildiği anlaşılmıştır.












