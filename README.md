# Recipe Assistant

RAG (Retrieval-Augmented Generation) mimarisi kullanılarak geliştirilen bir yemek tarifi asistanıdır.

Kullanıcıdan gelen yemek tarifi sorusu, ChromaDB üzerinde semantic search ile aranır. En alakalı metin parçaları alınarak LLM'e context olarak verilir ve cevap oluşturulur.

## Teknolojiler

- Python
- Google Gemini API
- ChromaDB
- Sentence Transformers
- Embeddings
- RAG
- Google Colab

## RAG Akışı

PDF → Text Extraction → Chunking → Embedding → ChromaDB  
User Query → Retrieval → Relevant Chunks → Context → Gemini LLM → Answer

## Mevcut Durum

Projenin ilk RAG pipeline'ı oluşturuldu.  
Doküman işleme, embedding, ChromaDB retrieval ve LLM ile cevap üretme aşamaları çalışmaktadır.

## Karşılaşılan Problemler ve Çözüm Yaklaşımları

PDF veri kaynağındaki tariflerin yapısı gereği (BÖLÜM 1, BÖLÜM 2... ve altlarındaki numaralandırılmış tarifler), varsayılan/standart metin parçalama (chunking) yöntemleri kullanıldığında tariflerin anlamsal bütünlüğü bozulmakta ve tarifler birbirine karışabilmektedir.

Bu durumu önlemek için **Semantic Chunking** ve **Hiyerarşik Metin İşleme** yaklaşımları uygulanacaktır:

1. **Bölüm Bazlı Yapısal Parçalama (Section-Based Splitting):**
   * Metin parçalanırken sabit karakter/token sınırları yerine `BÖLÜM` anahtar kelimeleri ayraç (delimiter) olarak kullanılacaktır.
   * Maksimum token sınırı aşılmadığı sürece bir bölüm tam parçalanmadan tek bir chunk olarak ele alınacak, bir chunk asla yeni bir bölümün yarısında kesilmeyecektir.

2. **Alt Tarif Bütünlüğünün Korunması (Hierarchical Chunking):**
   * Bölümler altındaki numaralandırılmış tarifler parçalanırken tarif bütünlüğü göz önünde bulundurulacaktır.
   * İlk aşamada **Bölüm seviyesinde**, ikinci aşamada ise Bölüm altındaki **Tekil Tarif seviyesinde** hiyerarşik parçalama yapılarak context kalitesi artırılacaktır.

## Geliştirme

Proje ilerleyen aşamalarda chunking, retrieval ve context kalitesinin iyileştirilmesi amacıyla geliştirilecektir.
