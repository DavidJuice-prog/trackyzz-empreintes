# TRACKYZZ – empreintes quotidiennes du registre

**Don't trust. Verify.**

TRACKYZZ relève chaque minute les prix publics de 7 plateformes (Binance, Coinbase, Kraken, Bitstamp, OKX, Bitfinex,
Bybit). Il les inscrit dans un registre chaîné par SHA-256 et signé (Ed25519). Ce dépôt publie, une fois par jour,
l'**empreinte** des fichiers de la veille. Une empreinte publiée ici n'est **jamais modifiée ni supprimée** : l'historique
git en est la preuve.

Pourquoi ? Si quelqu'un, y compris nous, modifiait plus tard un seul octet du registre d'un jour passé, son SHA-256 ne
correspondrait plus à celui qui est publié ici depuis ce jour-là.

## Contenu de `empreintes/AAAA-MM-JJ.txt`

```
trackyzz-empreinte-jour/1
jour 2026-10-09 (UTC)
registre/2026-10-09.jsonl sha256 … octets … blocs … index … hash_dernier_bloc …
rapports/2026-10-09.md    sha256 … octets …
rapports/2026-10-09.json  sha256 … octets …
tetes/2026-10-10.txt      sha256 … octets …     (tête du registre, horodatée la nuit suivante)
tetes/2026-10-10.txt.tsq / .tsr                 (horodatage RFC 3161, FreeTSA)
```

Les preuves OpenTimestamps (`.ots`) ne figurent pas dans l'empreinte : elles sont complétées plus tard, quand la
transaction Bitcoin est confirmée.

## Vérifier un fichier

Avec un fichier du registre en main (par exemple `registre/2026-10-09.jsonl`) :

```sh
sha256sum registre/2026-10-09.jsonl          # Linux / macOS : shasum -a 256
```

```powershell
Get-FileHash registre\2026-10-09.jsonl -Algorithm SHA256    # Windows
```

Le résultat doit être identique à la ligne `sha256` de l'empreinte du jour. La date de publication dans ce dépôt est
donnée par l'historique git (`git log empreintes/2026-10-09.txt`).

Deux horodatages indépendants couvrent aussi la tête du registre :

- **RFC 3161 (FreeTSA)** :
  ```sh
  openssl ts -verify -in 2026-10-10.txt.tsr -data 2026-10-10.txt -CAfile freetsa_cacert.pem -untrusted freetsa_tsa.crt
  ```
  Les certificats publics sont sur https://freetsa.org/files/.
- **OpenTimestamps (Bitcoin)** : `ots verify 2026-10-10.txt.ots`

## Ce que ce dépôt ne contient pas

Pas de prix, pas de conseil, aucune donnée personnelle : seulement des empreintes. Trackyzz ne vous dit pas quoi
acheter. Il prouve que ses relevés n'ont pas été modifiés après coup.
