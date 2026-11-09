---
name: sladka-pika-demo
description: Ključni podatki za gradnjo in urejanje demo strani slaščičarne Sladka Pika (statični generator v Pythonu, vsebina v Markdownu + HTML blokih, objava na GitHub Pages, mobilni prikaz). Uporabi pred vsakim dodajanjem ali urejanjem strani, menija, kontaktnih podatkov, slik, obrazca, videza ali generatorja v tem projektu.
---

# Sladka Pika (demo) – kako graditi ta projekt

**Demo stran izmišljenega podjetja.** Vsi podatki so izmišljeni; pasica »Demo stran« v `template.html`,
`noindex` in obrazec brez pošiljanja morajo ostati.

Spletna stran za domače torte. Jezik strani in dela z uporabnikom: **slovenščina**.
Večina obiskovalcev bo na **telefonu** – vsako spremembo preveri tudi pri širini 375 px.

## Zgradba

- `content/**/index.md` → vsaka mapa je ena stran. Strani: `/` (Domov), `/torte`, `/ponudba`,
  `/o-nas`, `/kontakt`. Vsaka `.md` **mora** imeti naslov `# ...` (sicer `no h1 header found`).
- `site.json` → `name`, `lang`, `url` (polni naslov, brez `/` na koncu), `description`, `tagline`, kontakt (`phone`, `phone_display`,
  `whatsapp`, `email`, `location`) in `nav`.
- `template.html` → oznake `{{ Title }}`, `{{ SiteName }}`, `{{ Lang }}`, `{{ Nav }}`, `{{ Content }}`
  `{{ PageUrl }}` (polni URL strani) in poljubne `{{ site.kljuc }}`. Vsebuje glavo (znamka, gumb »Naroči torto«, meni), nogo
  s kontaktom in spodnjo vrstico za telefon (Pokliči · WhatsApp · Naroči).
- `static/` → `index.css`, `favicon.svg`, `images/`; skopira se v izhodno mapo kot je.
- `src/` → generator in testi `test_*.py` (unittest).

## Generator – kaj zna

- `{{ site.kljuc }}` se zamenja v predlogi **in v vsebini** z (escapano) vrednostjo iz `site.json`.
  Neznan ključ → `ValueError: unknown site value`. Kontaktne podatke nikoli ne piši ročno v vsebino,
  vedno prek `{{ site.* }}`.
- `sitemap.xml` in `robots.txt` se ustvarita iz map z `index.md` in `site.json` → `url`
  (`generate_sitemap`; brez `url` se preskočita). Head ima canonical, og:url, og:image.
- Meni: trenutna stran dobi `aria-current="page"` (URL iz poti mape, npr. `/torte`).
- **HTML bloki:** blok, ki se začne z `<` + črka/`!`/`/`, gre na stran nespremenjen (`RawNode`).
  Znotraj HTML bloka **ni praznih vrstic** (prazna vrstica konča blok) in Markdown se ne obdela.
- Basepath: `render_page` zamenja `href="/` in `src="/`. Notranje povezave in slike vedno z `/`.
  Drugi atributi (npr. `action`, `content`) se ne popravljajo.

## Ukazi

- `./test.sh` – testi (vedno poženi po spremembi v `src/`).
- `./main.sh` – gradnja v `public/` + strežnik na http://localhost:8888.
- Predogled v brskalniku: `.claude/launch.json` konfiguracija `stran` (streže `public/`),
  pred tem `python3 src/main.py "/" ./public`.
- `./build.sh` – produkcijska gradnja v `docs/` (basepath `/sladka-pika/`).

## Markdown

- Podprto: naslovi, odstavki, `**krepko**`, `_ležeče_`, `` `koda` ``, kodni bloki, citati `>`,
  seznami `- ` in `1. `, povezave, slike.
- **Ni podprto:** tabele (cene piši kot seznam), `*ležeče*`, gnezdeni seznami.
- Neparni `**` ali `` ` `` → `invalid markdown: unmatched ...`.
- Slog: vikanje, topel in preprost ton.

## Gradniki (CSS razredi)

- `.btn`, `.btn-ghost`, `.btn-whatsapp`, `.btn-viber`; vrsta gumbov v `<p class="actions">`.
- `.hero` (domača stran: slika + `.hero-text`), `.features` > `.feature`, `.cta` (poziv k naročilu).
- `.gallery` > `figure.card` > `img` + `figcaption` (`<strong>` ime, `<span>` cena) + `<p>` + `<a>`.
- `.about` (slika + besedilo), `.contact-buttons`, `form.order-form` (`id="narocilo"`), `.form-row`.
- Barve so spremenljivke na `:root` (`--accent` #c2386b, `--accent-dark`, `--accent-soft`, `--bg` ...). Nove dodaj tja.
- Kontrast besedila mora biti ≥ 4.5 : 1 (WCAG AA). Na `--accent-soft` piši z `--accent-dark`, ne z `--accent`;
  bel napis gre na `--accent`, `--whatsapp` (#167c41) in `--viber` (#6550e6).
- CSS je mobile-first: osnovna pravila za telefon, `@media (min-width: 560px)` in `(min-width: 860px)`.
  Polja obrazca imajo `font-size` ≥ 16px (iPhone sicer povečuje), gumbi ≥ 46px visoki.

## Obrazec

Web3Forms (`https://api.web3forms.com/submit`), ključ v `site.json` → `web3forms_key`.
Polja: ime, telefon, e-pošta, vrsta torte, datum prevzema (min. +7 dni prek JS), število oseb, opombe.
Ključ si mora uporabnik ustvariti sam na web3forms.com.

## Slike

Ilustracije (SVG, brez besedila na sliki) so v `static/images/` (naslovna, rojstnodnevna, cokoladna, jagodna, otroska,
porocna, cupcakes, kuhinja). Prave fotografije: `.webp`/`.jpg`, do ~1600 px, pod 300 KB, razmerje
4:3 za torte; zamenjaj `src` v vsebini in posodobi `width`/`height`. Za več velikosti `srcset`
(generator ga **ne** popravlja za basepath). Slika za deljenje: `static/images/deljenje.png` 1200×630
(iz `deljenje.svg`, upodobljeno s Playwright: zaslonska slika SVG pri 1200×630).

## Objava

Push na `main` → GitHub Actions: testi, `build.sh`, objava `docs/` na
https://milinkovicluka.github.io/sladka-pika/ . Pri lastni domeni: `static/CNAME` in basepath `/`.

## Odprte stvari

- Vsi kontaktni podatki, cene, okusi in besedilo »O nas« so izmišljeni (demo).
- Obrazec ima `data-demo` in ne pošilja (prikaže demo sporočilo).
- Lighthouse lokalno ne gre (ni Node/Chrome); uporabi pagespeed.web.dev na objavljeni strani.
- `robots.txt` deluje šele z lastno domeno (GitHub project pages niso koren domene).
