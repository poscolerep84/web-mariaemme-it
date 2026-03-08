# Pagina di parcheggio domini — Omnia Service srl

Pagina statica ospitata su **GitHub Pages**, progettata per fungere da landing page di cortesia per tutti i domini gestiti da **Omnia Service srl** che non hanno ancora un sito web attivo.

---

## Panoramica

Quando un cliente affida a Omnia Service la gestione del proprio dominio (es. `miodominio.it`), il dominio può essere puntato a questa pagina tramite un record **CNAME**. L'utente che visita il dominio vede una pagina professionale che:

- mostra automaticamente il nome del dominio visitato nell'hero
- presenta Omnia Service come gestore del dominio
- elenca i servizi IT offerti
- fornisce i contatti e le sedi operative

---

## Come funziona — GitHub Pages

### Repository e branch

La pagina è ospitata su un repository GitHub pubblico. GitHub Pages serve il contenuto del branch `main` (o del branch/cartella configurata nelle impostazioni del repository).

### File principali

| File | Ruolo |
|---|---|
| `index.html` | Unica pagina HTML, completamente self-contained (CSS e JS inline) |
| `CNAME` | Contiene il dominio principale associato al repository GitHub Pages |
| `README.md` | Questo file di documentazione |

### Il file CNAME

Il file `CNAME` contiene il dominio primario del repository, attualmente:

```
mariaemme.it
```

GitHub Pages usa questo file per sapere quale dominio "proprio" servire. Gli altri domini aggiuntivi vengono gestiti tramite CNAME DNS lato client (vedi sezione dedicata).

---

## Dominio dinamico nell'hero — come funziona

Il punto centrale di questa pagina è la capacità di **mostrarsi personalizzata per ciascun dominio** che la raggiunge, senza alcuna modifica al codice.

### Il badge nell'hero

In cima all'hero è presente un badge rotondeggiante che mostra il nome del dominio visitato:

```
[ miodominio.it ]
```

### Il meccanismo JavaScript

Alla fine del file `index.html` è presente questo script:

```javascript
var host = window.location.hostname;
if (host) {
  document.getElementById('domain-label').textContent = host;
  document.getElementById('page-title').textContent = host + ' – Ospitato da Omnia Service';
} else {
  document.getElementById('domain-label').textContent = 'Grazie per aver visitato questa pagina';
  document.getElementById('page-title').textContent = 'Ospitato da Omnia Service';
}
```

**Cosa fa:**

1. Legge `window.location.hostname` — il dominio con cui il browser ha aperto la pagina (es. `cliente1.it`)
2. Se il dominio è rilevato, lo inserisce nel badge hero e nel titolo della tab del browser
3. Se il dominio **non è rilevabile** (es. pagina aperta come file locale da disco), mostra il testo di fallback `"Grazie per aver visitato questa pagina"`

**Nessuna configurazione server-side è necessaria.** La logica è interamente lato client (browser).

---

## Come aggiungere un nuovo dominio

Per far puntare un nuovo dominio a questa pagina sono necessari **due passaggi**:

### 1. Configurare il DNS del dominio (lato registrar/cliente)

Accedere al pannello DNS del dominio e aggiungere un record CNAME:

| Tipo | Nome (host) | Valore (destinazione) | TTL |
|---|---|---|---|
| CNAME | `@` oppure `www` | `poscolerep84.github.io` | 3600 (o auto) |

> **Nota:** Alcuni registrar non permettono CNAME su `@` (root). In quel caso usare record **ALIAS** o **ANAME** se disponibili, oppure puntare solo il sottodominio `www`.

### 2. Aggiungere il dominio in GitHub Pages (Custom Domains)

1. Aprire le **Settings** del repository su GitHub
2. Sezione **Pages → Custom domain**
3. Aggiungere il nuovo dominio e salvare
4. Attendere la verifica e l'emissione del certificato SSL (automatica, tramite Let's Encrypt)

> GitHub Pages supporta più domini custom sullo stesso repository. Il file `CNAME` nella root del repo contiene il dominio primario; i domini aggiuntivi vanno aggiunti dalle impostazioni GitHub.

---

## Flusso completo — dalla richiesta alla pagina live

```
Browser utente
     |
     | digita: miodominio.it
     v
DNS registrar
     |
     | CNAME → poscolerep84.github.io
     v
Server GitHub Pages
     |
     | serve index.html
     v
Browser carica la pagina
     |
     | JavaScript legge window.location.hostname = "miodominio.it"
     v
Badge hero mostra: [ miodominio.it ]
Titolo tab:        miodominio.it – Ospitato da Omnia Service
```

---

## SSL / HTTPS

GitHub Pages fornisce automaticamente un certificato SSL gratuito (Let's Encrypt) per ogni dominio custom aggiunto e verificato. Non è necessaria alcuna configurazione manuale.

La pagina funziona correttamente sia in HTTP che in HTTPS. GitHub Pages reindirizza automaticamente al HTTPS se l'opzione "Enforce HTTPS" è abilitata nelle impostazioni del repository.

---

## Manutenzione e aggiornamenti

La pagina è un singolo file HTML statico. Per aggiornare contenuti (testi, contatti, servizi, stile grafico):

1. Modificare `index.html` nel repository
2. Fare commit e push sul branch `main`
3. GitHub Pages pubblica automaticamente entro pochi secondi

Non è necessario alcun sistema di build, framework o dipendenza esterna. Tutto il CSS e il JavaScript sono inline nel file HTML.

---

## Struttura del file index.html

```
index.html
├── <head>
│   ├── Meta tag (charset, viewport, description)
│   ├── <title> — aggiornato dinamicamente via JS
│   └── <style> — tutto il CSS inline
└── <body>
    ├── #hero          — sezione principale con badge dominio dinamico
    ├── #stats         — barra con statistiche (24/7, sedi, servizi)
    ├── #servizi       — griglia card servizi IT
    ├── #info          — sedi e contatti
    ├── <footer>       — copyright e link
    └── <script>       — anno corrente + logica dominio dinamico
```

---

## Contatti gestore

**Omnia Service srl unipersonale**
- Sede principale: Piazza Garibaldi 59, Lonigo (VI)
- Tel: 0444 437746
- Email: info@omniaservice-italia.it
- Web: www.omniaservice-italia.it
