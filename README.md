# hreflang-checker

Piccolo strumento da riga di comando per controllare i tag **hreflang** di una pagina web.
Gli errori hreflang sono tra i problemi piu' comuni nei siti multilingua: Google puo' mostrare
la versione sbagliata della pagina o ignorare del tutto le alternative.

*English: a small command-line tool that checks a page's hreflang tags (valid codes, duplicates,
self-reference, x-default and return links). Standard library only, no dependencies.*

## Cosa controlla

- codici lingua validi (`it`, `en-GB`, `zh-Hant`, `x-default`...)
- hreflang duplicati
- URL assoluti
- auto-riferimento (la pagina deve includere se stessa)
- presenza di `x-default` (avviso)
- **reciprocita'**: ogni versione linguistica deve rimandare alla pagina di partenza

## Requisiti

Python 3.8 o superiore. Nessuna libreria da installare.

## Uso

```bash
python3 hreflang_checker.py https://www.esempio.it/pagina/
```

Opzioni:

```bash
python3 hreflang_checker.py URL --no-reciprocal   # salta il controllo di reciprocita' (piu' veloce)
python3 hreflang_checker.py URL --timeout 20      # timeout in secondi
```

Codici di uscita: `0` nessun errore, `1` errori trovati, `2` pagina non raggiungibile
(utile per usarlo in script o in CI).

## Esempio di output

```
Pagina: http://127.0.0.1:8765/it.html
Versioni linguistiche trovate: 4
  it         http://127.0.0.1:8765/it.html
  en         http://127.0.0.1:8765/en.html
  de         http://127.0.0.1:8765/de.html
  xx_YY      http://127.0.0.1:8765/bad.html
AVVISO  Manca x-default (consigliato per utenti con lingua non coperta).
ERRORE  Codice lingua non valido: 'xx_YY' (http://127.0.0.1:8765/bad.html)
ERRORE  [de] http://127.0.0.1:8765/de.html NON rimanda a http://127.0.0.1:8765/it.html (manca la reciprocita').
ERRORE  [xx_YY] impossibile aprire http://127.0.0.1:8765/bad.html: HTTP Error 404: File not found
```

## Limiti

- Legge solo gli hreflang nel `<head>` HTML (non quelli dichiarati nella sitemap XML o negli header HTTP).
- Non esegue JavaScript: se i tag sono inseriti via script non vengono rilevati.
- Controlla una pagina alla volta.

## Idee per sviluppi futuri

- lettura degli hreflang dalla sitemap XML
- scansione di piu' pagine da un elenco
- esportazione del report in CSV

## Autore

Andrea Barbieri, [BTF Traduzioni SEO Sviluppo Web](https://btftraduzioniseoweb.it/).

## Licenza

MIT, vedi il file `LICENSE`.
