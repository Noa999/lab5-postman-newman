# Лаб 5 — API систем тест: Bruno ба Newman

**Нэр:** Ц.Номунзаяа
**Код:** B232270804

## Орчны хувилбарууд

node -v: v22.22.1
bru --version: 4.2.1

## 1. Сонголтууд ба төлөөлөх утгууд

| Сонголт (Choice) | Төлөөлөх утгууд |
|---|---|
| studentID хүчинтэй байдал | идэвхтэй (active) / идэвхгүй (inactive) / байхгүй (бүртгэгдээгүй) |
| Оюутны үзсэн хичээлүүд (coursesTaken) | урьдач нөхцөл бүгдийг хангасан / огт хангаагүй / зарим нь хангасан |
| courseID хүчинтэй байдал | байгаа / байхгүй |
| Хичээлийн урьдач нөхцөл (prerequisites) | хоосон (урьдач нөхцөлгүй) / 1+ урьдач нөхцөлтэй |
| Хүсэлтийн формат | studentID, courseID хоёулаа байгаа / аль нэг нь дутуу / буруу JSON |

## 2. Спецификацийн хүснэгт

| № | Тохиолдол | studentID | courseID | Нөхцөл | Хүлээгдэх статус | Хүлээгдэх result |
|---|---|---|---|---|---|---|
| 1 | Happy path | идэвхтэй, CS201 үзсэн | CS313, prereq=CS201 | бүх шаардлага хангасан | 201 | OK (+registrationID) |
| 2 | Оюутан байхгүй | бүртгэгдээгүй ID | CS313 (байгаа) | — | 200 | ERROR_NO_STUDENT |
| 3 | Идэвхгүй оюутан | status="inactive" | CS313 (байгаа) | — | 200 | ERROR_INACTIVE_STUDENT |
| 4 | Хичээл байхгүй | идэвхтэй | бүртгэгдээгүй courseID | — | 200 | ERROR_NO_COURSE |
| 5 | Урьдач нөхцөл дутуу | идэвхтэй, coursesTaken=[] | CS313, prereq=CS201 | CS201 үзээгүй | 200 | ERROR_PREREQUISITES (missing:["CS201"]) |
| 6 | studentID дутуу | — (талбар дутуу) | CS313 | Body-д studentID байхгүй | 400 | ERROR_BAD_REQUEST |
| 7 | courseID дутуу | идэвхтэй | — (талбар дутуу) | Body-д courseID байхгүй | 400 | ERROR_BAD_REQUEST |
| 8 | Буруу JSON | — | — | Body нь хүчингүй JSON | 400 | ERROR_BAD_JSON |
| 9 | Давхар алдаа: идэвхгүй + байхгүй хичээл | status="inactive" | бүртгэгдээгүй courseID | Аль алдаа түрүүлэх вэ? | 200 | ERROR_INACTIVE_STUDENT (status эрт шалгагддаг) |
| 10 | Хязгаарын тохиолдол: хоосон prerequisites | идэвхтэй, coursesTaken=[] | prereq=[] | Урьдач нөхцөлгүй тул OK | 201 | OK |

## 3. Тестийн тоо

results/newman-pass.txt-ийн дагуу: 16 Requests (16 Passed), 21 Tests (21/21 Passed), exit code 0.

## 4. PASS / FAIL / DOWN ажиллуулалт

**PASS** (lab05-collection/): 16 Requests (16 Passed), 21/21 Tests, exit code 0.
Файл: results/newman-pass.txt

**FAIL** (lab05-collection-fail/): "Register - Happy Path" тестийн oracle-ыг
санаатайгаар 201→200 болгож эвдсэн. Үр дүн: 16 Requests (15 Passed, 1 Failed),
19/20 Tests, exit code 1. k6/CI-ийн adил зарчмаар, threshold эсвэл тест
зөрвөл build/deploy зогсох ёстойг харуулж байна (quality gate).
Файл: results/newman-fail.txt

**DOWN** (сервер унтраасан үед): node server.js процессыг Ctrl+C-ээр зогсоогоод
lab05-collection/-ийг дахин ажиллуулахад бүх 16 хүсэлт "connect ECONNREFUSED
127.0.0.1:3000" алдаатай буцсан. Exit code 1. Файл: results/newman-down.txt

**Ялгаа — интерфейсийн алдаа vs oracle-ийн алдаа:** DOWN тохиолдолд серверт
огт хүрч чадаагүй тул энэ бол сүлжээ/холболтын (interface) алдаа — манай
API-гийн логикт (oracle) ямар ч хамаагүй. Харин FAIL тохиолдолд сервер хариу
өгсөн ч бидний хүлээж байсан утга (oracle) буруу байсан — энэ бол логикийн
алдаа. Хоёр нь өөр шалтгаантай ч аль алинд нь CI pipeline зогсох ёстой
(exit code ≠ 0).

## 5. Дүгнэлт



## 6. Нийтийн API (нэмэлт, Даалгавар 5)


