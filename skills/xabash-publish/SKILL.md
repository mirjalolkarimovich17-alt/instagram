---
name: "xabash-publish"
description: "Tayyor va QA'dan o'tgan xabash.reels videolarini GitHub media tarmog'iga yuklab, Buffer orqali xabash.jonkam Instagram'iga Reels sifatida aniq vaqtga rejalashtirish (faqat egasi tasdiqlagan videolar)."
---

# Videoni Instagram'ga rejalashtirish

**Faqat egasi mavzuni/videoni tasdiqlagan bo'lsa joylang** (egasi tasdiqlagan mavzu kunlik joylashga ruxsat hisoblanadi). Tasdiq bo'lmasa — `saveToDraft: true` bilan qoralama qiling va egasiga ayting. Faqat QA'dan (tuzatilib) o'tgan videolar joylanadi (`post.json → status: "QA o'tdi"`).

## 1. Videoni ochiq havolaga joylash (GitHub `media` tarmog'i)
GitHub Releases bu muhitda yopiq — `media` orphan tarmog'idan foydalaning:
```bash
cd /home/claude/xabash.reels
git fetch --depth 1 origin media && git worktree add /tmp/media origin/media -B media
mkdir -p /tmp/media/<YYYY-MM-DD>
cp jobs/<job>/out/video.mp4 /tmp/media/<YYYY-MM-DD>/<HHMM>_<slug>.mp4
cp jobs/<job>/out/cover.jpg /tmp/media/<YYYY-MM-DD>/<HHMM>_<slug>_cover.jpg
cd /tmp/media && git add -A && git commit -m "media: <date> <slug>" && git push origin media
```
Havola: `https://raw.githubusercontent.com/safarovman/xabash.reels/media/<YYYY-MM-DD>/<HHMM>_<slug>.mp4` — `curl -sIL` bilan 200 ekanini tekshiring. Har bir fayl < 100 MB.
Tozalash: `media` tarmog'ida 14 kundan eski papkalarni o'chiring (faqat Buffer'da `sent` bo'lganlarini).

## 2. Buffer
`config.json` dan `organizationId` va `channelId` ni oling (yoki `get_account` → `list_channels`, akkaunt **xabash.jonkam**, instagram business). `get_account` dan `currentTime` ni oling — vaqt kelajakda bo'lishi shart. Avval `list_posts` bilan band slotlarni tekshiring — dublikat qilmang.

`create_post`:
- `channelId`, `schedulingType: "automatic"` ("notification" ISHLATILMAYDI), `mode: "customScheduled"`, `dueAt: "<YYYY-MM-DD>T<HH:MM>:00+05:00"`
- `text`: `post.json` dagi caption
- `assets: [{"video": {"url": <raw havola>, "metadata": {"thumbnailOffset": <timing.json cover_ms>, "title": <title>}}}]`
- `metadata: {"instagram": {"type": "reel", "shouldShareToFeed": true, "isAiGenerated": true}}`
- Trial reel qilinmaydi. Egasi "hozir" desa — `mode: "shareNow"`.

Buffer bepul tarifida bir vaqtda 10 ta rejalashtirilgan post limiti bor — faqat keyingi 24 soat ichidagi bo'sh slotlarni to'ldiring.

## 3. Tekshirish va hisobot
- `get_post` bilan holatni tekshiring (`scheduled` / `sending` / `sent`; `error` bo'lsa xabarini egasiga ayting).
- `jobs/<job>/post.json` ga `buffer_post_id`, `video_url`, `dueAt` yozing; `topics_log.md` ni yangilang; `main` va `media` ga commit + push.
- Egasiga qisqa jadval yuboring: vaqt — mavzu — holat.