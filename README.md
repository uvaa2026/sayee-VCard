# Digital Business Card – Azure deployment

## Files
| File | Purpose |
|---|---|
| `index.html` | The mobile card page people see after scanning |
| `contact.vcf` | The contact file behind the **Save Contact** button |
| `staticwebapp.config.json` | Makes Azure Static Web Apps serve `.vcf` correctly |
| `web.config` | Only if you use App Service (Windows) instead |
| `qr-generator.html` | Open locally to create the printable QR PNG |
| `photo.jpg` | Photo shown on the card page (not saved into the contact) |

## 1. Details (already filled in)
To change anything later, edit
**both** `index.html` and `contact.vcf`. Phone numbers: use international format (`+91...`).
WhatsApp link format: `https://wa.me/91XXXXXXXXXX` (no `+` or spaces).
Brand colours are at the top of `index.html` (`--navy`, `--orange`).

## 2. Deploy to Azure Static Web Apps (Free plan)

**Option A – Portal + SWA CLI (no GitHub needed)**
1. Azure Portal → *Create a resource* → **Static Web App**.
2. Plan type: **Free**. Deployment source: **Other**. Create.
3. Open the resource → *Manage deployment token* → copy the token.
4. On your PC (Node.js installed), in this folder:
   ```bash
   npm install -g @azure/static-web-apps-cli
   swa deploy . --env production --deployment-token <YOUR_TOKEN>
   ```
5. Your URL is shown on the resource *Overview* page (e.g. `https://xxxx.azurestaticapps.net`).

**Option B – GitHub**: push this folder to a repo, create the Static Web App with source = GitHub,
App location = `/`, no build/output location. Every push redeploys.

**Optional custom domain**: Static Web App → *Custom domains* → e.g. `card.yourdomain.com`
(free SSL included). Do this *before* generating the QR so the QR never needs reprinting.

**App Service alternative (F1 Free)**: zip-deploy the folder; `web.config` handles the `.vcf` MIME type.

## 3. Create the QR code
Open `qr-generator.html` in a browser, paste your live URL, click Generate → Download PNG.
Print at least **2 cm × 2 cm** with a white margin around it.

## 4. Test
Scan with an iPhone and an Android phone, tap **Save Contact**:
- iPhone (Safari): shows the contact preview → *Create New Contact*.
- Android (Chrome): downloads the file → tap it → choose Contacts to import.

Updating details later: just edit and redeploy – the QR stays the same.
