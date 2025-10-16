// Değişkenler
MAX_PIN_ATTEMPTS <- 3
DAILY_LIMIT <- 2000            // günlük çekim limiti (örnek)
NOTE_UNIT <- 20                // en küçük banknot birimi
cardInserted <- false
sessionActive <- false

// Fonksiyonlar (sadeleştirilmiş)
function authenticatePIN(account):
    attempts <- 0
    while attempts < MAX_PIN_ATTEMPTS:
        pin <- prompt("Lütfen PIN giriniz:")
        if pin == account.pin:
            return true
        else:
            attempts <- attempts + 1
            print("Hatalı PIN. Kalan deneme: " + (MAX_PIN_ATTEMPTS - attempts))
    // 3 hatalı giriş
    account.blocked <- true
    print("Kart bloke oldu. Banka ile iletişime geçiniz.")
    return false

function canWithdraw(account, amount):
    if amount % NOTE_UNIT != 0:
        print("Lütfen " + NOTE_UNIT + " TL'nin katları şeklinde bir tutar giriniz.")
        return false
    if amount > account.balance:
        print("Yetersiz bakiye.")
        return false
    if (account.withdrawnToday + amount) > DAILY_LIMIT:
        print("Günlük limit aşılıyor. Kalan limit: " + (DAILY_LIMIT - account.withdrawnToday))
        return false
    return true

function dispenseCash(amount):
    // Gerçekte kasa mekanizması çalışır
    print(amount + " TL veriliyor...")
    // burada not dağılımı hesaplanabilir
    print("Fiş hazırlanıyor...")

// Ana program
while true:
    wait until cardInserted = true
    sessionActive <- true
    card <- readCard()
    if card == null:
        continue

    // Kart kontrolü
    if card.account.blocked:
        print("Kart bloke. İşlem yapılamaz.")
        ejectCard()
        sessionActive <- false
        cardInserted <- false
        continue

    // PIN doğrulama
    if not authenticatePIN(card.account):
        // Kart bloke olduysa oturumu bitir
        ejectCard()
        sessionActive <- false
        cardInserted <- false
        continue

    // İşlem döngüsü (birden fazla işlem yapma imkanı)
    repeatTransaction <- true
    while repeatTransaction:
        print("1) Bakiye sorgula")
        print("2) Para çek")
        print("3) Kart iade / Çıkış")
        choice <- prompt("Seçiminiz:")

        if choice == "1":
            print("Hesap bakiyesi: " + card.account.balance + " TL")
            print("Bugün çekilen toplam: " + card.account.withdrawnToday + " TL")

        else if choice == "2":
            // Miktar alma ve kontroller
            amount <- toInteger(prompt("Çekilmek istenen tutarı giriniz:"))
            if canWithdraw(card.account, amount):
                // Hesaptan düş ve günlük toplamı güncelle
                card.account.balance <- card.account.balance - amount
                card.account.withdrawnToday <- card.account.withdrawnToday + amount
                dispenseCash(amount)
                print("İşlem başarılı. Kalan bakiye: " + card.account.balance + " TL")
            // canWithdraw içindeki mesajlar gerekli yönlendirmeyi yapar

        else if choice == "3":
            print("Kart iade ediliyor...")
            ejectCard()
            sessionActive <- false
            cardInserted <- false
            repeatTransaction <- false
            break

        else:
            print("Geçersiz seçim.")

        // Başka işlem yapmak ister misiniz?
        if sessionActive:
            cont <- prompt("Başka işlem yapmak ister misiniz? (E/H):")
            if cont == "H" or cont == "h":
                print("Kart iade ediliyor...")
                ejectCard()
                sessionActive <- false
                cardInserted <- false
                repeatTransaction <- false

    // Oturum sonu, döngü başa döner (yeni kart beklenir)
end while
