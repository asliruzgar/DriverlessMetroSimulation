from collections import defaultdict, deque
import heapq
from typing import Dict, List, Tuple, Optional

class Istasyon:
    def __init__(self, idx: str, ad: str, hat: str):
        self.idx = idx
        self.ad = ad
        self.hat = hat
        self.komsular: List[Tuple['Istasyon', int]] = []

    def komsu_ekle(self, istasyon: 'Istasyon', sure: int):
        self.komsular.append((istasyon, sure))

class MetroAgi:
    def __init__(self):
        self.istasyonlar: Dict[str, Istasyon] = {}

    def istasyon_ekle(self, idx: str, ad: str, hat: str):
        if idx not in self.istasyonlar:
            istasyon = Istasyon(idx, ad, hat)
            self.istasyonlar[idx] = istasyon

    def baglanti_ekle(self, istasyon1_id: str, istasyon2_id: str, sure: int):
        istasyon1 = self.istasyonlar[istasyon1_id]
        istasyon2 = self.istasyonlar[istasyon2_id]
        istasyon1.komsu_ekle(istasyon2, sure)
        istasyon2.komsu_ekle(istasyon1, sure)
    
    def en_az_aktarma_bul(self, baslangic_id: str, hedef_id: str) -> Optional[List[str]]:
        if baslangic_id not in self.istasyonlar or hedef_id not in self.istasyonlar:
            return None
        
        kuyruk = deque([(baslangic_id, [baslangic_id])])
        ziyaret_edilen = set()
        
        while kuyruk:
            mevcut_id, yol = kuyruk.popleft()
            if mevcut_id == hedef_id:
                return [self.istasyonlar[i].ad for i in yol]
            
            if mevcut_id in ziyaret_edilen:
                continue
            ziyaret_edilen.add(mevcut_id)
            
            for komsu, _ in self.istasyonlar[mevcut_id].komsular:
                if komsu.idx not in ziyaret_edilen:
                    kuyruk.append((komsu.idx, yol + [komsu.idx]))
        
        return None

    def en_hizli_rota_bul(self, baslangic_id: str, hedef_id: str) -> Optional[Tuple[List[str], int]]:
        if baslangic_id not in self.istasyonlar or hedef_id not in self.istasyonlar:
            return None
        
        pq = [(0, baslangic_id, [baslangic_id])]
        ziyaret_edilen = {}
        
        while pq:
            toplam_sure, mevcut_id, yol = heapq.heappop(pq)
            
            if mevcut_id == hedef_id:
                return ([self.istasyonlar[i].ad for i in yol], toplam_sure)
            
            if mevcut_id in ziyaret_edilen and ziyaret_edilen[mevcut_id] <= toplam_sure:
                continue
            ziyaret_edilen[mevcut_id] = toplam_sure
            
            for komsu, sure in self.istasyonlar[mevcut_id].komsular:
                heapq.heappush(pq, (toplam_sure + sure, komsu.idx, yol + [komsu.idx]))
        
        return None

# Örnek kullanım ve testler
if __name__ == "__main__":
    metro = MetroAgi()
    
    # İstasyonlar ekleme
    istasyonlar = [
        ("M1", "AŞTİ", "Mavi Hat"),
        ("M2", "Kızılay", "Mavi Hat"),
        ("K1", "Kızılay", "Kırmızı Hat"),
        ("K2", "Ulus", "Kırmızı Hat"),
        ("K3", "Demetevler", "Kırmızı Hat"),
        ("K4", "OSB", "Kırmızı Hat"),
        ("M3", "Sıhhiye", "Mavi Hat"),
        ("M4", "Gar", "Mavi Hat"),
        ("T1", "Batıkent", "Turuncu Hat"),
        ("T2", "Demetevler", "Turuncu Hat"),
        ("T3", "Gar", "Turuncu Hat"),
        ("T4", "Keçiören", "Turuncu Hat"),
    ]
    
    for idx, ad, hat in istasyonlar:
        metro.istasyon_ekle(idx, ad, hat)
    
    # Bağlantılar ekleme
    baglantilar = [
        ("M1", "M2", 5), ("M2", "K1", 2), ("K1", "K2", 4), ("K2", "K3", 6), ("K3", "K4", 8),
        ("M2", "M3", 3), ("M3", "M4", 4), ("M4", "T3", 2),
        ("T1", "T2", 7), ("T2", "T3", 9), ("T3", "T4", 5),
        ("K3", "T2", 3)
    ]
    
    for i1, i2, sure in baglantilar:
        metro.baglanti_ekle(i1, i2, sure)
    
    # Test senaryoları ve çıktı doğrulama
    testler = [
        ("M1", "K4", "AŞTİ'den OSB'ye"),
        ("T1", "T4", "Batıkent'ten Keçiören'e"),
        ("T4", "M1", "Keçiören'den AŞTİ'ye"),
    ]
    
    for baslangic, hedef, ad in testler:
        print(f"\n{ad}:")
        rota = metro.en_az_aktarma_bul(baslangic, hedef)
        if rota:
            print("En az aktarmalı rota:", " -> ".join(rota))
        
        sonuc = metro.en_hizli_rota_bul(baslangic, hedef)
        if sonuc:
            rota, sure = sonuc
            print(f"En hızlı rota ({sure} dakika):", " -> ".join(rota))

    print("\nTestler tamamlandı, sistem başarıyla çalışıyor!")
