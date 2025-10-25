
<div align="center">

# 👋 Xin chào, ta là **Bit Eric** — _trẫm_ đây!
**Full‑stack Builder • Language Tech • Cultural Preservation**  
**Founder of _Học Tiếng Tày – Cánh đồng Ngôn ngữ_**

[![Profile Views](https://komarev.com/ghpvc/?username=biteric&style=flat)](https://github.com/biteric)
[![Followers](https://img.shields.io/github/followers/biteric?style=flat)](https://github.com/biteric?tab=followers)
[![Stars](https://img.shields.io/github/stars/biteric?affiliations=OWNER%2CCOLLABORATOR&style=flat)](https://github.com/biteric?tab=repositories)

</div>

---

## 🚀 Dự án trọng điểm: **Học Tiếng Tày – Cánh đồng Ngôn ngữ**
Nền tảng học ngôn ngữ số nhằm bảo tồn & lan tỏa văn hóa Tày qua bài học tương tác, audio‑visual, chatbot AI và lộ trình đa chương (Chương I, II, III…).

**Tính năng nổi bật**
- 🎯 **Bài học tương tác**: CardQuestion, ChoiceAudio, MatchQuestion, WriteAnswer, ArrangeSentence, FillInTheBlank…
- 🔊 **Phát âm**: bài luyện phát âm theo câu, theo từ; session theo lộ trình; chấm điểm & theo dõi tiến độ.
- 🔥 **Gamification**: streak ngày, mục tiêu hàng ngày, popup hoàn thành bài, tiến độ từng chương/bài.
- 🤖 **AI trợ giảng**: chatbot gợi ý/hints theo ngữ cảnh bài, ưu tiên kiến thức từ DB nội bộ trước, thiếu mới mở rộng.
- 📰 **Blog/Khám phá**: bài viết dài, ảnh hợp pháp (Unsplash API), sinh nội dung hỗ trợ bằng AI.
- ☁️ **CDN Media**: chuẩn hóa đường dẫn `upload/audio|image/...` → CDN (Cloudflare R2).

**Ngăn xếp công nghệ**
- Frontend: **Next.js 14**, **React 18**, **Tailwind**, **Lucide**, **mobile‑first**
- Backend: **Node.js 20**, **Express**, **PM2**, **Nginx/Cloudflare proxy**
- Data: **MySQL 8 / TiDB Cloud**, chuẩn hóa collation `utf8mb4_*`, seed scripts JSON
- Auth & tiện ích: **Firebase Auth** (khi cần), **Resend** mail, **CORS/Helmet/Rate‑Limit**
- Tools: **DBeaver**, **phpMyAdmin**, **GitHub flow** (branches, rebase/merge), **SSH port‑forwarding**

**Kiến trúc rút gọn**
```
hoctiengtay/
├─ frontend (Next.js)     → hoctiengtay.edu.vn
├─ backend  (Express API) → k9.lce.vn /api/v1
└─ storage  (R2 CDN)      → cdn.hoctiengtay.edu.vn/upload/{audio|image}/...
```

---

## 🧭 Triết lý kỹ thuật
- **Dữ liệu là trung tâm**: chuẩn hoá schema, seeding đa môi trường, theo dõi tiến độ người học.
- **Tối ưu trải nghiệm**: hiệu năng FE, trạng thái rõ ràng (đúng/sai, tiến độ, khóa/mở).
- **Triển khai vững**: tách miền FE/BE/CDN, CORS đúng thứ tự, log & rate‑limit, không lộ bí mật.
- **Hợp tác**: guideline cho team (branch naming, run local nhanh, DB remote an toàn).

---

## 🧰 Tech Stack & Thói quen làm việc
**Ngôn ngữ & FE**  
`JavaScript` `React` `Next.js` `Tailwind` `Lucide`

**BE & Hệ thống**  
`Node.js` `Express` `PM2` `Nginx` `Cloudflare` `Firebase Auth`

**CSDL & Dữ liệu**  
`MySQL 8` `TiDB Cloud` `DBeaver` `phpMyAdmin`

**DevOps**  
`SSH` `Port‑forwarding` `CORS` `Helmet` `Rate‑Limiter`

**Phương thức**  
`GitHub Flow` `Code Review` `Mobile‑first` `Perf‑aware`

---

## 🏆 Dấu mốc đáng nhớ
- **Thiết kế schema khóa học**: Chương → Bài → Bộ câu hỏi đa loại (Card/Choice/Match/Write/Arrange/Fill).
- **Gamification**: `user_daily_stats`, streak & goal theo ngày; popup hoàn thành & cập nhật tiến độ tức thời.
- **Seed đa hướng**: từ đường dẫn tương đối → URL CDN chuẩn; hợp nhất bảng từ vựng & nghĩa; sửa collation.
- **Triển khai**: tách miền FE/BE, cấu hình CORS chính xác, Nginx/CF proxy, theo dõi log & hạn mức.

---

## 🗺️ Roadmap ngắn hạn (Q4/2025)
- [ ] Phát âm nâng cao: phân tích âm vị & chấm điểm thời gian thực
- [ ] Mở rộng ngân hàng câu hỏi & audio chuẩn hoá
- [ ] Dashboard giáo viên: theo dõi lớp & từng học viên
- [ ] Chế độ ôn luyện thông minh (SRS) cho từ vựng
- [ ] Nội dung văn hoá (Hát Then, kho tàng truyện) dạng học tương tác

---

## 📦 Những thứ “plug‑and‑play” trẫm hay dùng
**Express server mẫu (production‑ready, rút gọn)**

```js
// server.js
require("dotenv").config();
const express = require("express");
const cors = require("cors");
const helmet = require("helmet");
const morgan = require("morgan");
const rateLimit = require("express-rate-limit");

const app = express();
app.disable("x-powered-by");
app.set("trust proxy", true);

app.use(helmet());
app.use(morgan("dev"));
app.use(express.json({ limit: "1mb" }));

app.use(cors({
  origin: (process.env.CORS_ORIGINS || "").split(",").filter(Boolean),
  credentials: process.env.CORS_CREDENTIALS === "true"
}));

app.use(rateLimit({ windowMs: 60 * 1000, max: 300 }));

app.get("/api/v1/health", (_req, res) => res.json({ ok: true }));

const PORT = Number(process.env.PORT || 5500);
app.listen(PORT, () => console.log(`API listening on :${PORT}`));
```

**React component cho bài học (JS thuần, mobile‑first)**

```jsx
// components/LessonCard.js
"use client";
import Link from "next/link";

export default function LessonCard({ href, title, progress = 0, locked = false }) {
  return (
    <Link
      href={locked ? "#" : href}
      className={`block p-4 rounded-2xl shadow-sm border
        ${locked ? "opacity-60 cursor-not-allowed" : "hover:shadow-md"}`}
      aria-disabled={locked}
    >
      <div className="text-base font-semibold">{title}</div>
      <div className="mt-2 h-2 bg-gray-200 rounded-full overflow-hidden">
        <div className="h-full bg-blue-500" style={{ width: `${progress}%` }} />
      </div>
      <div className="mt-2 text-sm text-gray-500">{progress}% hoàn thành</div>
    </Link>
  );
}
```

---

## 📊 GitHub Stats (tùy chọn)
> Bật nếu thích; chỉ cần sửa `username` thành tài khoản của trẫm.

[![GitHub Stats](https://github-readme-stats.vercel.app/api?username=biteric&show_icons=true)](https://github.com/anuraghazra/github-readme-stats)
[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=biteric&layout=compact)](https://github.com/anuraghazra/github-readme-stats)

---

## 🤝 Kết nối & hợp tác
- 🌐 Website: **hoctiengtay.edu.vn**
- 🔧 API: **k9.lce.vn** `/api/v1`
- ✉️ Email: **CSKH@hoctiengtay.edu.vn**
- 💬 Trao đổi ý tưởng, mentor/mentee, cộng tác nội dung văn hoá — **rất hoan nghênh**!

---

## 💬 Châm ngôn bỏ túi
> _“Code đẹp là code có người dùng.”_  
> _“Bảo tồn văn hoá bằng công nghệ chính là viết phần mềm biết kể chuyện.”_

<div align="center">
  
**Nếu thấy hay — cho trẫm một ⭐ nhé!**

</div>
