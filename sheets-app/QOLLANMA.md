# Maxfiy shartnomalar reestri — o'rnatish qo'llanmasi

Jadval **yopiq qoladi**. Dastur ma'lumotni faqat siz Google hisobingizga kirganingizda ko'rsatadi. Hech kim havola orqali jadvalni ko'ra olmaydi.

---

## 1-qadam: Google OAuth Client ID yaratish (bir martalik, ~5 daqiqa)

1. https://console.cloud.google.com ga kiring (jadval egasi hisobi bilan)
2. Yuqorida loyiha tanlash → **New Project** → nom bering (masalan "Maktab reestri") → **Create**
3. Chap menyu → **APIs & Services → Library** → "Google Sheets API" ni qidiring → **Enable**
4. Chap menyu → **APIs & Services → OAuth consent screen**
   - User Type: **External** → Create
   - App name, support email to'ldiring → Save and Continue
   - Scopes: o'tkazib yuboring → Save
   - **Test users → ADD USERS** → o'zingiz kiradigan email(lar)ni qo'shing → Save
   - (Eslatma: "Testing" rejimida faqat shu test userlar kira oladi — bu yetarli)
5. Chap menyu → **APIs & Services → Credentials**
   - **+ CREATE CREDENTIALS → OAuth client ID**
   - Application type: **Web application**
   - **Authorized JavaScript origins → ADD URI**, bu yerga dastur manzilini qo'shing:
     - Test uchun: `http://localhost:8000`
     - Vercel uchun: `https://SIZNING-DASTUR.vercel.app` (deploy qilgandan keyin aniq manzilni qo'shing)
   - **Create**
6. Chiqqan **Client ID** ni nusxalang (`...apps.googleusercontent.com` bilan tugaydi)

## 2-qadam: Client ID ni dasturga qo'yish

`index.html` ni oching, yuqoridagi qatorni toping va o'zgartiring:

```js
const CLIENT_ID = 'BU_YERGA_CLIENT_ID_QOYING.apps.googleusercontent.com';
```
↓
```js
const CLIENT_ID = '1234567890-abcdef.apps.googleusercontent.com';  // o'z Client ID ingiz
```

## 3-qadam: Vercel'ga yuklash

1. https://vercel.com/new ga kiring (bepul)
2. `sheets-app` papkasini brauzerga sudrab tashlang → **Deploy**
3. Olgan manzilingizni (`https://...vercel.app`) **1-qadam → 5-bosqichdagi Authorized JavaScript origins** ga qo'shishni unutmang, aks holda kirish ishlamaydi
4. Manzilni oching → **Google bilan kirish** → tayyor

---

## Lokal test (ixtiyoriy)
```bash
cd sheets-app
python3 -m http.server 8000
# brauzerda http://localhost:8000 ni oching
```
(localhost ham origins ga qo'shilgan bo'lishi kerak)

## Tez-tez beriladigan savollar

**"redirect_uri_mismatch" yoki kirmayapti** → Authorized JavaScript origins'dagi manzil dastur manzili bilan **aniq** bir xil emas. https/http, oxiridagi `/` ham muhim. To'g'rilang, 1-2 daqiqa kuting.

**"access_denied"** → email OAuth consent screen'dagi Test users ro'yxatida emas. Qo'shing.

**Boshqalar ham kirsin** → ularning emailini Test users ga qo'shing, VA jadvalda Share orqali ularga Viewer ruxsatini bering.
