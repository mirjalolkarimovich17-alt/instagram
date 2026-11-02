---
name: "xabash-reel-maker"
description: "Tasdiqlangan mavzudan xabash.jonkam uchun to'liq o'zbekcha Reels yasash: ilmoq, uslub, musiqa, imo-ishora va QA agentlari bilan ssenariy, Google Flow rasmlari, ElevenLabs ovozi va render."
---

# Mavzudan tayyor Reels videoga (video yig'uvchi)

Repo: `safarovman/xabash.reels` (klon: `/home/claude/xabash.reels`; yo'q bo'lsa clone, bor bo'lsa `git pull`). Dvigatel: `reel/engine.py`, format: `SCHEMA.md` (avval o'qing), to'liq tartib: `skills/WORKFLOW.md` (0d, 0c, 0a, 0, 0b — eng ustun qoidalar), agentlar: `agents/README.md` va `agents/*.md`, grafiklar: `analytics/*.md`. Sozlamalar: `config.json` (`slots`, `style`, `video_rules`, `elevenlabs`). Band slotlar: `SCHEDULE_NOTE.md`. Brend: 99 FOIZ (1% / BIR FOIZ va boshqa sahifalar logotipi ishlatilmaydi).

Har video papkasi: `jobs/<YYYY-MM-DD>_<HHMM>_<slug>/` — `scenes.json`, `img/` (sahnalar `NN.jpg`, qo'shimcha kadrlar `NNb.jpg`/`NNc.jpg`, ixtiyoriy `NN_b.jpg`, personajlar `c_<nom>.jpg`, buyumlar `p_<nom>.jpg`, imo-ishoralar `p_g_<nom>.jpg`, muqova `cover.jpg`), `aud/vo_NN.mp3`, agent brieflari `brief_hooks.json`, `brief_style.json`, `brief_music.json`, `brief_gestures.json`, `qa_report.json`, `post.json`.

Egasi tasdiqlagan namuna: `media/test/tez_sinov_2026-10-16.mp4` — hamma videolar shu tezlik, kadrlar soni, belgilar va o'tishlar darajasida.

## 0. Agentlar (har video uchun)
1. Ssenariy qoralamasi tayyor bo'lgach AVVAL **ilmoq agenti** (`agents/hook_director.md`) → ssenariyni tahrirlaydi + `brief_hooks.json` (ovozga berishdan oldin).
2. So'ng **Agent tool** bilan uchta agentni bir vaqtda ishga tushiring (general-purpose; prompt: "Repo /home/claude/xabash.reels. agents/<fayl>.md ni o'qib amal qil. Video: <slot, mavzu, sahnalar, job papka>"):
   - `agents/style_director.md` → mavzuga mos RASM USLUBI (oxirgi 5 videoda yo'q), shrift, palitra, o'tishlar va MUQOVA (`cover.layout`, oxirgi 5 videoda takrorlanmasin) → `brief_style.json`
   - `agents/music_director.md` → mavzuga mos janr, bpm, tonallik, kayfiyatlar (oxirgi 3 videoda yo'q) → `brief_music.json`
   - `agents/gesture_director.md` → 15–25 ta imo-ishora: `mark` belgilari va Flow'dan `p_g_*.jpg` stikerlari → `brief_gestures.json` (tarix videolarida ham majburiy)
3. Siz brieflarni birlashtirib `scenes.json` yozasiz, rasm, ovoz va renderni qilasiz.
4. Render'dan keyin **tekshiruvchi agent** (`agents/qa_inspector.md`) → `qa_report.json`.
Tarix slotlari (10:30, 14:30): `agents/history_scout.md` qoidalari ham majburiy. Mavzu agenti (`agents/topic_scout.md`) = `xabash-topics` skill.

## 1. Ssenariy (kamida 70 s, uzunlik mavzuga qarab) — HAR SAFAR BOSHQACHA
Bitta shablonni takrorlamang. `topics_log.md` dan oxirgi videolarning qahramoni va ochilish turini o'qing va farqli tanlang:
- **Yangi bosh qahramon**: jinsi, yoshi, kasbi, kiyimi boshqa; ism bering.
- **Yangi ochilish turi** (navbat bilan): qiziqish bo'shlig'i / afsonani buzish / cold open / tomoshabinga murojaat / raqamli va'da / tanish holat / ovozsiz vizual zarba. Oddiy dialog yoki sekin "Tasavvur qiling" bilan BOSHLANMAYDI.
- **Stop-time** (`analytics/retention_2026-10-17.md`): birinchi 3 soniyada savol yoki va'da + eng g'alati kadr; bitta ovoz bo'lagi ≤ 6 s; ~20 va ~30 soniyada kuchli qayta ilmoq; har 8–12 soniyada re-hook.
- **JAVOB OXIRIDA**: ilmoqda va'da qilingan javob / asosiy fakt videoning oxirgi 15–20% qismida beriladi, o'rtada emas.
- **Yakuniy savol** izohga chorlaydigan ochiq savol (oxirgi 5 videoda takrorlanmasin); "1 — ha, 2 — yo'q" / "1·2·3" ovoz berish ISHLATILMAYDI. Caption'dagi savol ham shunday.
- **2–3 personaj gapiradi**, har biri boshqa ElevenLabs ovozida (4-bo'lim kastingi).
- **Ekrandagi yozuvlar to'liq, tugallangan gap**, KATTA HARFDA.
- Doim: WebSearch bilan tekshirilgan 2–3 fakt (`card`, nom + muallif/yil). Tarixda 3–4 ishonchli manba, afsona "rivoyat qilishlaricha".
- Bir xil raqam yoki motivni videodan-videoga takrorlamang.
- Raqamlar ovozda so'z bilan, o'zbek lotin. Shriftda yo'q belgilar (→, ≠) ishlatmang.
- `scenes.json` da `cover_title` (2 qator) va `cover_sub` majburiy.
- `topics_log.md` ga qator: qahramon, ochilish turi, dizayn va `uslub · shrift · musiqa · muqova · ilmoq · savol`.

## 2. Tezlik va dizayn — har videoda boshqacha
- `"pace": "fast"` — har 1.5–2 soniyada ekranda yangi narsa. O'tishlar `theme.transitions` (whip/zoom/flash/slide/torn), sahna ichidagi kesish `theme.cut_fx` (punch/flash/whip).
- **Rasmlar ko'p: 20–30 ta Flow rasmi.** Har sahnada 2–3 kadr: `"imgs": ["05b.jpg", ["05c.jpg", "ibora"], {"img": "05d.jpg", "fx": "flash"}]`.
- Har mavzuga O'Z dizayningiz: `theme` `brief_style.json` dan; shriftlar `reel/fonts` dagi fayllar (kichik harf, "-Regular"siz). Reference videolarni nusxalamang.
- Butun videoga bitta umumiy fon YO'Q: har sahna foni mavzuga mos Flow rasmi; protsedurali `space`/`blueprint` fon faqat ataylab 1–2 sahnada.
- Bosh qahramon PNG stiker bo'lib 2–3 sahnada har xil reaksiya bilan chiqadi (`char.moods`).
- **PNG stikerlar**: `char` (`from`, `talk: ["vo"]`, `out`, `motion`) va `prop` (`label` bilan). Harakat: `point`, `pulse`, `jump`, `wag`, `bounce`, `shake` — sekin `spin`/`float` YO'Q. Asosiy sahna rasmidagi qahramonni kesib qimirlatish YO'Q.
- **Imo-ishoralar 15–25 ta**: `mark` va `p_g_*.jpg`; yuz va pufakchalarni yopmasin. Kutubxona: `assets/gestures/`.
- **Dialog**: `bubble`. **Infografika**: `tag`, `bars`, `avatar`.
- Katta qizil `stamp`/`big` — videoda ko'pi bilan 1 marta.
- **Muqova** har videoda boshqacha: alohida Flow rasmi `img/cover.jpg` + `scenes.json → cover` (layout: hero/split/bubble/polaroid/diagonal/zoom/poster/classic).

## 3. Rasmlar — Google Flow (egasining kompyuteridagi ichki brauzer)
- Uslub `brief_style.json` dagi `image_prefix` / `character_prefix` / `prop_prefix` (config'dagi `style.*` faqat zaxira). Bitta video ichida bitta uslub. Odamlar tabiiy proporsiyada; chibi/katta ko'zli multfilm yo'q; haqiqiy siyosatchi/mashhurlar chizilmaydi; rasmda yozuv yo'q.
- `built-in-browser` skill'ini o'qing. Loyiha: flow.google.com (Image, 9:16, x1, Nano Banana). Kirish kerak bo'lsa — egasidan so'rang, parol hech qachon kiritilmaydi. Higgsfield ISHLATILMAYDI.
- Personaj stikerlari oq fonda → `img/c_<nom>.jpg`; buyumlar → `img/p_<nom>.jpg`; imo-ishoralar → `img/p_g_<nom>.jpg` (dvigatel rembg bilan kesadi). Bir personajning hamma rasmlarida birinchi rasmni namuna qiling: "Same person as in the reference image (...)".
- Ingredient qo'shish: grid'dagi tile → "Дополнительные параметры" → "Добавить в запрос". So'ng ProseMirror maydoniga prompt yozing va yuborish koordinata (558,575) bilan (JS .click ishlamaydi). browser_batch max 25 amal. «Ошибка» — refresh tugmasi.
- Yuz/asosiy obyekt markazda, pastki 1/3 va yuqori 1/4 yozuv va pufakchalar uchun tinchroq.
- **2-kadr**: asl rasm tile'ini "Добавить в запрос" qilib, "Edit the reference image: keep exactly the same drawing style, characters, clothes, background, colors, camera angle and composition. Only one small change: ..." → `img/NN_b.jpg`.
- Yuklab olish (to'liq o'lcham): tile `img.src` ni fetch → yakuniy URL dagi `=s512-rw` ni `=s0` ga almashtirib fetch; hammasini BITTA paket faylga (4 bayt uzunlik + JSON indeks + baytlar) yig'ib `<a download>` → Downloads → `device_list_dir` → `device_stage_files` → konteynerda bo'ling. Moslashni rasm o'xshashligi bilan qiling (Hungarian assignment), kontakt varaq bilan tekshiring.

## 4. Ovoz — ElevenLabs (jonli, hissiyotli, o'zbekcha)
- Flow `config.json → elevenlabs.flow_id` (`qGgjoCYEgz4zhvjl3GCQ`); eski `g89LrH9AMPN93hZwIUYS` / `vrOkazhVp30VTw8bn1YR` ishlatilmaydi. Ochilmasa `creative_create_flow` bilan yangisini oching va config'ga yozing. `eleven_v4`, `generations_count: 1`. 429 bo'lsa ketma-ket, bittadan.
- **Kasting** (`config.json` → `elevenlabs.cast`):
  - Hikoyachi — DOIM Bekzod `kGOig3GPePLxobJGHo2M`
  - Yosh qiz — Dilnoza `O72h9AUwisM6Zj4He72B` yoki Zilola `cyPun3bl9MxcpLBX6pT0`
  - Ona / opa — Nodira `132QLQIkg1RJGmpicuhR` yoki Shahnoza `eggZu1Ffe4if51TKev2v`
  - Ota / bobo — Jahongir `xDwfBjUEPdIoQekNOXAX` (+ `[elderly voice]` bobo uchun)
  - Yigit / oshna — Sardor `UjuEL3hLLUaDED9iW4Yb`
  - Buvi — Malika `VTN9ql0D5MshokKncKAy` (+ `[elderly voice]`), zaxira Gulnora `6Fkh9WgMXOqBcOWxX91f`
- INGLIZ ovozlari (Sarah, Charlie, Bill, Lily, Jessica — `banned_voices`) TAQIQLANGAN.
- Har qatorda 1–2 hissiyot tegi: `[excited] [laughs] [chuckles] [sighs] [sad] [angry] [surprised] [whispers] [warmly] [sarcastic] [pause]`. Tabiiy jonli so'zlashuv, kitobiy til emas.
- Ro'yxatda yo'q ovoz kerak bo'lsa: `creative_list_voices` dan o'zbekcha ovoz tanlab bitta qisqa qator bilan sinang, so'ng `cast` ga yozing.
- Dvigatel har ovoz bo'lagini -16 LUFS ga tenglaydi (`vo_lufs`).
- "Free Tier disabled" yoki kredit tugashi: jumlani biroz boshqacha yozib bir marta urinib ko'ring; yana chiqsa egasiga DARHOL ayting.
- Yuklab olish: konteyner storage.googleapis.com ga chiqa olmaydi. URL ro'yxatini vaqtincha `media` tarmog'iga `tmp/vo_urls.json` qilib push qiling; ichki brauzerda storage.googleapis.com sahifasida JS bilan hammasini fetch qilib BITTA paket fayl qilib download (havolani `document.createElementNS('http://www.w3.org/1999/xhtml','a')` bilan yarating) → stage → konteynerda bo'lib `aud/vo_NN.mp3`; keyin `tmp/` ni o'chiring. URL'lar 2 soat amal qiladi.

## 5. Musiqa
`scenes.json → "music": {"genre", "bpm", "key", "scale", "progression"}` va yangi `music_seed` (`brief_music.json` dan; janrlar `reel/music.py` GENRES: doira_pop, doira_folk, lapar, chiptune, synthwave, space, lofi, jazz_swing, detective, funk, disco, polka, western, tango, toy, epic; oxirgi 3 videoda takrorlanmasin). Sahna `mood` lari har 2–3 sahnada o'zgaradi. Egasi bergan trek: `"music": {"file": "music.mp3"}`.

## 6. Render
- `pip install --break-system-packages pyfluidsynth opencv-python-headless pillow numpy scipy "rembg[cpu]"`; soundfont: `apt-get install -y fluid-soundfont-gm`.
- `python3 reel/engine.py jobs/<job> --auto-preview` → `out/preview/sheet.jpg` ni ko'ring.
- To'liq render: `nohup python3 reel/engine.py jobs/<job> &` (~25–30 daq, 2 tasini parallel ishlatmang). `until grep -q done <log>; do sleep 10; done` bilan kuting. `pkill -f engine` ishlatmang — `pgrep -x python3`. Natija: `out/video.mp4` (max sifat, <95 MB), `out/cover.jpg`, `out/timing.json` (`cover_ms`).
- Logo `brand/logo_99foiz.png` har kadrda avtomatik.

## 7. QA — tekshiruvchi agent (`agents/qa_inspector.md`)
- (1) audio transkripsiya (ElevenLabs `creative_attach_reference_file` + `creative_transcribe_audio`) va har sahnani `scenes.json` matni bilan solishtirish (<90% = xato); (2) `python3 reel/engine.py jobs/<job> --qa` varaqlarida rasm va imo-ishoralar vaqti, yuz yopilishi, yozuvlar; (3) javob oxirida, ovoz bo'laklari ≤ 6.5 s, ≥ 70 s.
- XATO TOPILSA VIDEONI TASHLAMANG — TUZATING: tekshiruvchi vaqt, sahna va tuzatish ko'rsatmasini beradi; faqat o'sha joyni tuzatib qayta render; yaxshi joylarga tegmang; ko'pi bilan 2 marta, keyin video qo'yiladi. Faqat jiddiy xato tuzatilmay qolsa — o'sha video qo'yilmaydi va egasiga darhol aytiladi.

## 8. Caption
`jobs/<job>/post.json`: **qisqa** — 1 ilmoq jumla + 1 ochiq savol + 6–8 heshteg (#99foiz #ozbekcha #uzbekistan #reels + mavzuga oid). Trial reel yo'q: Buffer'da `schedulingType: automatic`, `shouldShareToFeed: true`.

Oxirida egasiga kontakt varaq yoki videoni `SendUserFile` bilan ko'rsating.