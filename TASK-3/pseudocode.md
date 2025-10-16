BAŞLA

TEKRARLA
    YAZDIR "Lütfen TC Kimlik Numaranızı Giriniz:"
    TC <- GİRİŞ
    EĞER TC GEÇERLİ DEĞİLSE
        YAZDIR "Geçersiz TC Kimlik Numarası!"
    DEĞİLSE
        YAZDIR "Hoşgeldiniz!"
        YAZDIR "İşlem Seçiniz: 1-Randevu Al, 2-Tahlil Sonucu Gör"
        SECIM <- GİRİŞ

        EĞER SECIM = 1 İSE
            YAZDIR "Poliklinik Seçiniz:"
            POLIKLINIK <- GİRİŞ
            YAZDIR "Doktor Listesi Gösteriliyor..."
            DOKTORLAR <- DOKTOR_LISTESI(POLIKLINIK)

            DÖNGÜ HER DOKTOR İÇİN DOKTORLAR
                YAZDIR DOKTOR.AD
            SON

            YAZDIR "Doktor Seçiniz:"
            DOKTOR <- GİRİŞ
            YAZDIR "Uygun Saatler Listeleniyor..."
            SAATLER <- UYGUN_SAATLER(DOKTOR)

            DÖNGÜ HER SAAT İÇİN SAATLER
                YAZDIR SAAT
            SON

            YAZDIR "Randevu Saati Seçiniz:"
            SAAT <- GİRİŞ
            YAZDIR "Randevunuz Onaylandı!"
            YAZDIR "SMS ile bilgilendirme gönderiliyor..."
        
        DEĞİLSE EĞER SECIM = 2 İSE
            YAZDIR "Tahlil Sonuçları Kontrol Ediliyor..."
            EĞER TAHLIL_VAR(TC) = HAYIR İSE
                YAZDIR "Tahlil kaydı bulunamadı."
            DEĞİLSE
                EĞER SONUC_HAZIR(TC) = EVET İSE
                    YAZDIR "Tahlil Sonucunuz Hazır!"
                    YAZDIR "Sonuç Görüntüleniyor..."
                    YAZDIR "PDF olarak indirmek ister misiniz? (E/H)"
                    INDIR <- GİRİŞ
                    EĞER INDIR = "E" İSE
                        PDF_INDIR(TC)
                    SON
                DEĞİLSE
                    YAZDIR "Tahlil sonucu henüz hazır değil, lütfen daha sonra tekrar deneyiniz."
                SON
            SON
        DEĞİLSE
            YAZDIR "Geçersiz Seçim!"
        SON
    SON

    YAZDIR "Başka işlem yapmak ister misiniz? (E/H)"
    DEVAM <- GİRİŞ
UNTIL DEVAM = "H"

YAZDIR "İyi günler dileriz!"
BİTİR
