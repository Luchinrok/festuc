# Festuc

PWA sense framework (JS, HTML, CSS, sense build) per portar el menjar de casa: **Compra'm** (llistes de la compra per supermercat, dins un cub de Swiper), **Cuina'm** (receptes, planificador) i **Menja'm** (nevera, congelador, rebost, caducitats i alertes). Abans es deia EatMeFirst / Buyte. Parla amb l'usuari en català.

## On viu i com es publica

- **Adreça:** https://luchinrok.github.io/ (des del 03-11-2026). Repo `Luchinrok/luchinrok.github.io` = remot `origin`. GitHub Pages publica sol des de `main`.
- El repo antic `Luchinrok/Buyte` (remot `buyte-antic`) només serveix la redirecció de `/Buyte/` a l'arrel. No s'hi treballa.
- **Fer push a `main` és publicar.** Treballar en una branca, ensenyar el resultat i **pujar només quan l'usuari ho digui**.
- Git no té identitat configurada en aquest PC: fer els commits amb `git -c user.name="$(git log -1 --format=%an)" -c user.email="$(git log -1 --format=%ae)" commit ...`.
- Les dades de cada usuari són a `localStorage` (claus `eatmefirst_*`), lligades al domini `luchinrok.github.io`: **no canviar mai de domini** sense resoldre abans la migració de dades (vegeu `docs/MIGRACIO-ARREL.md`). Firebase (projecte `eatmefirst`) només serveix per a la sincronització per codi de família.
- A casa **ningú té iPhone** (l'usuari fa servir Android), però l'app ha de continuar funcionant a iOS.

## Llegir abans de començar

- `memory/BACKLOG.md`: font de veritat del que queda pendent i del que s'ha fet. Actualitzar-lo a cada canvi.
- `NOTES.md`: lliçons apreses. **No bloquejar mai `#screen-supermarket` amb `height:100%` + `overflow:hidden`**: trenca el desplaçament a iOS dins del cub de Swiper.
- `docs/MIGRACIO-ARREL.md`: la migració a l'arrel (feta) i el pas que queda per a Google Play.

## Com es treballa

- **Versions als enllaços:** quan es canvia un CSS o un JS, pujar el seu `?v=AAAAMMDD-N` a `index.html` (i `I18N_V` per a les traduccions); si no, els mòbils continuen amb el fitxer antic. No hi ha service worker.
- **Traduccions (ca, es, en, fr):** s'editen **només a `i18n.source.js`** i es regeneren amb `pwsh tools/split-i18n.ps1` (escriu `i18n-core.js` i `i18n/*.js`). Atributs traduïbles: `data-i18n`, `data-i18n-aria`, `data-i18n-placeholder`; al JS, `t('clau')`.
- **Logo i icones:** els mestres són `icons/festuc-icon-any.svg` i `icons/festuc-icon-maskable.svg` (festuc mig obert, 03-11-2026). Les PNG i el `favicon.ico` es regeneren amb `node tools/fes-icones.mjs` (fa servir Edge o Chrome). `tools/gen-icons.ps1` és obsolet.
- **Mascota (Nevi):** `<svg class="nevi">` a `index.html`. El codi (`setNevi` a `js/biteme.js`) li canvia els colors i la boca pels ids `nevi-body`, `nevi-divider`, `nevi-eye-left`, `nevi-eye-right` i `nevi-mouth`: mantenir-los si es redibuixa.
- **Disseny:** hi ha els skills `design-taste-frontend` (veure'n `references/app-screens.md`), `web-design-guidelines` i `awesome-design-md`. Festuc té marca pròpia (targetes amb degradat, emojis 3D de producte): no rebatejar-la. Els emojis de producte són contingut.
- **Textos:** to proper, frases curtes i concretes, que no sembli escrit per una IA.
- **Provar:** servir la carpeta en local (`python -m http.server`) i fer captures a 390 px (p. ex. `node ~/.claude/skills/design-taste-frontend/scripts/inspect.mjs <url> --app --widths 390`). No tocar mai dades reals de cap usuari.

## Google Play (en curs)

Objectiu: publicar Festuc com a TWA. Fet: migració a l'arrel i `.nojekyll` perquè es publiqui `.well-known/`. Queda: compte de desenvolupador de Google Play (25 $, l'ha d'obrir l'usuari; comptes personals nous: comprovar si cal prova tancada amb 12 persones durant 14 dies), generar el paquet (PWABuilder o Bubblewrap) i omplir `.well-known/assetlinks.json` amb el `package_name` i l'empremta SHA-256 reals. Bloquejant tou: primera càrrega (~300 KB gzip de scripts).
