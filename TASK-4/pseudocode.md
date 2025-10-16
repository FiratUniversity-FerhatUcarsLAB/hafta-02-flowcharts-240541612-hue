Başla

// 1. Öğrenci girişi
ÖğrenciNo, Sifre ← Kullanıcıdan al
Eğer GirişDoğru(ÖğrenciNo, Sifre) ise
    Giriş Başarılı
    ToplamKredi ← 0
    KayıtlıDersler ← Boş Liste

    Döngü: Ders işlemleri
        DersListesi ← Tüm dersleri getir
        Her Ders için dersListesi
            DersAdı, Kontenjan, ÖnKoşul, DersSaatleri, Kredi ← ders bilgisi

            // Kontenjan kontrolü
            Eğer Kontenjan > 0 ise
                // Ön koşul kontrolü
                Eğer ÖnKoşulTamamlandı(ÖğrenciNo, ÖnKoşul) ise
                    // Zaman çakışması kontrolü
                    Eğer ZamanUygun(DersSaatleri, KayıtlıDersler) ise
                        // Kredi limiti kontrolü
                        Eğer (ToplamKredi + Kredi) ≤ 35 ise
                            Ders ekleme/çıkarma işlemi sor
                            Eğer Öğrenci seçerse DersEkle(DersAdı)
                                ToplamKredi += Kredi
                                Kontenjan -= 1
                                KayıtlıDersler ← DersAdı
                            Yoksa DersÇıkar(DersAdı)
                                ToplamKredi -= Kredi
                                Kontenjan += 1
                                KayıtlıDersler ← DersAdı'dan sil
                        Yoksa
                            "Kredi limiti aşılıyor!" mesajı göster
                    Yoksa
                        "Ders zaman çakışması var!" mesajı göster
                Yoksa
                    "Ön koşul dersi tamamlanmamış!" mesajı göster
            Yoksa
                "Kontenjan dolu!" mesajı göster
        Döngü sonu

        // Danışman onayı kontrolü
        GPA ← ÖğrenciGPA(ÖğrenciNo)
        Eğer GPA < 2.5 ise
            DanışmanOnayıGerekli ← Doğru
        Yoksa
            DanışmanOnayıGerekli ← Yanlış

        // Kayıt özeti
        KayıtÖzetiGoster(KayıtlıDersler, ToplamKredi)
        KayıtOnayıSor()
        Eğer Onaylandı ise
            "Kayıt tamamlandı" mesajı göster
        Yoksa
            Döngüye dön
    Döngü sonu

Aksi halde
    "Giriş başarısız" mesajı göster

Bitir
