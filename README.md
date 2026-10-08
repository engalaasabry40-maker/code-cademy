# Code Cademy

Single-page website (`index.html`) with a Supabase backend for accounts, club libraries, level exams, progress and announcements.

## تشغيل الـ Backend (مرة واحدة فقط)

1. افتح مشروعك في [Supabase](https://supabase.com/dashboard) ← **SQL Editor** ← **New query**.
2. انسخ محتوى الملف `supabase/schema.sql` كاملاً والصقه، ثم اضغط **Run**.
   الملف آمن للتشغيل أكثر من مرة.
3. من **Authentication → Providers → Email**: لو فعّلت "Confirm email" فالحساب يُنشأ تلقائياً بعد التأكيد (عن طريق الـ trigger).
4. ارفع `index.html` على GitHub Pages (فرع `main`) — وبعد النشر اعمل تحديث للصفحة بـ **Ctrl + F5**.

### ماذا يضيف `schema.sql`
| الجزء | الوظيفة |
|---|---|
| الجداول | `profiles`, `materials`, `exam_questions`, `exam_attempts`, `progress`, `notifications`, `material_reads` |
| Row Level Security | الطالب يرى مواد ناديه فقط، والمعلم فقط يرفع/يحذف، ولا أحد يرقّي نفسه لمعلم |
| Storage | bucket باسم `materials` (عام للقراءة، 50MB للملف، الرفع للمعلمين فقط) |
| `get_exam` / `submit_exam` | تصحيح الامتحان على السيرفر — الإجابة الصحيحة لا تصل للمتصفح أبداً |
| `club_leaderboard` / `popular_materials` | لوحة أكثر الطلاب قراءة + أكثر الكتب فتحاً |
| Trigger `on_auth_user_created` | ينشئ الملف الشخصي تلقائياً عند التسجيل |
| Realtime | الكتب والإشعارات الجديدة تظهر فوراً بدون تحديث الصفحة |

## Upload "Invalid key" fix
Supabase Storage keys must be plain ASCII. File names such as
`Conversation Club · Beginner Level A1-A2 _ Code Academy.pdf` (the `·`) or Arabic names were rejected.
The site now stores files under a safe key (`Conversation_Club/Beginner_A1-A2/<time>_<rand>_<name>.pdf`)
and keeps the original name in `materials.file_name`, which is used as the download file name.

## Features
- Drag & drop upload with progress, 50 MB check, auto-filled title and clear error messages (EN/AR)
- In-site reader for PDF, images, Word/PowerPoint, audio and video
- Search, "NEW" and "READ" badges, teacher delete
- Reading streak 🔥 with a 7-day strip, and a weekly top-readers leaderboard per club
- Live toasts when a teacher uploads a book or sends an announcement
- Exams graded on the server (falls back to the old in-browser grading until `schema.sql` is installed)
