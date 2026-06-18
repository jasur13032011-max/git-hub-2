# git-hub-2
Git-da versiyalash (tagging), GitHub platformasida relizlar yaratish va GitHub Actions yordamida CI/CD poyplaynlarini (workflows) sozlash bo'yicha to'liq qo'llanma.

1. 2 ta tag (Lightweight va Annotated)
Git-da tag'lar (teglar) loyihaning ma'lum bir commit tarixini (masalan, v1.0.0 relizini) belgilab qo'yish uchun ishlatiladi.

Lightweight (Yengil) tag: Bu shunchaki ma'lum bir commit'ga ko'rsatkich (pointer). Hech qanday qo'shimcha ma'lumot saqlamaydi.

Bash
git tag v1.0.0-lw
Annotated (Izohli) tag: Git bazasida to'liq obyekt sifatida saqlanadi. O'z ichiga muallif ismi, email, sana va maxsus xabarni (message) oladi. Relizlar uchun doim shu tur tavsiya etiladi.

Bash
git tag -a v1.0.0 -m "Birinchi rasmiy barqaror reliz"
2. Tag'larni push qilish (--tags)
Lokalda yaratilgan tag'lar oddiy git push buyrug'i bilan serverga (remote) ketmaydi. Ularni alohida jo'natish kerak:

Bash
# Faqatgina bitta aniq tag'ni push qilish
git push origin v1.0.0

# Lokaldagi barcha yangi tag'larni bir vaqtda push qilish
git push origin --tags
3. GitHub Release sahifa changelog bilan
Tag serverga push qilingandan so'ng, GitHub-da uni to'liq Release holatiga keltirish mumkin:

GitHub repozitoriyangizga kiring va o'ng tarafdagi Releases bo'limiga o'ting.

Draft a new release tugmasini bosing.

Push qilingan tag'ni tanlang (masalan, v1.0.0).

Generate release notes tugmasini bossangiz, GitHub avtomatik ravishda oxirgi relizdan beri qilingan barcha Pull Request'lar va commit'lardan Changelog (o'zgarishlar ro'yxati) tuzib beradi.

Publish release tugmasini bosing.

4. gh release create CLI bilan
Agar GitHub veb-interfeysiga kirmasdan, to'g'ridan-to'g'ri terminal orqali reliz yaratmoqchi bo'lsangiz, GitHub CLI (gh) dasturidan foydalanishingiz mumkin:

Bash
gh release create v1.0.0 --title "Reliz v1.0.0" --notes "Bu relizda yangi API logikasi va xavfsizlik tuzatishlari qo'shildi."
Bu buyruq avtomat ravishda tag yaratadi, uni serverga yuklaydi va GitHub Release sahifasini shakllantiradi.

5. .github/workflows/test.yml — Push/PR'da test + Matrix builds
Ushbu workflow har safar kod main branch'ga push qilinganda yoki har qanday Pull Request ochilganda ishga tushadi. Matrix builds yordamida testlar bir vaqtning o'zida Node.js dasturining 2 ta versiyasida tekshiriladi.

Fayl yo'li: .github/workflows/test.yml

YAML
name: Run Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test-matrix:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18.x, 20.x] # Kamida 2 ta versiya

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: npm ci

      - name: Run unit tests
        run: npm test
6. .github/workflows/deploy.yml — Main'ga merge'da deploy + Secrets
Ushbu workflow faqatgina kod muvaffaqiyatli tekshirilib, main branch'iga birlashganda (merge) ishga tushadi va loyihani serverga yuklaydi (simulyatsiya).

Fayl yo'li: .github/workflows/deploy.yml

YAML
name: Deploy Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Deploying to server
        env:
          # GitHub Settings -> Secrets and variables -> Actions qismida sozlangan maxfiy kalit
          DEPLOY_API_KEY: ${{ secrets.PRODUCTION_API_KEY }} 
        run: |
          echo "Deploy jarayoni boshlandi..."
          # Maxfiy kalit yordamida serverga so'rov yuborish simulyatsiyasi
          curl -H "Authorization: Bearer $DEPLOY_API_KEY" https://api.myserver.com/deploy
          echo "Deploy muvaffaqiyatli yakunlandi!"
7. Secrets sozlash (GitHub Actions uchun)
Kod ichida parollar yoki API kalitlarni ochiq yozish xavfli. Buning uchun GitHub Secrets ishlatiladi:

GitHub repozitoriyangizda Settings menyusiga kiring.

Chap menyudan Secrets and variables → Actions qismini tanlang.

New repository secret tugmasini bosing.

Name: PRODUCTION_API_KEY

Secret: Sizning maxfiy API kalitingiz

Add secret tugmasini bosing. Endi unga workflow ichida ${{ secrets.PRODUCTION_API_KEY }} orqali xavfsiz ulanish mumkin.

8. README'da Status Badge
Workflow muvaffaqiyatli ishlayotganini yoki xato berganini loyihaning bosh sahifasida (README.md) ko'rsatish uchun status badge (nishon) qo'shish mumkin.

README.md faylining eng yuqori qismiga quyidagi kod joylashtiriladi:

Markdown
![Test Status](https://github.com/USER_NAME/REPO_NAME/actions/workflows/test.yml/badge.svg)
(GitHub ushbu rasm o'rniga avtomatik ravishda passing yoki failing degan dinamik grafik nishonni chiqarib beradi).

9. Scheduled workflow misoli (cron)
Agar poyplaynni qandaydir hodisaga bog'lamasdan, maolum bir vaqtda (masalan, har kecha) avtomatik ishga tushirmoqchi bo'lsangiz, schedule (cron) ishlatiladi.

YAML
name: Nightly Backup

on:
  schedule:
    # Har kuni soat 00:00 da (UTC vaqti bilan) avtomat ishga tushadi
    - cron: '0 0 * * *'

jobs:
  backup:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Tizim to'liq zaxira (backup) qilinmoqda..."
10. YAML indentatsiya xato tahlili (2 vs 4 bo'shliq)
YAML formatida qat'iy qoida bor: Tab belgisini ishlatish mutlaqo taqiqlanadi, faqat bo'shliq (space) ishlatilishi shart. Ko'pchilik dasturchilar loyihada 2 ta yoki 4 ta joy tashlash (indentation) borasida xatoga yo'l qo'yishadi.

Muammo tahlili:
YAML sintaksisi daraxtsimon tuzilishga ega. Agar siz bitta fayl ichida adashib bir joyda 2 ta, boshqa joyda 4 ta bo'shliq ishlatsangiz, iyerarxiya buziladi:

YAML
# XATO VARIANT (Yashirin xatolik):
jobs:
  test:
    runs-on: ubuntu-latest
      steps: # XATO: steps bloki o'zidan tepada turgan runs-on bilan bir xil paragrafda bo'la olmaydi (4 ta emas, 2 ta joy ichkarida bo'lishi kerak edi)
      - uses: actions/checkout@v4
To'g'ri yechim:
Butun fayl davomida bir xil standartni tanlang (odatda GitHub Actions uchun 2 ta bo'shliq standarti tavsiya etiladi).

YAML
# TO'G'RI VARIANT:
jobs:
  test:
    runs-on: ubuntu-latest
    steps: # Har bir iyerarxiya aniq 2 tadan bo'shliq bilan surilgan
      - uses: actions/checkout@v4
Maslahat: Bunday xatolarni oldini olish uchun VS Code yoki o'zingiz ishlatadigan matn muharririga YAML lint plaginini o'rnatib oling. U sintaktik xatolarni terminalga yuborishdan oldin qizil chiziq bilan ogohlantiradi.
