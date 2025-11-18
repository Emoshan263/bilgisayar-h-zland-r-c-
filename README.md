# bilgisayar-hizland-rici
[AÇIK KAYNAKLI KODLARDIR.]

import os
import shutil
import subprocess
import sys
import time

def temizle_dizin(dizin_yolu):
    """Belirtilen dizinin içeriğini temizler ve raporlar."""
    print(f"-> {dizin_yolu} içeriği temizleniyor...")
    if not os.path.exists(dizin_yolu):
        print(f"Hata: Dizin bulunamadı: {dizin_yolu}")
        return

    temizlenen_sayi = 0
    hatalar = 0
    
    for öğe in os.listdir(dizin_yolu):
        öğe_yolu = os.path.join(dizin_yolu, öğe)
        try:
            if os.path.isfile(öğe_yolu):
                os.remove(öğe_yolu)
            elif os.path.isdir(öğe_yolu):
                shutil.rmtree(öğe_yolu)
            temizlenen_sayi += 1
        except Exception:
            hatalar += 1
            pass

    print(f"   [BAŞARILI]: {dizin_yolu} içinden {temizlenen_sayi} öğe silindi. ({hatalar} öğe atlandı)")

def nihai_performans_ekle():
    """Nihai Performans (Ultimate Performance) güç planını ekler ve aktif eder."""
    nihai_performans_guid = "e9a42b02-d5df-448d-aa00-03f14749eb61"
    
    print("\n-> Nihai Performans Güç Planı Kontrol Ediliyor ve Ekleniyor...")
    
    try:
        # Mevcut güç planlarını kontrol et
        sonuc_L = subprocess.run(["powercfg", "/L"], capture_output=True, text=True, check=True)
        guid_kontrol = nihai_performans_guid.replace('-', '').lower()
        
        if guid_kontrol in sonuc_L.stdout.replace('-', '').lower():
            print("   [BİLGİ]: Nihai Performans Güç Planı zaten mevcut.")
            # Aktif etme
            aktif_et_komutu = ["powercfg", "/S", nihai_performans_guid]
            subprocess.run(aktif_et_komutu, capture_output=True, text=True)
            print("   [BAŞARILI]: Nihai Performans Güç Planı aktif edildi.")
            return

        # Ekleme
        komut_ekle = ["powercfg", "-duplicatescheme", nihai_performans_guid]
        sonuc_ekle = subprocess.run(komut_ekle, capture_output=True, text=True)
        
        if sonuc_ekle.returncode == 0:
            print("   [BAŞARILI]: Nihai Performans Güç Planı başarıyla eklendi.")
            
            # Aktif Etme
            aktif_et_komutu = ["powercfg", "/S", nihai_performans_guid]
            subprocess.run(aktif_et_komutu, capture_output=True, text=True)
            print("   [BAŞARILI]: Nihai Performans Güç Planı aktif edildi.")
        else:
            print(f"   [HATA]: Güç Planı eklenirken bir sorun oluştu. Yönetici yetkisi kontrol edin.")

    except Exception as e:
        print(f"   [KRİTİK HATA]: powercfg komutu çalıştırılamadı. (Yönetici yetkisi gerekli): {e}")

# --- Programın Ana İşlevi ---
def ana_program():
    
    # Konsol Başlığını Ayarla (CMD için)
    if os.name == 'nt': # Yalnızca Windows için
        os.system('title PC Hızlandırıcı - BY: ANTOSEMO')

    print("------------------------------------------------")
    print("        PC Hızlandırıcı - BY: ANTOSEMO          ")
    print("------------------------------------------------")

    # Temizleme Onayı
    onay = input("Temizleme ve Optimizasyon işlemlerini yapmak istiyor musunuz? (E/H): ").strip().upper()
    
    if onay == 'E':
        print("\nİşlemler başlatılıyor...\n")
        
        # Dizinleri ayarla
        temp_kullanici = os.getenv('TEMP')
        temp_sistem = os.path.join(os.environ['SystemRoot'], 'Temp')
        prefetch_dizin = os.path.join(os.environ['SystemRoot'], 'Prefetch')
        
        # 1. Temizlik İşlemleri
        temizle_dizin(temp_kullanici)
        temizle_dizin(temp_sistem)
        temizle_dizin(prefetch_dizin)
        
        # 2. Güç Planı İşlemi
        nihai_performans_ekle()
        
        print("\n--- Tüm İşlemler Tamamlandı. Performansınız Optimize Edildi! ---")
    else:
        print("\nİşlemler iptal edildi. Program sonlandırılıyor.")

    # 3. Sonlandırma
    print("Çıkmak için herhangi bir tuşa basın...")
    
    # Eğer .exe olarak çalışıyorsa konsolun açık kalmasını sağlar
    if getattr(sys, 'frozen', False):
        input() 

if __name__ == "__main__":
    ana_program()


    ### ⚠️ NASIL ÇALIŞTIRILIR? (YÖNETİCİ GEREKLİ)

Bu betiğin, sistemin geçici dosyalarına (C:\Windows\Temp ve Prefetch) erişebilmesi ve Güç Planı ayarlarını değiştirebilmesi için **Yönetici Yetkisiyle** çalıştırılması zorunludur.

1.  Dosyayı bilgisayarınıza kaydedin (Örn: `hizlandirici.py`).
2.  Windows arama çubuğuna **CMD** yazın.
3.  **"Komut İstemi"** uygulamasına sağ tıklayın ve **"Yönetici olarak çalıştır"** seçeneğini seçin.
4.  CMD penceresinde betiği kaydettiğiniz dizine gidin ve çalıştırın:
    ```bash
    python hizlandirici.py
    ```

    ### 💡 Önemli Not: Prefetch Temizliği

Prefetch dizininin (C:\Windows\Prefetch) temizlenmesi, biriken gereksiz dosyaları atar. Ancak bu, Windows'un bazı uygulamaları ilk açışını geçici olarak yavaşlatabilir. Windows zamanla performansı tekrar optimize edecektir.



