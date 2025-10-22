# RAG-CHATBOT-GENAI-BOOTCAMP

# Akbank GenAI Bootcamp Projesi: RAG Tabanlı Akademik Masal Asistanı

## 1. Projenin Amacı

Bu proje, RAG mimarisini kullanarak geniş bir masal koleksiyonu üzerinde bilgi sorgulama ve tematik analiz yapabilen, özellikle edebiyat öğrencileri ve akademisyenlerine yönelik akademik araştırmalarda asaitanlık yapabilecek bir chatbot geliştirmeyi amaçlamaktadır. Sistem, LLM'in (Gemini 2.0 Flash) kendi genel bilgisini kullanmasını engelleyerek, yanıtların **öncelikli olarak yüklenen masal metinlerine** dayanmasını sağlamaktadır. Şu anda kullanılan veri seti sadece masal türünde metinlerden oluşup ileride geniş bir edebi kaynak setiyle daha kapsamlı aaştırmalarda kullanılması sağlanabilir.

## 2. Veri Seti Hakkında Bilgi
**Veri Seti:** Fairy Tales Collection
**Kaynak:** Kaggle (cuddlefish/fairy-tales)
**İçerik:** 19-20. yy'a ait çeşitli İngilizce masal koleksiyonlarından (Grimm, Anderson, Binbir Gece Masalları, vb.) derlenmiş metinler
**Hazırlık Metodolojisi:** Projenin başlangıcında, Colab'in kısıtlı kaynaklarında hızlı ve stabil çalışması için, tüm veri setinden **1000 adet metin parçası** (chunk) üretilerek sisteme dahil edilmiştir.

## 3. Kullanılan Yöntemler ve Çözüm Mimarisi

Bu RAG zinciri, Retrieval ve Generation aşamalarında aşağıdaki teknolojileri kullanmıştır:

| Bileşen | Teknoloji | Amaç |
| :--- | :--- | :--- |
| **Büyük Dil Modeli (LLM)** | Gemini 2.0 Flash  | Yüksek kaliteli, hızlı yanıt üretimi (Generation). |
| **Embedding Modeli** | Hugging Face `all-mpnet-base-v2` | Metinleri vektörlere dönüştürerek anlamsal arama yapılmasını sağlar. |
| **Vektör Veritabanı** | FAISS | Vektörler üzerinde hızlı ve verimli arama (Retrieval). |
| **RAG Çatısı** | LangChain | Tüm pipeline bileşenlerini entegre eder ve yönetir. |
| **Arayüz** | Gradio | Kullanıcı etkileşimi için Python temelli web arayüzü. |

Proje temelde akademik araştırmalarda kaynak taramada kolaylık açısından AI kullanımında oluşabilecek "AI hallucination" durumunun sebep olabileceği eksik ya da var olmayan metin kaynaklarının verilmesi probleminin önüne geçer. Ayrıca LLM modelinin bir metnin birçok versiyonunu birleştirerek üreteceği karmaşşık çıktıların yerine metnin tek bir versiyonuna sadık kalarak bu karmaşayı önlemesi amaçlanmıştır.

**RAG MİMARİSİ**
Proje, LangChain çatısı altında, tipik bir RAG akışını takip eder:
1.	Veri Yükleme: Masal metinleri yüklenir.
2.	Parçalama (Chunking): Metin, $500\text{ karakterlik}$ küçük parçalara ayrılır.
3.	Vektörleştirme (Embedding): Her parça, Hugging Face all-mpnet-base-v2 modeli kullanılarak sayısal bir vektöre dönüştürülür.
4.	Depolama: Vektörler, hızlı arama için FAISS indeksinde saklanır.
5.	Sorgu (Retrieval): Kullanıcı bir soru sorduğunda, bu soru vektörleştirilir ve FAISS, en alakalı $k=8$ metin parçasını bulur.
6.	Üretim (Generation): Bu 8 metin parçası context olarak kullanılır ve orijinal soru, akademik bir dille cevap verilmesi öngörülerek hazırlanmış prompt şablonuyla birlikte Gemini 2.0 Flash modeline gönderilir.
7.	Yanıt: Gemini, sadece sağlanan contexti kullanarak nesnel bir cevap sentezler.



### Elde Edilen Sonuçlar Özeti
### Bulgulara Dayalı Akademik Analiz

Yapılan 10 soruluk test serisi, sistemin iki kritik yeteneğini kanıtlamıştır:

* **Analitik Sentez Üstünlüğü:** Sistem, yüksek seviyeli akademik sorgulara ("Motivasyonlar," "Dönüşüm Motifleri") yanıt verirken, getirilen parçalardan nesnel, akademik çıkarımlar sentezleyebildiğini kanıtlamıştır. Bu, RAG'in sadece bilgi bulma değil, aynı zamanda **bağlama dayalı sentez** yapma yeteneğini gösterir.
* **Sıkı RAG Kuralı Uygulaması:** Model, spesifik bilgi arayan soruların cevabını limitli veri setinde bulamadığında (Örn: Köpeğin adı), katı kurala uyarak uydurma yapmaktan kaçınmış ve anti-hallucination hedefine ulaştığını göstermiştir. Bilgi sorularının bir kısmının reddedilmesi, sistemin kasıtlı olarak sadece $1000$ parçalık kaynağa bağlı kaldığının kanıtıdır.

### Detaylı Test Çıktıları ve Performans Tablosu

| No. | Soru Kategorisi | Soru (Özet) | Model Cevabı (Özet) | Sonuç |
| :---: | :--- | :--- | :--- | :--- |
| **1** | Bilgi | Fox's food request? | I am sorry... | RED (Sınır Dışı) |
| **2** | Bilgi | Miller's daughter's clothes? | I am sorry... | RED (Sınır Dışı) |
| **3** | Bilgi | 'Old Sultan', dog's name? | I am sorry... | RED (Sınır Dışı) |
| **4** | Bilgi | Hansel/Gretel house material? | I am sorry... | RED (Sınır Dışı) |
| **5** | Analiz | Promises and oaths analysis? | I am sorry... | RED (Sınır Dışı) |
| **6** | Analiz | Transformation environments? | Magical transformations occur in diverse environments, including castles, domestic settings, and natural landscapes. | BAŞARILI SENTEZ |
| **7** | Analiz | Primary motivations driving protagonists? | Motivations include a desire for discovery, familial duty, and moral obligation. | BAŞARILI KATEGORİZASYON |
| **8** | Analiz | Forms of wealth or reward? | Rewards include gold-tipped horns, golden apples, silver/copper apples, and the choice of three rewards. | BAŞARILI LİSTELEME |
| **9** | Analiz | Common narrative outcomes for kindness? | Outcomes are varied and context-dependent, including ruination, gratitude, and exploitation. | BAŞARILI ANALİZ |
| **10** | Analiz | Recurring motifs of transformation or disguise? | Motifs include physical alteration for deception (disguise as an old woman) and changes in demeanor. | BAŞARILI MOTİF ÇIKARIMI |

* **Optimizasyon:** Embedding süreci, **T4 GPU** optimizasyonu ile hızlandırılmıştır.

## 4. Çalışma Kılavuzu

1.  **Gereksinimler:** `requirements.txt` dosyasını kullanarak gerekli tüm kütüphaneleri kurun:
    ```bash
    pip install -r requirements.txt 
    ```
3.  **Ortam Ayarı:** Gemini API Key, Kaggle Username ve Kaggle Key'i ortam değişkeni olarak veya Colab Secrets aracı ile ayarlayın
4.  **GPU Ayarı:** Kodu .ali;tirmadan , önce Colab'de **Runtime $\to$ Change runtime type** menüsünden **T4 GPU**'yu seçin (Embedding aşamasının hızlı olması ve daha nokta atışı verilere ulaşması açısından kritiktir.)
5.  **Çalıştırma:** Notebook 6 hğcreden oluşur, hücreleri sırayla çalıştırın. Kod çalışınca sırayla veri setini indirecek, FAISS indeksini oluşturacak ve Gradio arayüzünü başlatacak.

## 5. Web Arayüzü & Product Kılavuzu

Arayüz, Gradio'nun `ChatInterface` yapısıyla oluşturulmuştur.

* **Çalışma Akışı:** Arayüze girildiğinde, sistem kurulmuş RAG zincirini kullanarak kullanıcı tarafından üretilen konu ile alakalı sorulara yanıt verir. En iyi sonuçlar için sorularınızı İngilizce sormanız tavsiye edilir çünkü kullanılan veri İngilizce kaynaklardan derlenmiştir. Kullanıcı sorusunu sorgu çubuğuna yazıp gönder tuşuna bastığında sistem cevaı üretmeye başlar. Eğer eriştiği verilerle sorulan soruyu cevaplayabiliyorsa kısa bir cevap verir, veremiyorsa "I am sorry, I could not find this information in the fairy tales available to me." mesajını sunar.
* **Test Etme:** Projenin yeteneklerini doğrulamak için **analitik ve tematik sorgular** kullanın (Örn: "Analyze the role of promises and oaths...").

### Gradio Arayüzü İçin App'in Web Linki

(https://1d91b6ad6838d17fa1.gradio.live/)

