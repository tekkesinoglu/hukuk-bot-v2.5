# ⚖️ Hukuk-Bot v2.5 (Turkish Legal AI Assistant)

**Hukuk-Bot**, Türk Hukuk Mevzuatına (TCK, Borçlar Kanunu, Medeni Kanun vb.) hakim, soruları kaynak göstererek cevaplayan ve sohbet edebilen gelişmiş bir Yapay Zeka asistanıdır.

Bu proje, **Llama-3-8B** dil modeli ve **RAG (Retrieval-Augmented Generation)** mimarisi kullanılarak geliştirilmiş; Google Colab'ın donanım limitlerinde (T4 GPU) çalışacak şekilde **RAM ve VRAM optimizasyonu** yapılmıştır.

## 🚀 Özellikler (v2.5)

* **🧠 Akıllı Hafıza (Smart Memory):** Bot, sohbet geçmişini hatırlar ancak kanun maddelerini hafızaya kalıcı olarak kazımaz. Bu sayede "Papağan Sendromu" (tekrara düşme) engellenmiştir.
* **🧩 Semantik Bölümleme (Regex Chunking):** PDF kitapları rastgele değil, `Madde 1`, `Madde 2` şeklinde mantıksal bütünlük korunarak parçalanmıştır.
* **🛡️ Güvenli Mod (Safe Mode):** GPU çökmesini (CUDA Error) önlemek için Vektör Arama (Embedding) işlemleri CPU'ya, Dil Modeli (LLM) işlemleri GPU'ya dağıtılmıştır (Hibrit Mimari).
* **🚦 Akıllı Yönlendirici (Router):** Kullanıcı "Naber?" dediğinde sohbet eder, "Cezası ne?" dediğinde veritabanına bağlanır.
* **🔍 X-Ray Modu:** Botun verdiği cevabın hangi kanun maddesinden alındığı arayüzde şeffaf bir şekilde gösterilir.

## 🛠️ Kullanılan Teknolojiler

* **LLM:** `unsloth/llama-3-8b-Instruct-bnb-4bit` (Türkçe talimat performansı yüksek)
* **RAG & Vektör DB:** `LangChain`, `ChromaDB`
* **Embedding:** `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
* **Arayüz:** `Streamlit`
* **Deployment:** `Ngrok` / `Cloudflare Tunnel`

## ⚙️ Kurulum ve Çalıştırma

Bu proje Google Colab üzerinde çalışmak üzere tasarlanmıştır.

1.  Repodaki `.ipynb` dosyasını Google Colab'da açın.
2.  Menüden **Runtime > Change runtime type > T4 GPU** seçeneğini aktif edin.
3.  `NGROK_TOKEN` alanına kendi token'ınızı yapıştırın.
4.  Tüm hücreleri sırasıyla çalıştırın.
5.  Çıktıdaki `ngrok` veya `trycloudflare` linkine tıklayarak arayüze erişin.

## 📂 Proje Mimarisi

```mermaid
graph TD;
    A[Kullanıcı Sorusu] --> B{Router / Niyet Analizi};
    B -- Sohbet --> C[Llama-3 Sohbet Modu];
    B -- Hukuk --> D[Vektör Veritabanı Tarama];
    D --> E[En Alakalı Kanun Maddeleri];
    E --> F[Llama-3 Hukuk Modu + Context];
    F --> G[Referanslı Cevap];
    C --> G;
