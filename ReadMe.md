# 🚇 Proje Başlığı: Metro Simülasyonu (Rota Optimizasyonu)

## 📌 1. Kısa Açıklama
Bu proje, bir metro ağı içerisinde **en az aktarmalı** ve **en hızlı rotayı** bulan bir simülasyon geliştirmeyi amaçlamaktadır. **BFS (Breadth-First Search)** ve **A* (A-Star)** algoritmaları kullanılarak rota optimizasyonu sağlanmıştır. Python ile geliştirilen proje, metro hatları arasında en uygun rotaları bulmak için graf veri yapısını kullanmaktadır.

---

## 📌 2. Kullanılan Teknolojiler ve Kütüphaneler
Proje, aşağıdaki temel kütüphaneleri kullanmaktadır:

- **Python 3.9+** → Projenin geliştirildiği programlama dili
- **collections.deque** → BFS algoritmasında kuyruk yapısı için kullanılmıştır
- **heapq** → A* algoritmasında öncelik kuyruğu oluşturmak için kullanılmıştır
- **defaultdict** → Metro ağı verisini daha düzenli yönetmek için kullanılmıştır

---

## 📌 3. Algoritmaların Çalışma Mantığı

### 🔹 BFS (Breadth-First Search) – En Az Aktarmalı Rota
- BFS, **en kısa yol algoritması** olarak kullanılır ve **genişlik öncelikli arama** yapar.
- Başlangıç noktasından itibaren **katman katman** ilerleyerek en kısa istasyon zincirini bulur.
- Kuyruk (queue) veri yapısını kullanarak, **önce eklenen istasyonları** işlemeye devam eder.
- **En kısa aktarma sayısına sahip olan yolu** bulduğunda işlemi durdurur.

### 🔹 A* (A-Star) Algoritması – En Hızlı Rota
- A* algoritması, **en düşük toplam süreyi** sağlayan yolu bulmak için kullanılır.
- **Öncelik kuyruğu (heapq)** ile en düşük maliyetli adımı önce işler.
- Her istasyonun **bir önceki istasyona olan süresini** hesaplayarak en hızlı rotayı oluşturur.
- **Hedefe en hızlı ulaşan yol tamamlandığında** algoritma durur.

**Neden BFS ve A* Kullanıldı?**
- **BFS** → **En az aktarmalı rotayı** garantili bulur.
- **A*** → **Toplam süreyi minimize eden en hızlı rotayı** bulur.
- **Birlikte kullanıldığında**, hem **yolculuk süresini**, hem de **aktarma sayısını** optimize eder.

---

## 📌 4. Örnek Kullanım ve Test Sonuçları

Aşağıda verilen test senaryoları, metro simülasyonunun doğruluğunu göstermek için çalıştırılmıştır.

```python
metro = MetroAgi()
metro.en_az_aktarma_bul("M1", "K4")  # En az aktarmalı rota
metro.en_hizli_rota_bul("M1", "K4")  # En hızlı rota
```

### 📝 Test Senaryoları
| Başlangıç | Hedef | En Az Aktarma | En Hızlı Rota (Dakika) |
|-----------|-------|--------------|------------------|
| AŞTİ | OSB | AŞTİ → Kızılay → Ulus → Demetevler → OSB | 25 |
| Batıkent | Keçiören | Batıkent → Demetevler → Gar → Keçiören | 21 |
| Keçiören | AŞTİ | Keçiören → Gar → Sıhhiye → Kızılay → AŞTİ | 19 |

---

## 📌 5. Projeyi Geliştirme Fikirleri
- **📍 Gerçek zamanlı veri entegrasyonu:** Trafik yoğunluğunu da hesaplayarak en hızlı rotayı daha dinamik hale getirme
- **🛤️ Görselleştirme ekleme:** Matplotlib veya NetworkX ile metro ağı haritası oluşturma
- **📊 Veri analizi:** En sık kullanılan hatları analiz etme ve yolcu akışını optimize etme
- **🗺️ Daha büyük metro ağı entegrasyonu:** Gerçek dünya metro verilerini içeren daha geniş bir simülasyon geliştirme

---

✅ **Proje tamamlandı ve başarılı şekilde test edildi!** Eğer yeni özellikler eklemek istersen, yukarıdaki geliştirme fikirlerini deneyebilirsin. 🚀😊

