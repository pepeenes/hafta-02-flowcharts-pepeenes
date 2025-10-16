BASLA

  // --- Değişken tanımlamaları ---
  TANIMLA DOGRU_PIN ← 1234
  TANIMLA BAKIYE ← 5000
  TANIMLA GUNLUK_LIMIT ← 2000
  TANIMLA GUNLUK_CEKILEN ← 0
  TANIMLA HAK ← 3
  TANIMLA PIN ← 0
  TANIMLA DEVAM ← "E"

  // --- PIN DOĞRULAMA ---
  DONGU (HAK > 0)
      YAZ "Lütfen PIN'inizi giriniz:"
      OKU PIN

      EGER PIN = DOGRU_PIN ISE
          YAZ "PIN doğrulandı."
          CIK DONGU
      DEGILSE
          HAK ← HAK - 1
          EGER HAK > 0 ISE
              YAZ "Hatalı PIN. Kalan deneme hakkı: ", HAK
          DEGILSE
              YAZ "3 kez hatalı PIN girildi. Kartınız bloke edildi."
              YAZ "İşlem sonlandırılıyor..."
              BITIR PROGRAM
          BITIR EGER
      BITIR EGER
  BITIR DONGU

  // --- ANA İŞLEM DÖNGÜSÜ ---
  DONGU (DEVAM = "E")
      YAZ "1 - Para Çekme"
      YAZ "2 - Bakiye Görüntüleme"
      YAZ "3 - Çıkış"
      YAZ "Seçiminizi giriniz:"
      OKU SECIM

      EGER SECIM = 1 ISE
          // --- PARA ÇEKME ---
          YAZ "Çekmek istediğiniz tutarı giriniz (20 TL katı):"
          OKU TUTAR

          // TUTAR KONTROLÜ
          EGER TUTAR <= 0 ISE
              YAZ "Geçersiz tutar girdiniz."
          DEGILSE EGER TUTAR % 20 <> 0 ISE
              YAZ "Tutar 20 TL’nin katı olmalıdır."
          DEGILSE EGER TUTAR > BAKIYE ISE
              YAZ "Yetersiz bakiye."
          DEGILSE EGER (GUNLUK_CEKILEN + TUTAR) > GUNLUK_LIMIT ISE
              YAZ "Günlük limit aşıldı. Günlük limit: ", GUNLUK_LIMIT
          DEGILSE
              BAKIYE ← BAKIYE - TUTAR
              GUNLUK_CEKILEN ← GUNLUK_CEKILEN + TUTAR
              YAZ "İşlem başarılı. Çekilen tutar: ", TUTAR
              YAZ "Kalan bakiye: ", BAKIYE
          BITIR EGER

      DEGILSE EGER SECIM = 2 ISE
          // --- BAKİYE GÖRÜNTÜLEME ---
          YAZ "Mevcut bakiyeniz: ", BAKIYE
          YAZ "Bugün çekilen toplam tutar: ", GUNLUK_CEKILEN

      DEGILSE EGER SECIM = 3 ISE
          YAZ "Kart iade ediliyor. İyi günler."
          CIK DONGU

      DEGILSE
          YAZ "Geçersiz seçim. Tekrar deneyiniz."
      BITIR EGER

      // --- İŞLEM TEKRARI SEÇENEĞİ ---
      YAZ "Başka işlem yapmak istiyor musunuz? (E/H):"
      OKU DEVAM

  BITIR DONGU

  YAZ "İyi günler dileriz."

BITIR PROGRAM
