# SEHAT O AFEYAT — Simple Backend

Sirf 1 file (`server.js`) · Express · data `data.json` file mein save hota hai (database install karne ki zaroorat nahi).

## Chalane ka tareeqa (Node 18+)
```bash
cd backend
npm install
cp .env.example .env     # Windows: copy .env.example .env
# .env kholein: ADMIN_EMAIL, ADMIN_PASSWORD, ANTHROPIC_API_KEY (AI ke liye) set karein
npm start
```
Check: http://localhost:3000/api/health → `{"ok":true}`

Frontend ko isi server se chalana ho to `index.html`, `app.js`, `styles.css` ko `backend/public/` folder mein rakh dein.

## Features aur endpoints
Login ke baad header bhejein: `Authorization: Bearer <token>`

| Feature | Endpoint |
|---|---|
| Signup / Login | `POST /api/signup`, `POST /api/login` |
| Donor search + matching (score + wajah) | `GET /api/donors?group=A+&component=Red Cells&city=Hyderabad&area=&avail=yes&ver=yes&urgency=Urgent` |
| Donor availability | `POST /api/donors/availability {available}` |
| Admin donor verify | `POST /api/donors/:id/verify` |
| Blood request banana (auto-match top 5) | `POST /api/requests` |
| Meri / related requests | `GET /api/requests?status=`, `GET /api/requests/:id` |
| Donor ko request bhejna | `POST /api/requests/:id/send {donorId}` |
| Donor accept / reject | `POST /api/requests/:id/respond {response}` |
| Hospital confirm | `POST /api/requests/:id/confirm` |
| Status tracking | `PATCH /api/requests/:id/status {status}` |
| History (receiver + donor) | `GET /api/history` |
| Notifications (in-app) | `GET /api/notifications`, `POST /api/notifications/read` |
| Dashboards (role ke hisab se) | `GET /api/dashboard` |
| NGO campaigns | `GET /api/campaigns`, `POST /api/campaigns` |
| AI Report Reader | `POST /api/analyze-report {text, language}` |

Status flow: Pending → Matched → Request Sent → Accepted / Rejected → Completed / Cancelled.

## Zaroori rules
- Completed tabhi ho sakta hai jab hospital confirm kare **aur** kisi donor ne accept kiya ho; tab donation history mein entry ban jati hai.
- Requester sirf apni request cancel kar sakta hai; hospital/admin status badal sakte hain.
- Compatibility server par dobara check hoti hai. Final faisla hamesha hospital/blood bank ka hai.
- Notifications sirf app ke andar hain (SMS/WhatsApp nahi).

## Limits (sach)
- `data.json` sirf learning/demo ke liye hai. Asal use ke liye real database aur privacy review chahiye.
- Email verification, password reset aur tests nahi hain.
