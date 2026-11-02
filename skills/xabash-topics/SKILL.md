---
name: "xabash-topics"
description: "Har kuni kechqurun xabash.jonkam Instagram sahifasi uchun ertangi 7 ta Reels mavzusini (08:00, 10:30, 12:30, 14:30, 16:00, 18:30, 21:00) o'zbekona hayot va tarixdan topib, egasiga taklif qilish."
---

# Ertangi 7 ta Reels mavzusini taklif qilish (mavzu agenti + tarix agenti)

Sahifa: Instagram **xabash.jonkam** (brend 99 FOIZ, Buffer orqali). Til: o'zbekcha (lotin). Format: hajviy-ijtimoiy va ma'rifiy Reels, kamida 70 soniya (aniq uzunlikni mavzuga qarab tanlang), 2–3 personaj gapiradi, har safar yangi qahramon, yangi ochilish va mavzuga mos o'z dizayni. Egasi tasdiqlagan namuna: `media/test/tez_sinov_2026-10-16.mp4` (tez, rasmlari ko'p).

Ma'lumotlar repo'da: `safarovman/xabash.reels` (`config.json`, `topics_log.md`, `SCHEDULE_NOTE.md`, `agents/`, `jobs/`). Ish boshida repo'ni oling: `add_repo` (owner safarovman, repo xabash.reels, access push) → `git clone --depth 1 https://github.com/safarovman/xabash.reels` (bor bo'lsa `git pull`). Agent ko'rsatmalari: `agents/topic_scout.md`, `agents/history_scout.md`, `agents/hook_director.md`.

**Egasi mavzuni o'zi bergan bo'lishi mumkin**: avval `topics_log.md`, `jobs/` va Buffer `list_posts` ni tekshiring — ertangi kun uchun mavzu tasdiqlangan yoki video yasalayotgan bo'lsa, yangi taklif qilmang va dublikat yasamang; faqat yetishmayotgan slotlarni to'ldiring.

**Trial reel qilinmaydi.** Hamma videolar oddiy Reels: Buffer'da `schedulingType: automatic`, `shouldShareToFeed: true` — o'zi profilga chiqadi.

## Qadamlar
1. `topics_log.md` ni o'qing — oxirgi 30 kunda ishlatilgan mavzu, qahramon, ochilish turi, dizayn va tarixiy voqealarni takrorlamang. `SCHEDULE_NOTE.md` va Buffer `list_posts` bilan band slotlarni tekshiring.
2. `config.json` dagi `slots` bo'yicha har bir vaqt uchun 1 ta mavzu tanlang (Asia/Tashkent):
   - **08:00 o'tmish**: ota-bobolar zamoni, eski Toshkent, «bizning vaqtimizda» — hozirgi bilan hazil taqqoslash
   - **10:30 jahon tarixi**: imperiyalar, janglar, kashfiyotlar, ixtirolar, g'alati tarixiy voqealar (`agents/history_scout.md`)
   - **12:30 hozirgi zamon**: internet, telefon, o'yinlar, ish, pul, onlayn xarid
   - **14:30 O'zbekiston tarixi**: Amir Temur va temuriylar, Somoniylar, Xorazmshohlar, Jaloliddin, Ulug'bek, Bobur, xonliklar, jadidlar, buyuk olimlar — faqat Amir Temur emas (`agents/history_scout.md`)
   - **16:00 kelajak**: koinot, AI, 2050-yil — fantastik hazil + haqiqiy ilmiy fakt
   - **18:30 urf-odatlar**: to'y, mehmondorchilik, choy, qo'shnichilik — mehr bilan, masxarasiz
   - **21:00 odamlardagi ko'p xatolar**: kechiktirish, telefon, parol, pul, sog'liq — hazil + foydali xulosa
3. Hajviy mavzular o'zbekona bo'lsin (oila, mahalla, to'y, osh, non, choy, qaynona, kredit, marshrutka, bozor, Telegram guruhlari, "ertadan", mehmondo'stlik va h.k.), bitta tanish odat yoki paradoks atrofida, haqoratsiz, siyosatsiz, din va millatni masxara qilmasdan. Har biriga 2–3 personaj (masalan dadasi: «Ex bolam, mening vaqtimda...») va mavzuga mos o'zingiz o'ylab topgan dizayn g'oyasi.
4. Tarix mavzulari bitta aniq voqea yoki savol atrofida ("Jaloliddin Sind daryosidan qanday o'tdi?"); yurish/jang xarita + strelkalar va 2-3 kadrli sahnalar bilan tasvirlanadi; afsona "rivoyat qilishlaricha" deb ajratiladi; payg'ambarlar va hozirgi siyosatchilar tasvirlanmaydi.
5. Har bir mavzu uchun tekshiriladigan fakt yoki tadqiqot toping (WebSearch; tarix uchun 3–4 ta ishonchli manba). Fakt topilmasa, mavzuni almashtiring — uydirma raqam ishlatmang.
6. Har mavzuga to'xtatuvchi ilmoq (birinchi 1.5 soniya, savol yoki va'da) va izohga chorlovchi ochiq yakuniy savol o'ylang — "1 — ha, 2 — yo'q" / "1·2·3" kabi ovoz berish YO'Q (`agents/hook_director.md`).
7. Egasiga **SendUserMessage** bilan shunday formatda yuboring:

```
Ertangi videolar uchun 7 ta mavzu:

📜 08:00 (o'tmish) — <Nom>
   Ilmoq: "<birinchi jumla>"
   Personajlar: <kim> · Dizayn: <g'oya> · Musiqa: <janr> · Fakt: <manba>
🌍 10:30 (jahon tarixi) — ...
📱 12:30 (hozir) — ...
🏰 14:30 (O'zbekiston tarixi) — ...
🚀 16:00 (kelajak) — ...
🫖 18:30 (urf-odat) — ...
⚠️ 21:00 (xatolar) — ...

Tanlang: "hammasi ok" deng, yoki raqamini aytib almashtiring (masalan "2 ni boshqasiga"), yoki o'z mavzuingizni yozing.
Tasdiqlaganingiz — shu videolarni belgilangan vaqtlarda joylashga ruxsat beradi.
```

8. Har bir vaqt uchun 1 ta zaxira mavzuni o'zingizda saqlang (egasi almashtirishni so'rasa).
9. Egasi tasdiqlagach (yoki 30 daqiqa javob bo'lmasa — taklif qilingan mavzular bilan) `xabash-reel-maker`, so'ng `xabash-publish` skill'iga o'ting. Hamma postlar `schedulingType: automatic`. Buffer bepul rejasi 10 ta rejalashtirilgan post — faqat keyingi 24 soatni to'ldiring.