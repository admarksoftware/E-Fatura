# 🧾 eFatura

$client = new InvoiceManager();
```
Giriş bilgilerinizi chain fonksiyonlarla tanımlayabiliyorsunuz, bu production için geçerlidir.
```php
// Production environment
$client->setUsername("XXX")->setPassword("YYY");
// VEYA
$client->setCredentials("XXX", "YYY");
```

```php
// Test Environment
$client->setDebugMode(true)->setTestCredentials();
```
Ayrıca bilgilerinizi görüntülemek isterseniz:
```php
$client->getCredentials();
```

```php
$client->connect();
```

```php
// Tüm faturaları listele
$client->getInvoicesFromAPI("01/01/2020", "08/02/2020");
```
```php
$fatura_detaylari  = [
"belgeNumarasi"  =>  "", // Zorunlu değil
"faturaTarihi"  =>  "08/02/2020",
"saat"  =>  "09:07:48",
"paraBirimi"  =>  "TRY",
"dovzTLkur"  =>  "0",
"faturaTipi"  =>  "SATIS",
"hangiTip"  =>  "5000/30000",
"vknTckn"  =>  "11111111111",
"aliciUnvan"  =>  "FURKAN KADIOGLU",
"aliciAdi"  =>  "FURKAN",
"aliciSoyadi"  =>  "KADIOGLU",
"binaAdi"  =>  "", // Zorunlu değil
"binaNo"  =>  "", // Zorunlu değil
"kapiNo"  =>  "", // Zorunlu değil
"kasabaKoy"  =>  "", // Zorunlu değil
"vergiDairesi"  =>  "MALTEPE",
"ulke"  =>  "Türkiye",
"bulvarcaddesokak"  =>  "DENEME SK. DENEME MAH.",
"mahalleSemtIlce"  =>  "", // Zorunlu değil
"sehir"  =>  " ",
"postaKodu"  =>  "", // Zorunlu değil
"tel"  =>  "", // Zorunlu değil
"fax"  =>  "", // Zorunlu değil
"eposta"  =>  "", // Zorunlu değil
"websitesi"  =>  "", // Zorunlu değil
"iadeTable"  => [], // Zorunlu değil
"ozelMatrahTutari"  =>  "0", // Zorunlu değil
"ozelMatrahOrani"  =>  0, // Zorunlu değil
"ozelMatrahVergiTutari"  =>  "0", // Zorunlu değil
"vergiCesidi"  =>  " ", // Zorunlu değil
"malHizmetTable"  => [],
"tip"  =>  "İskonto",
"matrah"  =>  100,
"malhizmetToplamTutari"  =>  100,
"toplamIskonto"  =>  "0",
"hesaplanankdv"  =>  18,
"vergilerToplami"  =>  18,
"vergilerDahilToplamTutar"  =>  118,
"odenecekTutar"  =>  118,
"not"  =>  "xxx", // Zorunlu değil
"siparisNumarasi"  =>  "", // Zorunlu değil
"siparisTarihi"  =>  "", // Zorunlu değil
"irsaliyeNumarasi"  =>  "", // Zorunlu değil
"irsaliyeTarihi"  =>  "", // Zorunlu değil
"fisNo"  =>  "", // Zorunlu değil
"fisTarihi"  =>  "", // Zorunlu değil
"fisSaati"  =>  " ", // Zorunlu değil
"fisTipi"  =>  " ", // Zorunlu değil
"zRaporNo"  =>  "", // Zorunlu değil
"okcSeriNo"  =>  "" // Zorunlu değil
];
``
```php
$fatura_detaylari["malHizmetTable"][] = [
"malHizmet"  =>  "Yazılım Geliştirme",
"miktar"  =>  28,
"birim"  =>  "DAY",
"birimFiyat"  =>  "3",
"fiyat"  =>  "84",
"iskontoOrani"  =>  0,
"iskontoTutari"  =>  "0",
"iskontoNedeni"  =>  "",
"malHizmetTutari"  =>  "99",
"kdvOrani"  =>  18,
"vergiOrani" => 0,
"kdvTutari"  =>  "15.12",
"vergininKdvTutari"  =>  "0",
"ozelMatrahTutari" => "0", //zorunlu
];
```
$inv  =  new Invoice();
$inv->mapWithTurkishKeys($fatura_detaylari); // Key yapısı türkçe 🇹🇷
// VEYA
$inv->mapWithEnglishKeys($invoice_details); // Key yapısı ingilizce 🇺🇸
```
```php
$client->setInvoice($inv);
```
Sonrasında da taslak oluşturuyoruz:
```php
$client->createDraftBasicInvoice();
```
Adımıza Düzenlenen Faturaları Sorgulamak için
```php
$client->getInvoicesMeFromAPI("01/01/2021", "31/12/2022");
```
```php
$userInformations = $client->getUserInformationsData();
```

```php
// Sadece vknTckn değiştirilemez.
$userInformations = $userInformations->setUnvan("FRKN Yazılım")->setApartmanNo("4");
$apartmanNo = $userInformations->getApartmanNo(); // 4
```


```php
$userInformations->export(); // Array olarak tüm değişkenleri döndürür.
```



```php
$client->setUserInformations($userInformations); // Manager'a tanımla.
$client->sendUserInformationsData(); // Sunucuya gönder.
```


```php
$client->getInvoiceHTML();
```

```php
$client->getInvoicePDF();
```

```php
$client->getDownloadURL();
```
```php
$client->cancelInvoice();
```

```php
$client->sendSMSVerification($telefon); // Operasyon id döndürür.
```

```php
$client->verifySMSCode($kod, $operasyonId);
```
```php
$client->getCompanyInfo($TCKimlikNoVeyaVergiNo);
```
```php
$client->logOutFromAPI();
```

```php
$oldInvoice = new Invoice();
$oldInvoice->setUuid("e8277cfa-4ac9-11ea-a5b5-acde48001122");
$client->setInvoice($oldInvoice)->getInvoiceFromAPI();
// {"faturaUuid":"8a4423bc-4aca-11ea-8c30-acde48001122","faturaTarihi":"09\/02\/2020"...
```

```php
$client->setDebugMode(true) // Test urlsine geçtik 
->setTestCredentials() // Test bilgilerini aldık
->connect() // Bilgilerle birlikte sunucuya bağlanıp token aldık.
->setInvoice($inv) // Faturamızı sınıfa tanımladık (Invoice sınıfı kullanılmalı)
->createDraftBasicInvoice() // Taslak faturamızı oluşturduk
->getDownloadURL(); // İndirme adresini aldık

// https://earsivportaltest.efatura.gov.tr/earsiv-services/download?token=b8b6c261c511a9b2757279c0111b538a2f02d98ae2df6205448d002687cab8cf74ce04d187bf0c6ce839dee40a6a8aad003aa6d5946ba02a0942ceb10bde327f&ettn=85933f42-4ab1-11ea-922e-acde48001122&belgeTip=FATURA&onayDurumu=Onaylandı&cmd=downloadResource
```

$gunBirim = UnitType::GUN; // DAY
$turkLirasi = CurrencyType::TURK_LIRASI; // TRY
$satisFaturasi = InvoiceType::SATIS; // SATIŞ
$gurcistanUlkesi = Country::GURCISTAN; // Gürcistan
```
$inv  =  new Invoice();

$invoice_details = [
    "uuid" => $uuid,
    "documentNumber" => $documentNumber,
    "date" => $date,
    "time" => $time,
    "currency" => $currency,
    "currencyRate" => $currencyRate,
    "invoiceType" => $invoiceType,
    "taxOrIdentityNumber" => $taxOrIdentityNumber,
    "invoiceAcceptorTitle" => $invoiceAcceptorTitle,
    "invoiceAcceptorName" => $invoiceAcceptorName,
    "invoiceAcceptorLastName" => $invoiceAcceptorLastName,
    "buildingName" => $buildingName,
    "buildingNumber" => $buildingNumber,
    "doorNumber" => $doorNumber,
    "town" => $town,
    "taxAdministration" => $taxAdministration,
    "country" => $country,
    "avenueStreet" => $avenueStreet,
    "district" => $district,
    "city" => $city,
    "postNumber" => $postNumber,
    "telephoneNumber" => $telephoneNumber,
    "faxNumber" => $faxNumber,
    "email" => $email,
    "website" => $website,
    "refundTable" => $refundTable,
    "specialBaseAmount" => $specialBaseAmount,
    "specialBasePercent" => $specialBasePercent,
    "specialBaseTaxAmount" => $specialBaseTaxAmount,
    "taxType" => $taxType,
    "itemOrServiceList" => $itemOrServiceList,
    "type" => $type,
    "base" => $base,
    "itemOrServiceTotalPrice" => $itemOrServiceTotalPrice,
    "totalDiscount" => $totalDiscount,
    "calculatedVAT" => $calculatedVAT,
    "taxTotalPrice" => $taxTotalPrice,
    "includedTaxesTotalPrice" => $includedTaxesTotalPrice,
    "paymentPrice" => $paymentPrice,
    "note" => $note,
    "orderNumber" => $orderNumber,
    "orderData" => $orderData,
    "waybillNumber" => $waybillNumber,
    "waybillDate" => $waybillDate,
    "receiptNumber" => $receiptNumber,
    "voucherDate" => $voucherDate,
    "voucherTime" => $voucherTime,
    "voucherType" => $voucherType,
    "zReportNumber" => $zReportNumber,
    "okcSerialNumber" => $okcSerialNumber
];

$inv->mapWithEnglishKeys($invoice_details); //
```


```php
$inv->setUuid("Buraya kendi fatura idniz") 
->setCountry("Türkiye")
->getCurrencyRate(); // TRY
```


```php
    $inv = new Invoice($data); // data arrayinden keylere göre tüm verileri alır.
    $inv->export(); // tüm verileri çıkartır.
```

```
composer test
```
