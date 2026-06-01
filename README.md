# InsuriFlight

UI demonstrativ pentru interacțiunea cu smart contractul de asigurare parametrică de zbor întârziat, implementat pe Ethereum Sepolia Testnet.

Proiect: **Dezvoltarea de software bazată pe tehnologia blockchain**  
Autor: Vlad Straistraru-Zlate · FEAA Iași · Informatică Economică · 2025–2026

---

## Demo live

> `https://zlv2004.github.io/InsuriFlight/`

---

## Despre proiect

InsuriFlight permite utilizatorilor să cumpere polițe de asigurare parametrică pentru zboruri întârziate direct pe blockchain, fără intermediari. Dacă zborul se întârzie cu cel puțin 3 ore, despăgubirea este trimisă automat în wallet-ul asiguratului prin execuția smart contractului.

**Smart contract:** `0x2DEe28174b1bb49E630194Fa1D7dcdCCa6323C5D` pe Sepolia Testnet  
→ [Vezi pe Etherscan](https://sepolia.etherscan.io/address/0x2DEe28174b1bb49E630194Fa1D7dcdCCa6323C5D)

---

## Stack tehnic

| Componentă | Tehnologie |
|---|---|
| Smart contract | Solidity ^0.8.19, OpenZeppelin v5 |
| IDE & deploy | Remix IDE, Sepolia Testnet |
| Frontend | React 18 (via CDN UMD), htm |
| Blockchain client | ethers.js v6 |
| Wallet | MetaMask |
| Hosting | GitHub Pages (frontend static) |

---

## Funcționalități UI

- **Conectare MetaMask** — detectare automată rețea, avertisment dacă nu ești pe Sepolia
- **Zboruri disponibile** — carduri cu date live din contract (`getFlightRoute`), vizibile și fără wallet conectat
- **Cumpărare poliță** — modal cu confirmare, verificare whitelist, tranzacție `purchasePolicy()`
- **Polițele mele** — scanare evenimente `PolicyPurchased` pentru adresa conectată, butoane Claim și Refund cu stare corectă
- **Request acces** — formular care deschide clientul de email (sistem whitelist)
- **Recomandă zbor** — formular de sugestii via email

---

## Configurare înainte de deploy

Deschide `index.html` și editează două constante în blocul `CONFIG`:

```js
// 1. Emailul tău — pentru formularele de contact
const OWNER_EMAIL = "email@example.com";

// 2. Zborurile înregistrate pe contract — codurile TREBUIE să coincidă
//    exact cu cele din registerFlight() de pe contractul tău
const FLIGHTS_META = [
  { code:"RO372",  origin:"OTP", originCity:"București",   dest:"LHR", destCity:"Londra"    },
  { code:"W61234", origin:"CLJ", originCity:"Cluj-Napoca", dest:"BCN", destCity:"Barcelona" },
  { code:"TK1413", origin:"OTP", originCity:"București",   dest:"IST", destCity:"Istanbul"  },
  { code:"FR8821", origin:"TSR", originCity:"Timișoara",   dest:"CDG", destCity:"Paris"     },
];
```

---

## Deploy pe GitHub Pages

### Pasul 1 — Creează repository

```bash
git init
git add index.html README.md
git commit -m "feat: InsuriFlight UI v1"
git branch -M main
git remote add origin https://github.com/<username>/insuriflight.git
git push -u origin main
```

### Pasul 2 — Activează GitHub Pages

1. Intră pe repository → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `main` / `/ (root)`
4. Salvează

Aplicația va fi disponibilă la `https://<username>.github.io/insuriflight/` în ~2 minute.

### Actualizare ulterioară

```bash
git add index.html
git commit -m "update: <descriere modificare>"
git push
```

---

## Utilizare

### Cerințe

- [MetaMask](https://metamask.io/) instalat în browser
- ETH de test pe Sepolia (obținut de la [pk910 PoW Faucet](https://sepolia-faucet.pk910.de))
- Adresă pe whitelist (contactează owner-ul contractului)

### Flux standard

1. Deschide aplicația și apasă **Conectează wallet**
2. MetaMask va cere permisiunea — confirmă
3. Dacă ești pe o altă rețea, apare un banner cu buton **Comută** → click și acceptă în MetaMask
4. Dacă adresa ta nu e pe whitelist → tab **Request acces** → completează formularul
5. Odată whitelisted, mergi la **Zboruri** → alege un zbor → **Cumpără**
6. Confirmă tranzacția în MetaMask (plătești prima în ETH de test)
7. Mergi la **Polițele mele** pentru a vedea polița activă
8. Când zborul este raportat ca întârziat și fereastra de claim este deschisă → **Revendică despăgubire**

---

## Structura fișierului

Aplicația este un singur fișier HTML self-contained (`index.html`), fără build step, fără dependențe locale.

```
insuriflight/
├── index.html    ← aplicația completă (React + ethers.js v6 via CDN)
└── README.md     ← acest fișier
```

---

## Limitări cunoscute (context academic)

- **Oracle centralizat** — owner-ul contractului raportează manual starea zborului (mock oracle Faza 1). În producție se folosește Chainlink Any API.
- **Whitelist** — necesită aprobare manuală din partea owner-ului; nu există self-registration on-chain.
- **Prima setată manual** — nu se calculează actuarial; owner-ul setează valorile per rută.
- **Capacitate limitată** — contractul necesită capitalizare 100% a expunerii maxime; modelul nu e scalabil comercial fără un pool de lichiditate.
- **Fără getAllFlights()** — mapping-urile Solidity nu sunt iterabile; zborurile disponibile sunt hardcodate în UI.
- **Date de test** — deploy-ul este pe Sepolia Testnet; ETH-ul folosit nu are valoare reală.

---

## Licență

Proiect demonstrativ cu scop educațional. Nu folosiți în producție.
