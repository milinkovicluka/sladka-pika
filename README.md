# Sladka Pika – demo stran slaščičarne

**Demo / referenca.** Podjetje Sladka Pika, cene in kontakti so izmišljeni. Stran je označena
z demo pasico, ima `noindex` (ne pojavi se v Googlu), obrazec pa ne pošilja (pokaže demo sporočilo).

Spletna stran za domače torte **Sladka Pika**. Zgradi jo preprost generator
statičnih strani v Pythonu (nastal v okviru tečaja
[Build a Static Site Generator](https://www.boot.dev/courses/build-static-site-generator-python)
na Boot.dev): Markdown datoteke iz `content/` pretvori v HTML strani po predlogi
`template.html` in jim doda datoteke iz `static/`.

Objavljena stran: https://milinkovicluka.github.io/sladka-pika/

## Zahteve

- Python 3.10 ali novejši (brez dodatnih knjižnic)
- Bash (na Windows: Git Bash)

## Uporaba

```bash
./test.sh    # požene teste
./main.sh    # zgradi stran v public/ in jo postreže na http://localhost:8888
./build.sh   # zgradi stran za GitHub Pages v docs/
```

Skripte delujejo na Linuxu, macOS in Windows (Git Bash).

## Zgradba projekta

| Pot | Vsebina |
| --- | --- |
| `content/` | strani v Markdownu; vsaka mapa z `index.md` postane svoja stran |
| `site.json` | ime, opis, kontaktni podatki in meni |
| `template.html` | glava z menijem in gumbom »Naroči«, noga, spodnja vrstica za telefon |
| `static/` | `index.css`, `favicon.svg` in slike v `static/images/` |

Pri gradnji nastaneta še `sitemap.xml` in `robots.txt` (iz map v `content/` in `url` v `site.json`).
| `src/` | generator in testi |

Strani: Domov, Torte, Ponudba, O nas in Kontakt (z obrazcem za naročilo).

## Podatki strani (`site.json`)

Kontaktni podatki so zapisani samo enkrat, v `site.json`. V predlogi in v
vsebini jih uporabiš z `{{ site.kljuc }}`, npr. `{{ site.phone_display }}`.

| Ključ | Primer | Uporaba |
| --- | --- | --- |
| `phone` | `+38640123456` | povezave `tel:` (gumb Pokliči) |
| `phone_display` | `040 123 456` | prikaz številke |
| `whatsapp` | `38640123456` | WhatsApp in Viber (brez `+` in presledkov) |
| `email` | `info@example.com` | e-pošta |
| `location` | `Ljubljana z okolico` | kraj prevzema |
| `url` | `https://milinkovicluka.github.io/sladka-pika` | polni naslov strani: sitemap, kanonični URL, slika za deljenje |
| `description`, `tagline` | | opis za Google in slogan |

Neznan ključ (npr. tipkarska napaka) ustavi gradnjo z napako, da ne pride na stran.

## Obrazec za naročilo

V demo različici obrazec ne pošilja (`data-demo`, prikaže sporočilo). Za pravo stranko obrazec pošilja prek brezplačne storitve
[Web3Forms](https://web3forms.com): na njihovi strani vpišeš e-pošto, na katero
želiš prejemati naročila, dobiš ključ (access key) in ga vpišeš v `site.json`
pod `web3forms_key`. Dokler je tam `VSTAVI-SVOJ-KLJUC`, obrazec ne deluje.

## Slike

Na demo strani so ilustracije v SVG (`static/images/*.svg`): lahke, ostre na vsakem zaslonu in brez avtorskih omejitev.
Za pravo stranko jih zamenjaš s fotografijami:

1. Shrani jih v `static/images/` (najbolje `.webp` ali `.jpg`, široke do ~1600 px, pod 300 KB).
2. V `content/` zamenjaj ime datoteke v `src="/images/..."`.

Velikosti slik: naslovna 1200 × 700, torte 800 × 600 (razmerje 4 : 3), O nas 600 × 700.

Slika za deljenje na Facebooku/WhatsAppu je `static/images/deljenje.png` (1200 × 630,
narejena iz `deljenje.svg`). Ko bodo prave fotografije, jo zamenjaj z lepo fotografijo torte.

Prave fotografije pripravi v več velikostih (npr. `torta-800.webp`, `torta-1600.webp`) in
jih poveži s `srcset`, da telefon prenese manjšo:

```html
<img src="/images/torta-800.webp" srcset="/images/torta-800.webp 800w, /images/torta-1600.webp 1600w" sizes="(min-width: 860px) 33vw, 100vw" alt="..." width="800" height="600" loading="lazy">
```

Pretvorba na Macu: `cwebp` (`brew install webp`), nato `cwebp -q 80 -resize 800 0 torta.jpg -o torta-800.webp`.
Pozor: `srcset` generator ne popravlja za basepath, zato na GitHub Pages piši tam polne poti z `/sladka-pika/` ali počakaj na lastno domeno.

## Nova stran

1. Ustvari mapo, npr. `content/galerija/`, in v njej `index.md`, ki se začne z naslovom `# Galerija`.
2. Po želji jo dodaj v meni v `site.json`: `{ "label": "Galerija", "url": "/galerija" }`.
3. Notranje povezave in slike piši z začetnim `/` (npr. `/torte`, `/images/torta.webp`),
   ker generator pred njih sam doda pravi basepath.

## Podprt Markdown

Naslovi (`#`–`######`), odstavki, **krepko**, _ležeče_, `koda`, kodni bloki,
citati, neoštevilčeni (`- `) in oštevilčeni (`1. `) seznami, povezave in slike.

**HTML bloki:** blok, ki se začne z oznako (`<div`, `<form`, `<section` ...), gre
na stran nespremenjen. Tako so narejene galerija, naslovni del in obrazec.
HTML blok ne sme vsebovati prazne vrstice, ker ta konča blok.

## Telefon

Stran je narejena najprej za telefon: meni je v eni vrstici, galerija v enem
stolpcu, spodaj pa je stalna vrstica **Pokliči · WhatsApp · Naroči**. Na
računalniku (od 860 px) se spodnja vrstica skrije, gumb »Naroči torto« pa je v glavi.

## Objava

Ob vsakem pushu na `main` GitHub Actions (`.github/workflows/deploy.yml`) požene
teste, zgradi stran in jo objavi na GitHub Pages. Mapi `docs/` in `public/` nista
v repozitoriju, ker se ustvarita pri gradnji.

Basepath (`/sladka-pika/`) se nastavi sam iz imena repozitorija, za
`uporabnik.github.io` je `/`. Ročni preklic: `BASEPATH="/xyz/" ./build.sh`.
Pri lastni domeni mora biti basepath `/`.

`robots.txt` velja samo v korenu domene, zato začne delovati šele z lastno domeno.
Sitemap lahko že zdaj ročno oddaš v [Google Search Console](https://search.google.com/search-console).

Hitrost in dostopnost preveri na [PageSpeed Insights](https://pagespeed.web.dev) po vsaki večji spremembi.

## Če iz demo strani nastane prava stran

- [ ] Odstrani demo pasico v `template.html` in `noindex` iz glave
- [ ] Pravi kontakti v `site.json` in ključ Web3Forms; obrazcu odstrani `data-demo` in demo skripto
- [ ] Prave fotografije v WebP s `srcset` in prava slika za deljenje
- [ ] Prave cene, okusi in besedilo O nas
- [ ] Lastna domena
