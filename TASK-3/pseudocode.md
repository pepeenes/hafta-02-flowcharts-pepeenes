digraph ATM {
  rankdir=TB;
  node [fontname="Helvetica"];

  /* Düğüm şablonları */
  Start [shape=oval, label="Başla"];
  End   [shape=oval, label="Bitir"];

  InputCard   [shape=parallelogram, label="Kartın takıldı / Kart bilgisi okundu"];
  EnterPIN    [shape=parallelogram, label="PIN gir (giriş)"];
  IncrementAttempts [shape=box, label="deneme_sayısı = deneme_sayısı + 1"];
  CheckPIN    [shape=diamond, label="PIN doğru mu?"];
  BlockCard   [shape=box, label="Kart bloke edildi / İşlem sonlandırıldı"];
  ShowLockMsg [shape=parallelogram, label="Kilit bilgisi göster"];

  AskAmount   [shape=parallelogram, label="Çekmek istediğiniz tutarı gir (girdi)"];
  CheckMultiple20 [shape=diamond, label="Tutar 20 TL'nin katı mı?"];
  ShowMultipleError [shape=parallelogram, label="Hata: Tutar 20 TL katı olmalı (çıktı)"];

  CheckDailyLimit [shape=diamond, label="Günlük limit aşılıyor mu?"];
  ShowDailyLimitError [shape=parallelogram, label="Hata: Günlük limit aşılıyor (çıktı)"];

  CheckBalance [shape=diamond, label="Hesap bakiyesi yeterli mi?"];
  ShowBalanceError [shape=parallelogram, label="Hata: Yetersiz bakiye (çıktı)"];

  DispenseCash [shape=box, label="Nakit ver (para ver)"];
  UpdateBalance [shape=box, label="Bakiye = Bakiye - tutar\nGünlük_tutar += tutar"];
  PrintReceipt  [shape=parallelogram, label="Fiş yazdır / İşlem bildirimi (çıktı)"];

  AskRepeat   [shape=diamond, label="Başka işlem yapmak ister misiniz?"];
  ReturnToMenu [shape=box, label="Ana ekrana dön / Menü"];

  /* PIN deneme kontrolu */
  InitAttempts [shape=box, label="deneme_sayısı = 0"];
  MaxAttemptsCheck [shape=diamond, label="deneme_sayısı >= 3?"];
  ShowAttemptsLeft [shape=parallelogram, label="Kalan deneme hakkı göster (çıktı)"];

  /* Kenar (ok) bağlantıları ve etiketleri */
  Start -> InputCard [label="Başla -> Kart"];
  InputCard -> InitAttempts [label="Kart okundu"];
  InitAttempts -> EnterPIN [label="PIN sor"];
  EnterPIN -> IncrementAttempts [label="PIN girildi"];
  IncrementAttempts -> CheckPIN [label="PIN kontrol"];
  CheckPIN -> ShowLockMsg [label="Hayır" color=black] ;
  ShowLockMsg -> End [label="Kart bloke / Bitir"];

  CheckPIN -> MaxAttemptsCheck [label="Hayır" color=black];
  MaxAttemptsCheck -> BlockCard [label="Evet"];
  BlockCard -> ShowLockMsg [label="Kart bloke"];

  MaxAttemptsCheck -> ShowAttemptsLeft [label="Hayır"];
  ShowAttemptsLeft -> EnterPIN [label="Tekrar PIN iste"];

  CheckPIN -> AskAmount [label="Evet" color=black];

  /* İşlem akışı */
  AskAmount -> CheckMultiple20 [label="Tutar girildi"];
  CheckMultiple20 -> ShowMultipleError [label="Hayır"];
  ShowMultipleError -> AskAmount [label="Tekrar tutar iste"];

  CheckMultiple20 -> CheckDailyLimit [label="Evet"];
  CheckDailyLimit -> ShowDailyLimitError [label="Evet"];
  ShowDailyLimitError -> AskAmount [label="Yeni tutar iste"];

  CheckDailyLimit -> CheckBalance [label="Hayır"];

  CheckBalance -> ShowBalanceError [label="Hayır"];
  ShowBalanceError -> AskRepeat [label="Tekrar dene/İptal?"];

  CheckBalance -> DispenseCash [label="Evet"];
  DispenseCash -> UpdateBalance [label="Nakit verme tamamlandı"];
  UpdateBalance -> PrintReceipt [label="Bakiye güncellendi"];
  PrintReceipt -> AskRepeat [label="Fiş/Onay gösterildi"];

  /* İşlem tekrarı veya bitiş */
  AskRepeat -> AskAmount [label="Evet"];
  AskRepeat -> ReturnToMenu [label="Hayır - Menü"];
  ReturnToMenu -> End [label="Çıkış / Kart geri ver"];

  /* PIN yanlış akışı (detaylı) */
  CheckPIN -> IncrementAttempts [label="Hayır (PIN yanlış)"];
  /* (yukarıda IncrementAttempts zaten bağlı) */

  /* Ek kenarlar: göster kalan hak */
  IncrementAttempts -> MaxAttemptsCheck [label="Deneme kontrolü"];

  /* Estetik notlar: cluster ile gruplayabilirsin (opsiyonel) */
}
