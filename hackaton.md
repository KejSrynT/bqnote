Sistem basitçe 3 ana parçadan oluşuyor: Veriyi temizleyen Python kodu (`data_cleaner.py`), veri tabanını yöneten SQL kodu (`db_reporter.py`) ve bunları birleştiren ana menü (`main.py`).

Aşağıdaki rehberde ihtiyacın olan tüm kodları, Git komutlarını ve mantığı bulabilirsin. Direkt kopyalayıp bilgisayarında taslakları oluşturmaya başla.

## 1. Temel Git Komutları Cheat Sheet

Hackatonda adımları kaydetmek (commit) puan getirecektir. Terminalde sırasıyla bunları kullanacaksın:

Bash

```
# Proje klasörünün içinde Git'i başlatır (İlk adım)
git init

# Değişiklik yaptığın dosyaları takibe alır
git add data_cleaner.py
# (Veya tüm dosyaları eklemek için: git add .)

# Değişiklikleri bir başlık altında kaydeder (Her başarılı adımdan sonra yap)
git commit -m "Veri temizleme fonksiyonu eklendi"
```

## 2. Öğrenci A: Veri Temizleme Modülü (`data_cleaner.py`)

Bu kod, Pandas kütüphanesini kullanarak bozuk `raw_tickets.csv` dosyasını okur, eksik verileri uçurur ve `customer_id` sütununda sayı olmayan satırları (örneğin "ABC") temizler.

Python

```
import pandas as pd

def clean_data(input_file="raw_tickets.csv", output_file="clean_tickets.csv"):
    try:
        # 1. CSV dosyasını oku
        df = pd.read_csv(input_file)
        print(f" Log: {input_file} başarıyla okundu.")
        
        # 2. Herhangi bir sütunu boş (NaN) olan tüm satırları sil
        df.dropna(inplace=True)
        
        # 3. customer_id sütunundaki sayısal olmayan verileri temizle
        # errors='coerce' ifadesi, sayıya çevrilemeyen her şeyi (örn: ABC) NaN yapar
        df['customer_id'] = pd.to_numeric(df['customer_id'], errors='coerce')
        
        # Sayısal olmadığı için NaN'a dönüşen satırları tekrar dropna ile temizliyoruz
        df.dropna(inplace=True)
        
        # ID'leri tam sayı (int) tipine çevirelim (Düzgün görünmesi için)
        df['customer_id'] = df['customer_id'].astype(int)
        
        # 4. Temizlenmiş veriyi yeni bir CSV olarak dışarı aktar
        df.to_csv(output_file, index=False)
        print(f" Başarılı: Temiz veri '{output_file}' olarak kaydedildi.\n")
        return True
        
    except Exception as e:
        print(f" Hata: Veri temizlenirken bir sorun oluştu -> {e}")
        return False

# Tek başına test etmek istersen alttaki satırı aktif edebilirsin
# if __name__ == "__main__": clean_data()
```

## 3. Öğrenci B: Veri Tabanı ve Raporlama Modülü (`db_reporter.py`)

Bu kod, SQLite üzerinde `raynet.db` adında bir veri tabanı ve `Tickets` tablosu oluşturur. Güvenlik gerekçesiyle SQL sorgularında `?` parametresini kullanır.

Python

```
import sqlite3
import csv

DB_NAME = "raynet.db"

def init_db():
    """Veri tabanını ve gerekli tabloyu oluşturur."""
    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()
    
    # Senaryodaki tablonun aynısını kuruyoruz
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS Tickets (
            ticket_id INTEGER PRIMARY KEY,
            customer_id INTEGER,
            description TEXT,
            status TEXT,
            priority TEXT
        )
    ''')
    conn.commit()
    conn.close()
    print(" Log: SQLite Veri tabanı ve Tickets tablosu hazır.")

def insert_csv_to_db(csv_file):
    """CSV dosyasını okur ve güvenli şekilde SQL tablosuna insert eder."""
    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()
    
    # Her import işleminde mükerrer (çift) kayıt olmasın diye tabloyu temizliyoruz
    cursor.execute("DELETE FROM Tickets")
    
    try:
        with open(csv_file, mode='r', encoding='utf-8') as f:
            reader = csv.DictReader(f)
            
            for row in reader:
                # Güvenlik için '?' işareti (Parametrik Sorgu) kullanımı zorunludur!
                cursor.execute('''
                    INSERT INTO Tickets (ticket_id, customer_id, description, status, priority)
                    VALUES (?, ?, ?, ?, ?)
                ''', (row['ticket_id'], row['customer_id'], row['description'], row['status'], row['priority']))
                
        conn.commit()
        print(f" Başarılı: '{csv_file}' verileri veri tabanına aktarıldı.")
    except Exception as e:
        print(f" Hata: Veri tabanına yazılırken hata oluştu -> {e}")
    finally:
        conn.close()

def show_management_reports():
    """Yönetim için SQL raporları üretir."""
    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()
    
    print("\n=============================================")
    print("             YÖNETİM RAPORLARI              ")
    print("=============================================")
    
    # Rapor 1: Durumu 'Open' (Açık) olan kritik talepler
    print("\n[Açık (Open) Durumdaki Talepler]")
    cursor.execute("SELECT * FROM Tickets WHERE status='Open'")
    open_tickets = cursor.fetchall()
    for t in open_tickets:
        print(f"Ticket ID: {t[0]} | Müşteri: {t[1]} | Öncelik: {t[4]} | Özet: {t[2][:30]}...")
        
    # Rapor 2: Toplam Ticket Sayısı ve Öncelik Dağılımı
    print("\n[Genel İstatistikler]")
    cursor.execute("SELECT COUNT(*) FROM Tickets")
    total = cursor.fetchone()[0]
    print(f"Sistemdeki Toplam Kurtarılan Ticket: {total}")
    
    cursor.execute("SELECT priority, COUNT(*) FROM Tickets GROUP BY priority")
    for row in cursor.fetchall():
        print(f" - {row[0]} Öncelikli: {row[1]} adet")
        
    print("=============================================\n")
    conn.close()
```

## 4. Birleşme Aşaması: Ana Arayüz Dosyası (`main.py`)

Öğrenci A ve Öğrenci B kodlarını birleştirdiğinde, tüm projeyi ayağa kaldıracak olan ana kontrol merkezidir. PDF'teki CLI tasarımının birebir aynısını içerir.

Python

```
import os
from data_cleaner import clean_data
from db_reporter import init_db, insert_csv_to_db, show_management_reports

def main():
    # Sistem başlarken veri tabanı yapısını otomatik hazırla
    init_db()
    
    while True:
        # PDF'te istenen birebir konsol tasarımı
        print("=============================================")
        print("           RAYNET DATA CENTER v1.0           ")
        print("=============================================")
        print("[1] Ham Veriyi Temizle ve Veri Tabanına Aktar")
        print("[2] Yönetim Raporlarını Görüntüle")
        print("[0] Çıkış")
        print("=============================================")
        print("Hazırlayan: MTT Erasmus+ Kulübü\n")
        
        secim = input("İşlem seçiniz: ")
        
        if secim == "1":
            # Öğrenci A ve B'nin birleştiği an
            if os.path.exists("raw_tickets.csv"):
                print("\n[İşlem 1] Temizlik ve aktarım başlatılıyor...")
                # Önce temizle
                success = clean_data("raw_tickets.csv", "clean_tickets.csv")
                if success:
                    # Temizlendiyse DB'ye aktar
                    insert_csv_to_db("clean_tickets.csv")
            else:
                print("\nHata: 'raw_tickets.csv' dosyası bulunamadı! Lütfen dosyanın klasörde olduğundan emin olun.\n")
                
        elif secim == "2":
            # Raporlama fonksiyonunu çağırır
            if os.path.exists("raynet.db"):
                show_management_reports()
            else:
                print("\nHata: Veri tabanı bulunamadı. Önce 1. seçeneği çalıştırın.\n")
                
        elif secim == "0":
            print("\nSistemden çıkılıyor. Kurtarma operasyonu tamamlandı!")
            break
        else:
            print("\nGeçersiz seçim! Lütfen 1, 2 veya 0 yazın.\n")

if __name__ == "__main__":
    main()
```

## 5. Hackathon Günü Klasör Yapısı Nasıl Olmalı?

Çalışırken hata almamak için tüm dosyalarının aynı klasörün içinde olmasına dikkat et. Klasör düzenin tam olarak şöyle görünmeli:

Plaintext

```
 Klasor_Adi/
 │
 ├── raw_tickets.csv         # Öğretmenin vereceği bozuk veri
 ├── mock_clean_data.csv     # Öğretmenin vereceği test verisi
 │
 ├── data_cleaner.py         # Öğrenci A'nın kodu
 ├── db_reporter.py          # Öğrenci B'nin kodu
 └── main.py                 # İkisini birleştiren ana menü (Bunu çalıştıracaksın)
```

> **Son Dakika Tüyosu:** Kodları bilgisayarına kaydettikten sonra terminalden veya VS Code üzerinden `python main.py` yazarak test et. `raw_tickets.csv` adında geçici bir test dosyası oluşturup içine kasıtlı olarak boş satırlar veya harfli ID'ler koyarak sistemin bunları nasıl elediğini canlı canlı gör. Yarın başarılar!