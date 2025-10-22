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

### Elde Edilen Sonuçlar Özeti

Yapılan testler sonucunda:
* **Analitik Başarı:** Sistem, kaynak materyal üzerinden **yüksek seviyeli sentez** ve akademik çıkarımlar (Örn: Kahraman motivasyonları, yeminlerin rolü) yapabildiğini kanıtlamıştır.
* **RAG Kuralı Uygulaması:** Model, spesifik faktik soruların cevabını bulamadığında, *anti-hallucination* hedefine uygun olarak **kesinlikle reddetmiş** (Örn: Köpeğin adı), bu da RAG kuralına sıkı sıkıya uyduğunu göstermiştir.
* **Optimizasyon:** Embedding süreci, **T4 GPU** optimizasyonu ile hızlandırılmıştır.

## 4. Çalışma Kılavuzu

1.  **Gereksinimler:** `requirements.txt` dosyasını kullanarak bağımlılıkları kurun:
    ```bash
    pip install -r requirements.txt 
    ```
2.  **Ortam Ayarı:** Gemini API Key, Kaggle Username ve Kaggle Key'i ortam değişkeni olarak veya Colab Secrets aracı ile ayarlayın
3.  **GPU Ayarı:** Colab'de **Runtime $\to$ Change runtime type** menüsünden **T4 GPU**'yu seçin (Embedding aşamasının hızlı olması ve daha nokta atışı verilere ulaşması açısından kritiktir.)).
4.  **Çalıştırma:** Notebook'u sırayla çalıştırın. Kod çalışınca sırayla veri setini indirecek, FAISS indeksini oluşturacak ve Gradio arayüzünü başlatacak.

## 5. Web Arayüzü & Product Kılavuzu

[cite_start]Arayüz, Gradio'nun `ChatInterface` yapısıyla sunulmuştur[cite: 25].

* **Çalışma Akışı:** Arayüze girildiğinde, sistem kurulmuş RAG zincirini kullanarak analiz ve faktik sorulara yanıt verir. En iyi sonuçlar için sorularınızı İngilizce sormanız tavsiye edilir.
* **Test Etme:** Projenin yeteneklerini doğrulamak için **analitik ve tematik sorgular** kullanın (Örn: "Analyze the role of promises and oaths...").

### Web Linkiniz 

**Önemli:** Projenizin URL'si mutlaka burada paylaşılmalıdır.

> **[LÜTFEN GRADIO İLE OLUŞAN CANLI LİNKİNİZİ BURAYA YAPIŞTIRIN]**
