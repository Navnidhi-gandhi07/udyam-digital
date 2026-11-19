# Udyam Digital — Vercel deploy guide

Is folder mein ready-to-deploy static build hai (koi build step nahi chahiye).

```
udyam-digital-deploy/
  public/        <- poori app (index.html, assets/, data/, ocr/)
  vercel.json    <- SPA rewrites + cache headers + "no build" settings
  DEPLOY.md
```

`public/ocr/` mein local Tesseract OCR ke files hain (worker, wasm, English data), jo aapke original `dist/` mein missing the.

## Option A — GitHub + Vercel (recommended)
1. github.com par New repository banao (name: `udyam-digital`), empty rakho.
2. Is folder ke andar terminal kholo:
   ```
   git init
   git add .
   git commit -m "Udyam Digital deploy build"
   git branch -M main
   git remote add origin https://github.com/<USERNAME>/udyam-digital.git
   git push -u origin main
   ```
3. vercel.com -> Sign up with GitHub -> Add New -> Project -> `udyam-digital` -> Import.
4. Settings jo dikhengi (vercel.json se auto-set hongi, kuch badalna nahi):
   - Framework Preset: Other
   - Build Command: (khaali)
   - Output Directory: `public`
5. Deploy dabao. 1-2 min mein `https://udyam-digital-xxxx.vercel.app` mil jaayega.

## Option B — Bina GitHub ke (Vercel CLI)
```
npm i -g vercel
vercel login
vercel --prod
```
Sawal aaye toh: Set up and deploy? Y -> scope choose -> Link to existing project? N -> project name `udyam-digital` -> directory `./` -> baaki defaults (Enter).

## Deploy ke baad checklist (3 min)
1. Production URL kholo (preview URL nahi) aur `/kyc` seedha browser mein paste karke refresh karo. Page khulna chahiye (404 nahi).
2. Demo scenario A: Professional office, 20, 1, Business broadband, 6000, GSTIN `37LDYXK0296F1ZB`, Tier 2, Office Pro, consent, Look up, sirf `K09_signatory_id.png` + `K09_address_proof.png` upload (GST certificate upload mat karo), Run validation -> "Ready for next step".
3. Checkout -> demo UPI -> tracker mein 3 simulate buttons.
4. Agar Vercel login maange: Project -> Settings -> Deployment Protection -> Vercel Authentication OFF (ya Production URL use karo).

## Demo ke tips
- Page refresh mat karo: data sirf memory mein rehta hai (design choice).
- Har naya applicant: "Reset demo" dabao.
- K01 -> Blocked (Cancelled GST) ; K11 -> Needs manual review (stale address proof).
- K01 ke liye GSTIN `37IHKXV5167L1ZS`, K11 ke liye `03KGZXT1320R1ZZ`.

## Limitation
Isme `src/` source code nahi hai (sirf compiled build). Code badalna ho toh original `src/` + `scripts/` wale project se `npm run build` karke naya `dist/` is `public/` mein daalna hoga (aur `ocr/` folder rehne dena).
