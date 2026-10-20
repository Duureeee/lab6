## Тайлбар

Би найз охиныхоо лабораторийн ажлыг хийж өгч байхдаа өөрийн лабораторийн ажлын README.md файлд андуурч түүний нэр болон оюутны кодыг бичиж, GitHub-д илгээсэн байна.

Энэ нь миний анхаарал болгоомжгүй байдлаас үүдсэн алдаа бөгөөд өөрийн мэдээллийг зөв оруулж, README файлыг засварласан болно.


---

# Лаборатори №6 — Category-Partition ба PICT

**Хичээл:** F.CSA313 — Программ хангамжийн чанарын баталгаа ба тест
**Оюутны нэр:** Б.Дүүрэнбилэг
**Оюутны код:** B232270032

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

## Хэсэг B — PICT Pairwise Testing

### B1. Browser model — 2-way болон 3-way

Браузерын 6 параметр:
Content, Cookies, Popups, Addons, Attacks, Forgeries.

Бүрэн олонлог:

N = 3 × 3 × 2 × 2 × 2 × 2 = 144

| Арга | Тестийн мөрийн тоо |
|---|---:|
| Бүрэн олонлог | 144 |
| Лекцийн гараар хийсэн 2-way | 9 |
| PICT 2-way | 10 |
| PICT 3-way | 20 |

#### Үр дүнгийн тайлбар

PICT-ийн 2-way тест 10 мөр гарсан бөгөөд лекцийн
гараар боловсруулсан 9 мөрөөс нэгээр олон байна.
Энэ нь PICT greedy алгоритмаар ажилладагтай холбоотой.
Greedy алгоритм нь тестийн тоог багасгахыг зорьдог боловч
хамгийн цөөн мөртэй шийдлийг заавал гаргахгүй.

3-way тест 20 мөртэй гарсан нь 2-way-ээс хоёр дахин олон.
Учир нь 3-way нь гурван параметрийн бүх боломжит
утгын хослолыг хамрах шаардлагатай.

PICT-ийн хоёр хувилбар хоёулаа бүрэн олонлогийн
144 тестээс мэдэгдэхүйц цөөн тест үүсгэсэн.

Нотолгоо:
- results/browser-2way.txt
- results/browser-3way.txt

### B2. Transfer моделийн үр дүнгийн харьцуулалт

| Үзүүлэлт | transfer-nc | transfer | transfer-neg |
|---|---:|---:|---:|
| Нийт тестийн тоо | 16 | 18 | 18 |
| Happy path | 0 | 0 | 2 |
| Нэг мөр дэх хамгийн их алдааны ангилал | 3 | 3 | 1 |
| Олон алдаатай мөр | 13 | 15 | 0 |
| Алдаагүй мөр | 0 | 0 | 2 |

#### Үр дүнгийн тайлбар

Хязгаарлалтгүй transfer-nc.model нь 16 тест үүсгэсэн.
Эдгээрээс 13 мөрөнд хоёр буюу түүнээс олон
алдааны ангилал зэрэг орсон.

IF хязгаарлалттай transfer.model нь 18 тест үүсгэсэн.
Үүнээс 15 мөр олон алдаатай байсан.
Хязгаарлалт нэмэхэд pairwise алгоритмын сонголт
өөрчлөгддөг тул тестийн тоо заавал багасдаггүй.

transfer-neg.model нь мөн 18 тест үүсгэсэн.
Сөрөг утгуудыг ~ тэмдэглэгээгээр ялгаснаар
нэг мөрөнд хамгийн ихдээ нэг сөрөг утга орсон.
Олон алдаатай мөр 0 болж, 2 happy path үүссэн.

Эхний хоёр модельд happy path заавал үүсээгүй.
Учир нь pairwise coverage нь бүх параметр зөв
байх тестийг заавал баталгаажуулдаггүй.

Алдааны ангиллын тооллогод From, To, Amount
гэсэн гурван параметрийн буруу утгуудыг ашигласан.
Энэ нь бодит банкны бүх боломжит алдааны тоо биш.

#### Error masking

Нэг хүсэлтэд олон алдаа зэрэг орвол эхний
шалгалтын алдаа бусад алдааг далдалж болно.
Үүнийг error masking гэж нэрлэнэ.

Жишээлбэл From = Missing, To = Missing,
Amount = Zero гэсэн хүсэлтэд A2-д сонгосон
давуу эрэмбээр ERROR_NO_FROM буцна.

Negative утгуудыг ~ тэмдэглэгээгээр ялгах нь
энэ асуудлыг багасгадаг. Гэхдээ IF хязгаарлалт
хэрэгтэй хэвээр бөгөөд ~ нь дансны үлдэгдэл,
өдрийн лимит, валютын хөрвүүлэлтийн
логикийг автоматаар шалгахгүй.

Нотолгоо:
- results/transfer-nc.txt
- results/transfer.txt
- results/transfer-neg.txt

### B2. PICT гаралтаас боловсруулсан бодит тест кейсүүд

Дараах тестүүдийг results/transfer-neg.txt файлын
эхний гурван мөрөөс сонгосон.

#### TC-P01 — Валют хөрвүүлэлттэй амжилттай шилжүүлэг

PICT: Valid, Valid, Normal, Different, Available, Sufficient

Setup:
- ACC001: хүчинтэй, царцаагдаагүй MNT данс.
- ACC001 үлдэгдэл: 1,000,000 MNT.
- Өдрийн үлдсэн лимит: 500,000 MNT.
- ACC002: хүчинтэй, ACC001-ээс өөр данс.
- Тестийн ханш: 1 USD = 3,500 MNT.

Input:

    POST /transfer
    {"from":"ACC001","to":"ACC002","amount":100,"currency":"USD"}

Oracle:
- Хөрвүүлсэн дүн: 350,000 MNT.
- HTTP status: 201.
- Response: {"result":"OK"}.

#### TC-P02 — Ижил валютаар амжилттай шилжүүлэг

PICT: Valid, Valid, Normal, Same, Available, Sufficient

Setup:
- ACC001: хүчинтэй MNT данс.
- Үлдэгдэл: 1,000,000 MNT.
- Өдрийн үлдсэн лимит: 500,000 MNT.
- ACC002: хүчинтэй, өөр данс.

Input:

    POST /transfer
    {"from":"ACC001","to":"ACC002","amount":50000,"currency":"MNT"}

Oracle:
- HTTP status: 201.
- Response: {"result":"OK"}.

#### TC-P03 — Өдрийн лимит хэтэрсэн

PICT: Valid, Valid, ~OverLimit, Different, NearlyUsed, Sufficient

Setup:
- ACC001: хүчинтэй MNT данс.
- Үлдэгдэл: 1,000,000 MNT.
- Өдрийн үлдсэн лимит: 10,000 MNT.
- ACC002: хүчинтэй, өөр данс.
- Тестийн ханш: 1 USD = 3,500 MNT.

Input:

    POST /transfer
    {"from":"ACC001","to":"ACC002","amount":100,"currency":"USD"}

Oracle:
- Хөрвүүлсэн дүн: 350,000 MNT.
- Үлдэгдэл хүрэлцээтэй боловч лимит хэтэрсэн.
- HTTP status: 200.
- Response: {"result":"ERROR_LIMIT"}.

Эдгээр нь API код ажиллуулсан үр дүн биш,
PICT гаралтад үндэслэн зохиосон тестийн
хүлээгдэх үр дүн юм.

### B3. Дүгнэлт

Энэ лабораторийн ажлаар Category-Partition болон Microsoft PICT аргыг ашиглан банкны шилжүүлгийн тестүүдийг боловсрууллаа.

1. Category-Partition арга нь оролтын утгуудыг ангилж, алдаатай болон зөв нөхцөлүүдийг тодорхойлоход тохиромжтой байв.
2. Гараар тест зохиох үед хүлээгдэх үр дүн болон алдааны давуу эрэмбийг нарийн тодорхойлох боломжтой байсан.
3. Харин олон параметртэй системд бүх хослолыг гараар боловсруулах нь цаг хугацаа шаарддаг.
4. PICT нь параметрүүдийн pairwise хослолыг автоматаар хамарсан тестүүдийг үүсгэж чадсан.
5. Browser моделийн 144 боломжит хослолыг 2-way аргаар 10, 3-way аргаар 20 тест болгон бууруулсан.
6. Transfer моделийн хязгаарлалтгүй хувилбар 16, хязгаарлалттай хувилбар 18 тест үүсгэсэн.
7. Negative тэмдэглэгээ ашигласнаар олон алдаа зэрэг орсон мөрүүдийг арилгаж, error masking асуудлыг багасгасан.
8. Хамгийн их бодол шаардсан хэсэг нь Amount, Balance, DailyLimit-ийн хоорондын IF хязгаарлалтуудыг зөв тодорхойлох явдал байв.
9. Иймээс Category-Partition нь тестийн утга болон хүлээгдэх үр дүнг тодорхойлоход, PICT нь хосын хамралтыг хангахад илүү тохиромжтой бөгөөд хоёр аргыг хамтад нь ашиглах нь үр дүнтэй гэж дүгнэв.
