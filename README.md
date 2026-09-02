# NEET Premium by Arnab — Deploy Guide

## Files
- `index.html` → app ka pura interface + login/signup + progress-save logic
- `questions.json` → sirf yeh file update karni hai jab naye questions daalne ho

## ⚠️ Pehle Firebase setup karna hai (student login + record save ke liye)
Student ka login aur record cross-device save karne ke liye ek free Firebase project chahiye (Google ki service, koi cost nahi lagti chhote scale pe).

1. https://console.firebase.google.com pe jao → **Add project** → naam do
2. Left menu → **Build → Authentication** → **Get started** → **Email/Password** provider ko **Enable** karo
3. Left menu → **Build → Firestore Database** → **Create database** → **Start in test mode** (baad mein rules tight kar sakte ho)
4. **Project settings (⚙️ icon) → General** tab → neeche scroll karo → **Your apps** → web icon `</>` dabao → app register karo
5. Wahan ek `firebaseConfig` object milega (apiKey, authDomain, projectId, etc.) — usko copy karo
6. `index.html` file mein `const firebaseConfig = {...}` wala section dhundo (Ctrl+F "PASTE_YOUR") aur apne values paste karo
7. File save karke GitHub/Vercel pe commit-push karo

## Approval system (student login → tumhara accept karna)
- Naya student signup karega → account "pending" state mein rahega → use "Waiting for approval" screen dikhegi
- `index.html` mein `const ADMIN_EMAILS = [...]` mein apna email daal do (jis email se tum login karoge)
- Us email se signup/login karo → tumhe bottom nav mein ek extra **Admin** tab dikhega
- Admin tab mein pending students ki list milegi, "Approve" dabate hi wo student turant app use kar payega

Isके baad students signup/login karke apna record kahi se bhi (phone, laptop, koi bhi device) access kar payenge — data Firestore database mein save hota hai.

## GitHub Pages pe live karna (one-time setup)
1. github.com pe login karo → **New repository** → naam do (e.g. `neet-premium`)
2. **Add file → Upload files** → `index.html` aur `questions.json` dono daal do → **Commit**
3. Repo ke andar **Settings → Pages**
4. **Branch: main**, folder: **/ (root)** → **Save**
5. 1-2 min baad link milega: `https://<tumhara-username>.github.io/neet-premium/`

Yeh link students ko bhej do — bas isi ek link se app khulega.

## Baad mein naye questions add karne ka tareeka
1. Repo mein `questions.json` file kholo
2. Pencil (✏️) icon dabao (edit)
3. Naye question objects array ke andar add karo (same format follow karo, comma se separate)
4. Neeche **Commit changes** dabao

Bas. Koi naya deploy, koi app update nahi chahiye — students agli baar app kholenge to naya content khud fetch ho jayega (cache nahi hota, hamesha fresh copy aati hai).

## Naya question add karne ka format
```json
{
  "id": "unique-id-yahan",
  "subject": "phy",
  "chapter": "units-dimensions",
  "source": "NEET 2022",
  "question": "Question text yahan",
  "options": ["Option A", "Option B", "Option C", "Option D"],
  "correct": 0,
  "stats": [40, 20, 25, 15],
  "solution": "Explanation yahan"
}
```
- `subject`: `phy` / `chem` / `zoo` / `bot`
- `correct`: sahi answer ka index, 0 se start (0=A, 1=B, 2=C, 3=D)
- `stats` optional hai
