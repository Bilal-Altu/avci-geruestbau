# AVCI Gerüstbau – Website

Website für **AVCI Gerüstbau**, Bürstadt (Inhaber: Sabri Avci) — Fassaden-, Dach- und
Traggerüste in den Metropolregionen Rhein-Neckar und Rhein-Main.

## Aufbau

Eine einzelne statische HTML-Datei, kein Build-Schritt, keine Abhängigkeiten.

```
index.html   komplette Seite inkl. CSS und JavaScript
img/         Firmenlogo und Baustellenfotos
```

Extern geladen werden nur die Schrift *Barlow / Barlow Condensed* (Google Fonts)
und die Kartenansicht (Google Maps). Beides ist in der Datenschutzerklärung erwähnt.

## Lokal ansehen

Die Seite braucht keinen Server — `index.html` lässt sich direkt im Browser öffnen.
Für eine realistische Vorschau (relative Pfade, Karte) reicht ein beliebiger
statischer Server, zum Beispiel:

```bash
npx --yes http-server . -p 4610 -c-1
```

## Inhalte pflegen

| Was | Wo in `index.html` |
| --- | --- |
| Telefon, E-Mail, Adresse | Kopfbereich, Abschnitt „Kontakt", Fußzeile und der `application/ld+json`-Block im `<head>` |
| Öffnungszeiten | Abschnitt „Kontakt", Fußzeile und `openingHoursSpecification` im JSON-LD |
| Leistungen | Liste `<ol class="svc">` |
| Referenzfotos | `<div class="gal">`, Dateien in `img/` |
| Bewertungen | `<div class="quotes">` und `aggregateRating` im JSON-LD |
| Einsatzgebiet | `<ul class="towns">` und `areaServed` im JSON-LD |

Die Jahreszahl in der Fußzeile und die „Jahre Erfahrung" im Kennzahlenband
berechnet ein kleines Skript am Seitenende selbst (Gründung März 2009) — die
müssen nicht jedes Jahr angefasst werden.

## Auf den Server bringen (ALL-INKL)

Hochgeladen wird der **Inhalt** dieses Ordners, nicht der Ordner selbst. Am Ende
muss `index.html` direkt im Dokumentenverzeichnis der Domain liegen:

```
/index.html
/.htaccess
/robots.txt
/sitemap.xml
/img/…
```

Nicht mit hochladen: `.git/`, `README.md`, `.gitignore`.

**Schritte im KAS**

1. **Domain anlegen** – „Domain" → Domain eintragen, Dokumentenverzeichnis notieren.
2. **SSL aktivieren** – „Domain" → SSL-Schutz → Let's Encrypt (kostenlos).
   Muss *vor* dem Hochladen der `.htaccess` passieren, sonst sperrt die
   HTTPS-Weiterleitung die Seite aus. Details stehen oben in der Datei.
3. **FTP-Zugang anlegen** – „FTP" → neuer Benutzer, auf das Verzeichnis begrenzen.
4. **Hochladen** – per FTP-Programm (FileZilla, SFTP wenn verfügbar) oder über
   den WebFTP im KAS. Versteckte Dateien einblenden, sonst fehlt die `.htaccess`.
5. **Prüfen** – Seite aufrufen, danach `https://…/img/01-avci-geruestbau-logo.png`
   direkt aufrufen. Kommt ein 404, liegt der `img`-Ordner eine Ebene falsch.

**Nach dem Domainumzug noch erledigen**

- [ ] In `index.html` im `<head>` ergänzen:
      `<link rel="canonical" href="https://avci-geruestbau.de/">`
- [ ] `og:image` auf die absolute Adresse umstellen
      (`https://avci-geruestbau.de/img/02-avci-geruestbau-baustelle.jpg`),
      sonst zeigt die Vorschau bei WhatsApp und Facebook kein Bild.
- [ ] Die GitHub-Pages-Fassung abschalten oder auf `noindex` setzen — sonst
      stehen zwei identische Seiten bei Google in Konkurrenz zueinander.
- [ ] Sitemap in der Google Search Console einreichen.

Beides erst *nach* dem Umzug: Solange die Domain noch auf die alte Seite zeigt,
würde ein Canonical-Verweis Google auf die alte Seite lenken.

## Kontaktformular

Das Formular sendet über [FormSubmit](https://formsubmit.co) an
`avci.geruestbau21@hotmail.de`.

**Vor der ersten Nutzung:** Beim ersten Absenden verschickt FormSubmit eine
Bestätigungsmail an diese Adresse. Erst nach dem Klick darauf werden Anfragen
weitergeleitet.

Danach lässt sich im FormSubmit-Konto ein zufälliger Alias-Endpunkt erzeugen.
Der sollte die Adresse im `action`-Attribut ersetzen, damit die E-Mail-Adresse
nicht im Quelltext steht und von Spam-Bots gelesen werden kann.

## Offene Punkte

- **Umsatzsteuer-ID prüfen.** Im Impressum steht `DE 005 802 614 00`. Das Format
  entspricht eher einer Steuernummer als einer USt-IdNr. (die hat `DE` + 9 Ziffern).
  Impressumsangaben sind rechtlich verbindlich — bitte gegenprüfen.
- **„über 500 Baustellen"** wurde aus der Vorgängerseite übernommen und ist nicht
  belegt.

## Fotos

© AVCI Gerüstbau, Bürstadt. Die Schemazeichnungen der Gerüstarten sind als
Inline-SVG in `index.html` enthalten.
