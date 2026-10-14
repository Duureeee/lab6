# Лаборатори №6 — Category-Partition ба PICT

**Хичээл:** F.CSA313 — Программ хангамжийн чанарын баталгаа ба тест  
**Оюутан:** Ц.Алтжин  
**Оюутны код:** 23B1NUM2568

## Зорилго

Category-Partition аргаар банкны шилжүүлгийн функцэд тестийн сонголт,
төлөөлөх утга, хязгаарлалт, хүлээгдэх үр дүнг тодорхойлох.

Microsoft PICT хэрэгслээр pairwise тестүүд үүсгэж,
гараар боловсруулсан тестүүдтэй харьцуулах.

## PICT суулгалт

PICT-ийг Microsoft-ийн GitHub репозиториос эх кодоор татаж,
CMake ашиглан Ubuntu WSL орчинд build хийсэн.

Суулгалтын нотолгоо: `results/pict-build.txt`

## Хэсэг A — Category-Partition

### A1. Сонголт ба төлөөлөх утгууд

| Параметр / нөхцөл | Сонголт | Төлөөлөх утга | Тайлбар |
|---|---|---|---|
| From | Valid | ACC001 | Байгаа, царцаагдаагүй данс |
| From | Frozen | ACC003 | Царцаагдсан данс |
| From | Missing | ACC999 | Системд байхгүй данс |
| To | Valid | ACC002 | Хүлээн авагчийн хүчинтэй данс |
| To | Missing | ACC998 | Хүлээн авагчийн данс байхгүй |
| To | SameAsFrom | ACC001 | Өөрийн данс руу шилжүүлэх |
| Amount | Normal | 50000 | Эерэг, үлдэгдэл болон лимитэд багтсан |
| Amount | Zero | 0 | Тэг дүн |
| Amount | Negative | -100 | Сөрөг дүн |
| Amount | OverBalance | 200000 | Үлдэгдлээс хэтэрсэн |
| Amount | OverLimit | 150000 | Өдрийн лимитээс хэтэрсэн |
| Currency | Same | MNT | Дансны валюттай ижил |
| Currency | Different | USD | Валютын хөрвүүлэлт шаардлагатай |
| DailyLimit | Available | 500000 үлдсэн | Өдрийн лимит хүрэлцээтэй |
| DailyLimit | NearlyUsed | 10000 үлдсэн | Өдрийн лимит бараг дууссан |
| Balance | Sufficient | 1000000 | Хангалттай үлдэгдэл |
| Balance | Insufficient | 10000 | Хүрэлцээгүй үлдэгдэл |

**Далд орчны нөхцөл:** Дансны үлдэгдэл, өдрийн лимитийн ашиглалт нь хүсэлтийн body-д байхгүй боловч шилжүүлгийн үр дүнд нөлөөлнө.

**Валютын дүрэм:** Amount нь хүсэлтэд заасан currency-гийн валютаар өгөгдөнө. Үлдэгдэл болон өдрийн лимитийг валют хөрвүүлсний дараах дүнгээр шалгана.

### A2. Хязгаарлалтууд

#### Алдааны давуу эрэмбэ

Нэг хүсэлтэд олон алдаа байвал дараах дарааллаар шалгана:

1. ERROR_NO_FROM
2. ERROR_FROZEN
3. ERROR_NO_TO
4. ERROR_SAME_ACCOUNT
5. ERROR_BAD_AMOUNT
6. ERROR_INSUFFICIENT
7. ERROR_LIMIT

Энэ дарааллыг дансны оршин байх эсэхийг эхэлж шалгаж,
дараа нь шилжүүлгийн дүнг баталгаажуулах зарчмаар сонгосон.
Энэ нь лабораторийн тест дизайн дахь таамагласан эрэмбэ бөгөөд
бодит хэрэгжүүлэлтийн баталгаажсан дараалал биш.

#### ERROR, SINGLE, IF тэмдэглэгээ

| Сонголт | Утга | Тэмдэглэгээ | Үндэслэл |
|---|---|---|---|
| From | Valid | Энгийн | Шилжүүлэг хийх боломжтой данс |
| From | Frozen | ERROR | Царцаагдсан дансанд шилжүүлэг хийхгүй |
| From | Missing | ERROR | Илгээгчийн данс байхгүй |
| To | Valid | Энгийн | Хүчинтэй хүлээн авагч |
| To | Missing | ERROR | Хүлээн авагчийн данс байхгүй |
| To | SameAsFrom | ERROR, IF | Зөвхөн From данс байгаа үед ижил данс гэж шалгана |
| Amount | Normal | Энгийн | Зөвшөөрөгдөх дүн |
| Amount | Zero | ERROR | amount > 0 нөхцөлийг зөрчинө |
| Amount | Negative | ERROR, SINGLE | Сөрөг дүнгийн тусгай тест |
| Amount | OverBalance | ERROR, IF | Үлдэгдлээс хэтэрсэн дүн |
| Amount | OverLimit | ERROR, IF | Өдрийн лимитээс хэтэрсэн дүн |
| Currency | Same | Энгийн | Хөрвүүлэлт шаардлагагүй |
| Currency | Different | IF | Валютын хөрвүүлэлт шаардлагатай |
| DailyLimit | Available | Энгийн | Өдрийн лимит хүрэлцээтэй |
| DailyLimit | NearlyUsed | IF | Үлдсэн лимиттэй харьцуулж шалгана |
| Balance | Sufficient | Энгийн | Дансанд хангалттай мөнгө байгаа |
| Balance | Insufficient | IF | Хөрвүүлсний дараах дүн үлдэгдлээс хэтэрч болно |

#### IF нөхцөлүүд

- To = SameAsFrom бол From = Valid эсвэл Frozen байна.
- Amount = OverBalance бол хөрвүүлсэн дүн үлдэгдлээс их байна.
- Amount = OverLimit бол хөрвүүлсэн дүн үлдсэн өдрийн лимитээс их байна.
- Currency = Different бол ханшаар хөрвүүлсний дараа үлдэгдэл болон лимитийг шалгана.
- From = Missing үед үлдэгдэл, лимитийн шалгалт хийгдэхгүй.


#### Бүрэн олонлогийн тооцоо (N → M)

| Сонголт | Утгын тоо |
|---|---:|
| From | 3 |
| To | 3 |
| Amount | 5 |
| Currency | 2 |
| DailyLimit | 2 |
| Balance | 2 |

Хязгаарлалтгүй бүрэн олонлог:

N = 3 × 3 × 5 × 2 × 2 × 2 = 360

Хязгаарлалтын дараах тооцоо:

From × To = 8 зөвшөөрөгдөх хослол.
Amount × Balance × DailyLimit = 12 зөвшөөрөгдөх хослол.
Currency = 2 сонголт.

M = 8 × 12 × 2 = 192

Иймд N → M = 360 → 192.

Энэ тооцоонд SameAsFrom ба Missing-ийн зөрчил, мөн
Amount-ийн ангиллуудыг Balance болон DailyLimit-ийн
нөхцөлтэй нийцүүлсэн загварын хязгаарлалтуудыг ашиглав.

#### N → M тооцоонд ашигласан нэмэлт нөхцөлүүд

Энэ тооцоонд дараах загварын нөхцөлүүдийг ашигласан.

1. To = SameAsFrom бол From = Missing байж болохгүй.
2. Amount = Normal бол Balance = Sufficient,
   DailyLimit = Available байна.
3. Amount = OverBalance бол Balance = Insufficient байна.
   DailyLimit-ийн хоёр төлөв боломжтой.
4. Amount = OverLimit бол Balance = Sufficient,
   DailyLimit = NearlyUsed байна.
5. Amount = Zero эсвэл Negative бол Balance болон
   DailyLimit-ийн бүх төлөв боломжтой.
6. Currency = Same болон Different аль аль нь боломжтой.

Amount × Balance × DailyLimit-ийн зөвшөөрөгдөх тоо:

| Amount | Хослолын тоо |
|---|---:|
| Normal | 1 |
| Zero | 4 |
| Negative | 4 |
| OverBalance | 2 |
| OverLimit | 1 |
| **Нийт** | **12** |

Иймээс:

N = 360
M = 8 × 12 × 2 = 192

N → M = 360 → 192

Эдгээр нь тест дизайн хийхэд сонгосон загварын
хязгаарлалтууд бөгөөд API-ийн нэмэлт бизнес дүрэм биш.

### A3. Тестийн спецификацийн хүснэгт

Дараах тестүүдэд From, To, Amount, Currency, Balance,
DailyLimit гэсэн сонголтуудыг ашигласан.

| ID | From | To | Amount | Currency | Balance | DailyLimit | Хүлээгдэх үр дүн |
|---|---|---|---|---|---|---|---|
| TC01 | Valid | Valid | Normal | Same | Sufficient | Available | 201 OK |
| TC02 | Missing | Valid | Normal | Same | Sufficient | Available | 200 ERROR_NO_FROM |
| TC03 | Frozen | Valid | Normal | Same | Sufficient | Available | 200 ERROR_FROZEN |
| TC04 | Valid | Missing | Normal | Same | Sufficient | Available | 200 ERROR_NO_TO |
| TC05 | Valid | SameAsFrom | Normal | Same | Sufficient | Available | 200 ERROR_SAME_ACCOUNT |
| TC06 | Valid | Valid | Zero | Same | Sufficient | Available | 200 ERROR_BAD_AMOUNT |
| TC07 | Valid | Valid | Negative | Same | Sufficient | Available | 200 ERROR_BAD_AMOUNT |
| TC08 | Valid | Valid | OverBalance | Same | Insufficient | Available | 200 ERROR_INSUFFICIENT |
| TC09 | Valid | Valid | OverLimit | Same | Sufficient | NearlyUsed | 200 ERROR_LIMIT |
| TC10 | Valid | Valid | Normal | Different | Sufficient | Available | 201 OK |
| TC11 | Missing | Missing | Normal | Same | Sufficient | Available | 200 ERROR_NO_FROM |
| TC12 | Frozen | Valid | Zero | Same | Sufficient | Available | 200 ERROR_FROZEN |
| TC13 | Valid | Valid | OverBalance | Different | Insufficient | Available | 200 ERROR_INSUFFICIENT |

#### Тестийн тайлбар

- TC01: Бүх нөхцөл зөв, шилжүүлэг амжилттай.
- TC02–TC09: Алдааны долоон ангиллыг бүрэн шалгана.
- TC07: Сөрөг дүнг тусгай SINGLE тестээр шалгана.
- TC10: Валют хөрвүүлэлттэй амжилттай шилжүүлгийг шалгана.
- TC11: Хоёр данс байхгүй үед ERROR_NO_FROM давамгайлна.
- TC12: Царцаагдсан данс болон тэг дүн зэрэг алдаатай үед ERROR_FROZEN давамгайлна.
- TC13: Валют хөрвүүлсний дараах үлдэгдэл хүрэлцэхгүй нөхцөлийг шалгана.

Амжилттай хүсэлтэд HTTP 201, result = OK буцна.
Алдаатай хүсэлтэд HTTP 200, result = ERROR_... буцна.
Давхар алдааны oracle нь A2-д тодорхойлсон
давуу эрэмбэд үндэслэсэн.
