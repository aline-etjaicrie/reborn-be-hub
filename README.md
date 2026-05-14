# Hub Com — Reborn & Be

Interface de pilotage éditorial de l'écosystème Reborn & Be.  
Hébergé sur GitHub Pages — URL permanente, jamais modifiée.

---

## URLs stables

| Page | URL |
|------|-----|
| **Hub principal** | `https://[username].github.io/reborn-be-hub` |
| **Évaluation** | `https://[username].github.io/reborn-be-hub/eval.html` |

> Remplacer `[username]` par le nom du compte GitHub propriétaire du repo.

---

## Architecture deux couches

```
GitHub Pages (front)          Apps Script (back/proxy)
──────────────────────        ──────────────────────────
index.html                    Code.gs
  • Interface utilisateur        • Proxy Anthropic (IA)
  • Pipeline éditorial           • Lecture/écriture Sheet
  • Photothèque                  • Google Calendar
  • Guide com                    • Drive upload
  • Boucles WhatsApp             • Gmail alertes
        │
        └──── POST ──────────► URL proxy figée (version v1)
```

Le front ne contient **aucune clé API** — tout passe par le proxy Apps Script.

---

## Mettre à jour le HUB (interface, textes, UI)

```bash
# 1. Modifier index.html localement
# 2. Committer et pusher
git add index.html
git commit -m "feat: description du changement"
git push origin main
# GitHub Pages se met à jour en ~2 minutes
# L'URL ne change pas
```

---

## Mettre à jour le PROXY (logique API, nouvelles actions)

```
1. Modifier Code.gs dans Apps Script
2. Déployer → Gérer les déploiements → Nouvelle version (ex: v2)
3. Copier la nouvelle URL /exec
4. Dans index.html, mettre à jour la constante :
      const PROXY_URL = 'https://script.google.com/macros/s/NOUVELLE_ID/exec';
5. Committer et pusher (voir section ci-dessus)
```

> Faire ça **rarement** — uniquement si la logique backend change.  
> Une fois le proxy déployé en version figée, son URL ne change plus.

---

## Configurer le proxy (première fois)

Dans `index.html`, ligne ~1435 :

```js
const PROXY_URL = 'REMPLACER_PAR_URL_PROXY';
```

Remplacer `REMPLACER_PAR_URL_PROXY` par l'URL Apps Script déployée en version figée.

### Obtenir l'URL Apps Script figée

1. Ouvrir le projet Apps Script
2. **Déployer → Gérer les déploiements → Nouveau déploiement**
3. Type : Application Web
4. Accès : Tout le monde
5. Version : créer `v1` (ne jamais modifier cette version ensuite)
6. Copier l'URL `/exec` → c'est l'URL à coller dans `PROXY_URL`

---

## Fichiers du repo

| Fichier | Rôle |
|---------|------|
| `index.html` | Hub Com complet — interface principale |
| `eval.html` | Page de présentation du projet |
| `README.md` | Cette documentation |

---

## Propriétés Apps Script requises

Configurer dans Apps Script → Propriétés du script :

| Clé | Valeur |
|-----|--------|
| `ANTHROPIC_API_KEY` | Clé API Anthropic |
| `BRAND_CALENDARS` | JSON auto-généré par `setupBrandCalendars()` |
| `BRAND_FOLDERS` | JSON auto-généré au premier upload Drive |

---

*Hub Com Reborn & Be — L'Atelier Com*
