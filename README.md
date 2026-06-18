# git-hub-2
Loyiha talablaridan kelib chiqib, Capstone loyihangizni mukammal darajada yakunlash va uni topshirishga tayyorlash uchun barcha qadamlar va hisobot shablonlari jamlangan yakuniy qo'llanma.1. Repo va Dokumentatsiya (Repository Setup)Repozitoriyangiz professional ko'rinishi uchun quyidagi tuzilmaga ega bo'lishi kerak:Plaintext├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
├── .gitignore
├── LICENSE
├── README.md
├── CONTRIBUTING.md
└── [Sizning loyiha kodlaringiz]
📄 README.md uchun status badge va live demo qismi:Markdown# Loyiha Nomi 🚀
[![Run Tests](https://github.com/USER_NAME/REPO_NAME/actions/workflows/test.yml/badge.svg)](https://github.com/USER_NAME/REPO_NAME/actions)

Loyiha haqida qisqacha ma'lumot (1-2 gap).

🔗 **Live Demo:** [https://loyiha-nomi.vercel.app](https://loyiha-nomi.vercel.app)
2. Issue va Project ManagementGitHub interfeysida quyidagilarni sozlang:Labels: Issue'larga rangli teglar bering (bug, feature, documentation, ci/cd).Milestone: v1.0.0 nomli milestone yarating va unga barcha 10+ issue'larni biriktiring (Deadline belgilash esdan chiqmasin).Project Board: GitHub Projects qismida Automated Kanban (Todo, In Progress, Review, Done) tizmida ishni tashkil qiling.Assignees: Har bir issue jamoaning ma'lum bir a'zosiga biriktirilgan bo'lishi shart.3. Branching, PR va Code Review🟢 Branch Protection Rules:Settings -> Branches -> Add branch protection rule qismiga kiring:Branch name pattern: mainRequire a pull request before merging $\rightarrow$ YoqishRequire status checks to pass before merging $\rightarrow$ Yoqish (Actions testlari o'tishi shart bo'lishi uchun).✍️ Conventional Commits Standarti:Commit xabarlarini quyidagi formatda yozing:feat(auth): login tizimi qo'shildifix(api): ulanish xatoligi to'g'rilandidocs(readme): status badge qo'shildi🔗 Issue bog'lash:PR ochganda uning tavsifiga (Description) quyidagilardan birini yozing:This PR closes #12 (Bu merge bo'lganda 12-sonli issue avtomat yopiladi).4. CI/CD KonfiguratsiyasiLint, build va testlarni tekshirish uchun .github/workflows/test.yml fayli namunasi (Node.js misolida):YAMLname: CI Quality Check

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  quality-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Run Linter
        run: npm run lint # yoki tegishli lint buyrug'i

      - name: Run Build
        run: npm run build

      - name: Run Tests
        run: npm test
5. Yakuniy Topshirish: Retrospective Hisobot ShablonLoyihani topshirishda o'qituvchi yoki baholovchilarga taqdim etiladigan Retrospective Report hujjati formati:Markdown# PROJECT RETROSPECTIVE REPORT (V1.0.0)

## 1. Umumiy Ma'lumotlar
* **GitHub Repository:** [LINK]
* **Live Demo URL:** [LINK]
* **Release Link:** [LINK]

## 2. Jamoa va Hissa (Contributions)
* **Dasturchi 1 (Team Lead):** [Ism] — [Vazifasi: masalan, CI/CD va Auth] — Commitlar soni: X
* **Dasturchi 2:** [Ism] — [Vazifasi: Frontend logikasi] — Commitlar soni: Y
* **Dasturchi 3:** [Ism] — [Vazifasi: API va Database] — Commitlar soni: Z
*(GitHub Insights grafiklariga ko'ra balans saqlangan).*

## 3. Git Konfliktlari va Ularning Yechimi
* **Konflikt senariysi:** `feature/auth` va `feature/api` branchlari birlashtirilayotganda `server.js` faylida konflikt yuzaga keldi.
* **Qanday hal qilindi:** Jamoa bilan birgalikda lokal kompyuterda `git rebase main` qilindi, har ikki tomonning kodi saqlangan holda konflikt bloklari tozalandi va `git rebase --continue` orqali muvaffaqiyatli hal qilindi.

## 4. Nimalarni o'rgandik? (Lessons Learned)
1. Pull Request ochishdan oldin local branch'ni doim `main`ga rebase qilish merge konfliktlarni ancha kamaytirishini bildik.
2. `git push --force-with-lease` kaliti jamoadoshlarimiz kodi serverda o'chib ketishidan qanchalik muhim himoya ekanini amalda ko'rdik.
3. GitHub Actions workflow sintaksisidagi mayda indentatsiya (space) xatolari poyplaynni buzishini va YAML linter ishlatish kerakligini tushundik.

## 5. Kelajakdagi rejalar (Next Steps)
* v1.1.0 talqinida avtomatik Docker image build qilish va unit testlar qamrovini (coverage) 90% ga yetkazish.
Ushbu shablon va ko'rsatmalar asosida loyihangizni 100% talablarga mos holatda yakunlashingiz mumkin. Kursni va Capstone loyihasini muvaffaqiyatli topshirishingizga tilakdoshman! Yangi ochilgan repozitoriyangiz yoki poyplaynlar bo'yicha yana biron bir texnik savol bormi?
