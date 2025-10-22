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
| **Büyük Dil Modeli (LLM)** | [cite_start]Gemini 2.0 Flash [cite: 42] | Yüksek kaliteli, hızlı yanıt üretimi (Generation). |
| **Embedding Modeli** | [cite_start]Hugging Face `all-mpnet-base-v2` [cite: 43] | Metinleri vektörlere dönüştürerek anlamsal arama yapılmasını sağlar. |
| **Vektör Veritabanı** | [cite_start]FAISS [cite: 43] | Vektörler üzerinde hızlı ve verimli arama (Retrieval). |
| **RAG Çatısı** | [cite_start]LangChain [cite: 44] | Tüm pipeline bileşenlerini entegre eder ve yönetir. |
| **Arayüz** | Gradio | Kullanıcı etkileşimi için Python temelli web arayüzü. |

### [cite_start]Elde Edilen Sonuçlar Özeti [cite: 12]

Yapılan testler sonucunda:
* **Analitik Başarı:** Sistem, kaynak materyal üzerinden **yüksek seviyeli sentez** ve akademik çıkarımlar (Örn: Kahraman motivasyonları, yeminlerin rolü) yapabildiğini kanıtlamıştır.
* **RAG Kuralı Uygulaması:** Model, spesifik faktik soruların cevabını bulamadığında, *anti-hallucination* hedefine uygun olarak **kesinlikle reddetmiş** (Örn: Köpeğin adı), bu da RAG kuralına sıkı sıkıya uyduğunu göstermiştir.
* [cite_start]**Optimizasyon:** Embedding süreci, **T4 GPU** optimizasyonu ile hızlandırılmıştır[cite: 43].

## [cite_start]4. Kodun Çalışma Kılavuzu [cite: 19]

[cite_start]Bu aşama, kodunuzun çalıştırılabilmesi için gerekenleri anlatan bir kılavuzdur[cite: 20].

1.  **Gereksinimler:** `requirements.txt` dosyasını kullanarak bağımlılıkları kurun:
    ```bash
    [cite_start]pip install -r requirements.txt [cite: 21]
    ```
2.  **Ortam Ayarı:** Gemini API Key, Kaggle Username ve Kaggle Key'i ortam değişkeni olarak veya Colab Secrets aracı ile ayarlayın[cite: 21].
3.  **GPU Ayarı:** Colab'de **Runtime $\to$ Change runtime type** menüsünden **T4 GPU**'yu seçin (Embedding süreci için kritiktir).
4.  **Çalıştırma:** Notebook'u veya Python dosyanızı sırayla çalıştırın. Kod, veri setini indirecek, FAISS indeksini oluşturacak ve Gradio arayüzünü başlatacaktır[cite: 21].

## [cite_start]5. Web Arayüzü & Product Kılavuzu [cite: 24]

[cite_start]Arayüz, Gradio'nun `ChatInterface` yapısıyla sunulmuştur[cite: 25].

* **Çalışma Akışı:** Arayüze girildiğinde, sistem kurulmuş RAG zincirini kullanarak analiz ve faktik sorulara yanıt verir. En iyi sonuçlar için sorularınızı İngilizce sormanız tavsiye edilir.
* **Test Etme:** Projenin yeteneklerini doğrulamak için **analitik ve tematik sorgular** kullanın (Örn: "Analyze the role of promises and oaths...").

### [cite_start]Web Linkiniz 

[cite_start]**Önemli:** Projenizin URL'si mutlaka burada paylaşılmalıdır.

> **[LÜTFEN GRADIO İLE OLUŞAN CANLI LİNKİNİZİ BURAYA YAPIŞTIRIN]**
