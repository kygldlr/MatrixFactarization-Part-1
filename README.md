Film Tavsiye Sistemi Part-1: Matris Faktörizasyon

Bu projede, Matris Faktörizasyon (MF) modeli kullanarak kullanıcılar için film derecelerini tahmin eden bir film tavsiye sistemi oluşturacağız. Projede, kullanıcıların filmlere verdikleri puanları tahmin etmek için MovieLens veri setini ve PyTorch kütüphanesini kullanacağız.

Özellikler

Matris Faktörizasyon kullanılarak iş birliğine dayalı filtreleme

PyTorch kullanımı

Kategorik özellikler için Label Encoding ile veri ön işleme

Model değerlendirmesi için eğitim-doğrulama ayrımı

Dataset ve DataLoader oluşturma

Model oluşturma

Modelin eğitilmesi

Ortalama Kare Hata (MSE) kaybı ile model değerlendirmesi

Eğitim ve doğrulama kayıplarının görselleştirilmesi

Sonuçların Kontrol Edilmesi


Gereksinimler

Python 3.x

PyTorch

NumPy

Pandas

Matplotlib

Google Colab (isteğe bağlı)



Kurulum

Gerekli kütüphaneleri yüklüyoruz:

pip install torch pandas numpy matplotlib



Veri Seti

Bu projede MovieLens veri setini kullanacağız. Veri seti, çeşitli filmler için userId,movieId,title,genres ve rating bilgilerini içermektedir. 

Kullanılacak dosyalar:

movies.csv: Film detayları (ör. film Id'leri, isimleri ve türleri)

ratings.csv: Kullanıcıların filmlere verdikleri puanlar




Adımlar


1. Veri Yükleme ve Ön İşleme

movies.csv ve ratings.csv dosyalarını yüklüyoruz.

Ardından movieId sütununa göre iki dosyayı birleştiriyoruz.

Daha sonra verileri userId ve timestamp'e göre sıralıyoruz.

Label Encoding kullanarak userId ve movieId sütunlarını sayısal indekslere dönüştürüyoruz.Yani userId ve movieId'nin benzersiz değerlerine ulaşırız.Bu bellek kullanımını azaltır ve sinir ağlarının gömme katmanları için giriş boyutunu küçültür.



2.Model değerlendirmesi için veri setinin ayrılması 

Veri setinde 610 benzersiz kullanıcı olduğu ve her kullanıcının en az 20 filmi puanladıklarını biliyoruz.
Kullanıcı kimliğine göre gruplar oluşturuyoruz.
Kullanıcıların son 5 puanlaması hariç tamamını eğitim setine dahil ediyoruz.
Genellikle eğitim ve test verisini %80'e %20 bölmek daha iyi olabilir. 


3. Dataset ve DataLoader

Dataset

Kullanıcı-film-puan verilerini hazırlamak için özelleştirilmiş bir PyTorch Dataset sınıfı tanımlıyoruz.
Kullanıcıların (userId), filmlerin (movieId) ve bu filmlere verdikleri puanların olduğu bir veri kümesini alıyoruz.
Bu verileri model eğitiminde kullanılabilir hale getiriyoruz.

DataLoader

DataLoader, genellikle PyTorch kütüphanesinde kullanılan bir veri yükleme aracıdır.
Veri kümesini işlerken modeli eğitmek ve test etmek için verileri küçük parçalara (mini-batch) böler, karıştırılmasını (shuffle) ve işlemeye uygun bir şekilde yüklenmesini sağlar.


4. Model Tanımı

PyTorch'un nn.Module sınıfını kullanarak Matris Faktörizasyon (MF) modelini tanımlıyoruz.
Bu sınıf modelin öğrenilebilir parametrelerini ve ileri geçiş (forward) işlemini tanımlar.

init() fonksiyonu, modelin katmanlarını ve parametrelerini tanımlıyor.
Kullanıcılar ve filmler için için gömme (embedding) tablosu oluşturacak ve her kullanıcı ve film embedding_dim boyutunda bir vektörle temsil edilecek.

forward fonksiyonu,kullanıcı ve film girişlerinden bir tahmin puanı üretir.(örneğin:Bir kullanıcının bir filme olan ilgisi)
Ardından modelin parametrelerini vererek modeli oluşturuyoruz.


4. Eğitim

Matrix Factorization modelini PyTorch kullanarak bir veri kümesi üzerinde eğitmek ve doğrulamak için bir eğitim döngüsü tanımlar.

Kodun Genel Amacı

Eğitim Döngüsü (Training Loop):
modeli eğitim verileri üzerinde eğitir.
Her epoch sonunda doğrulama verileri üzerinde modelin performansını değerlendirir.

Kayıp Fonksiyonu ve Optimizasyon:
Gerçek puanlar ile tahmin edilen puanlar arasındaki farkı ölçmek için MSE hata fonksiyonu kullanılır.  


5. Değerlendirme

Modelimizi eğittikten sonra doğrulama verisi üzerinde değerlendiriyoruz.
Her epoch sonunda eğitim ve doğrulama kayıplarını kaydediyoruz.


6. Görselleştirme

Görselleştirme için matplotlib kullanıyoruz.
Eğitimde ki epochlara göre eğitim ve doğrulama kayıplarını çizdiriyoruz.Böylelikle kaç epoch sonunda modelin doyuma ulaştığı ve kaybın minimuma yaklaştığını görürüz.



7.Sonuçların kontrolü

Kullanıcı ve filmlerin gömme ağırlıklarının min/max değerleri
Tahminlerin min/max değerleri
Gerçek puanların min/max değerleri

Sonuçlarımızı kontrol ettiğimizde modelin tahmin aralığı [-3.85 , 8.15 ] arasındadır. Ama gerçek değerlendirme aralığı [0.5 , 5 ] arasında olduğunu görüyoruz bu da modelin tam olarak doğru sonucu vermediğini gösteriyor.
Bunu iyileştirmek için Part-2'de bir çok iyileştirme yaparak modelin gerçek değer aralığında tahminde bulunmasını ve hatanın minimuma indirilmesini sağlayacağız. 


Dilara KAYAOĞLU