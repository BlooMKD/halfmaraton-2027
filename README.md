# Halfmaraton 2027 — Android PWA + Google Sheets

Фајлови: `index.html`, `manifest.webmanifest`, `sw.js`, две икони, `Code.gs`.

## 1. Постави Google Apps Script (backend)
1. Отвори го постоечкиот Google Sheet → **Extensions → Apps Script**.
2. Залепи ја содржината на `Code.gs` во `Code.gs` и зачувај.
3. Отвори **Project Settings → Script Properties → Add script property** и постави `API_TOKEN` со твој случајно генериран клуч од најмалку 24 знаци. Чувај го приватно.
4. Постави project time zone во **Europe/Skopje**. Во самиот Google Sheet, по можност избери **File → Settings → Time zone → (GMT+01:00) Skopje** (Sheets сега е поставен на America/Los_Angeles).
5. **Deploy → New deployment → Web app**. `Execute as: Me`; `Who has access: Anyone`. Авторизирај пристап до Google Sheets. Копирај го Web App URL кој завршува на `/exec`.
   **Безбедност:** Ова е јавна техничка адреса за пристап, но API_TOKEN го штити читањето/пишувањето. Поради JSONP, клучот се пренесува во URL за читање и може да се појави во browser/network logs; не користи лозинка од сметка и не споделувај URL со token. Оваа варијанта е за личен дневник, не за чувствителни податоци.
6. За промени во `Code.gs` избери **Deploy → Manage deployments → Edit → New version**.

## 2. Постави PWA (frontend)
1. На GitHub креирај repository, на пример `halfmaraton-2027` (за GitHub Pages на Free, repository обично е public: НЕ ставај го клучот во кодот!).
2. Upload `index.html`, `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png` во root. **Не ставај `Code.gs` во јавен репозиториум ако подоцна додадеш тајни во него.** Во доставениот `Code.gs` нема API_TOKEN.
3. Repository Settings → Pages → Deploy from branch → `main` / `(root)` → Save.
4. Отвори ја објавената `https://<username>.github.io/halfmaraton-2027/` адреса во Chrome на Android.
5. Во **Поставки** на апликацијата внеси Apps Script `/exec` URL и својот API_TOKEN, кликни „Зачувај и синхронизирај“.
6. Chrome ⋮ → **Add to Home screen** → Install / Add. Сега апликацијата има икона на телефонот.

## 3. Користење
- Преглед: километри, тежина, чекори, графикон, напредок до 21.1 km.
- Внес: сите полиња од „Дневник“. Датум кој веќе постои се ажурира, не се дуплира.
- Историја: избери „Измени“ за повторен внес.
- Бројките во dashboard се само за записите со вистински внесени податоци.
- Зачувувањето бара интернет; offline се отвора shell-от на апликацијата, но **нема offline queue**.

## Ограничувања / тестирање
- Кодот е подготвен за поставување, но **не е објавен ниту е тестирана вистинска синхронизација со Google** од оваа средина.
- Google Apps Script нема стандарден CORS API за ваков статички frontend. Читањето користи JSONP, пишувањето `no-cors` POST, а успехот го потврдува со повторно читање од Sheet. При проблем, прво провери го Sheet пред повторен клик.
- Sheet „Дневник“: заглавија во ред 4, датуми од ред 5; колоните L, M, P се формулски и не се препишуваат.
- Записот од 28.10.2026 во постоечкиот Sheet има тежина 55 kg. Тој е оставен непроменет и може да се коригира од апликацијата.
- Важно: ако во Google Sheet има постоечки дупликати за истиот датум, апликацијата го ажурира првиот пронајден датум; не ги брише дупликатите.


## Верзија 2
Новиот Code.gs филтрира вистински внесови, а repairDateOrder() може да ги подреди претходно додадените редови. Направи копија од Sheet-от пред извршување repairDateOrder(). Зачувај и објави New version на постојниот Apps Script deployment. На GitHub замени index.html и sw.js.
