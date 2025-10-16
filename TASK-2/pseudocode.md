BAŞLA

KULLANICI_GIRIS:
    KULLANICI_ADI ve SIFRE gir
    EĞER bilgiler doğruysa
        GİRİŞ BAŞARILI
    DEĞİLSE
        HATA mesajı göster
        TEKRAR DENE
    SON

ÜRÜN_GEZİNME:
    KATEGORİLERİ listele
    DÖNGÜ: Kullanıcı çıkış yapana kadar
        SEÇİLEN kategoride ürünleri göster
        Kullanıcı ürün seçerse
            SEPETE_EKLE:
                EĞER ürün stokta varsa
                    Ürünü sepete ekle
                DEĞİLSE
                    "Stokta yok" mesajı göster
                SON
        SEPET_GÖRÜNTÜLE:
            DÖNGÜ: Kullanıcı sepeti düzenlemek isteyene kadar
                Ürünleri ve toplam fiyatı göster
                Ekle / Sil işlemleri yapılabilir
            SON
    SON_DÖNGÜ

İNDİRİM_KODU:
    Kullanıcı indirim kodu girerse
        EĞER kod geçerliyse indirimi uygula
        DEĞİLSE "Geçersiz kod" mesajı göster
    SON

MIN_TUTAR_KONTROL:
    EĞER toplam_tutar < 50 TL
        "Minimum 50 TL alışveriş yapmalısınız" mesajı göster
        Ürün eklemeye yönlendir
    SON

KARGO_HESAPLA:
    EĞER toplam_tutar >= 200 TL
        KARGO = ÜCRETSİZ
    DEĞİLSE
        KARGO = 30 TL
    SON

ÖDEME:
    Ödeme yöntemi seç (Kredi Kartı / Kapıda Ödeme)
    EĞER yöntem geçerliyse devam et
    DEĞİLSE tekrar seçim iste
    SON

SİPARİŞ_ONAY:
    Sipariş özetini göster
    Onay al
    "Siparişiniz alınmıştır" mesajı göster

BİTİR
