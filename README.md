# English Numbers — PWA

Portage HTML de l'app SwiftUI `englishNumbers` (Caribou Labs), installable sur iOS et Android, fonctionnelle hors-ligne.

```
index.html              l'application complète (aucune dépendance externe)
manifest.webmanifest    nom, icônes, mode standalone, raccourcis par niveau
sw.js                   service worker, precache des 11 fichiers
icons/                  généré depuis AppIcon.appiconset
```

## Correction apportée aux données

`40` était orthographié `"fourty"` dans `ContentView.swift`. La graphie correcte est **forty** — `four` perd son `u` dans `forty`, exception qui ne touche ni `fourteen` ni `fourth`. Corrigé dans le portage, **à reporter dans l'app Swift** : une app qui enseigne l'anglais ne peut pas apprendre une faute.

## Mise en ligne

Le service worker exige HTTPS ou localhost — en `file://` l'app fonctionne mais ne s'installe pas.

**GitHub Pages** — pousser le contenu de ce dossier à la racine d'un dépôt distinct de celui des tables (deux Pages ne peuvent pas partager la même racine), puis Settings → Pages → branche `main`, dossier `/ (root)`.

**Test local** — `python3 -m http.server 8000` puis `http://localhost:8000`.

## Installation sur iPhone

Safari → Partager → Sur l'écran d'accueil. Ne pas installer depuis un onglet privé : le service worker ne s'y enregistre pas.

## Mise à jour

Cache d'abord, rafraîchissement en arrière-plan : une modification s'applique au deuxième lancement. Incrémenter `VERSION` dans `sw.js` purge l'ancien cache dès l'activation.

## Écarts avec l'app SwiftUI

- **`forty`** au lieu de `fourty` (ci-dessus).
- **« Menu principal »** revient à l'accueil. Dans le Swift, `dismiss()` depuis `ResultView` dépile `QuizView` et retombe sur l'écran Paramètres, ce qui contredit le libellé du bouton.
- **Clavier** ajouté : touches 1 à 6 pour répondre, Entrée pour passer à la suite, L pour réécouter.
- **Raccourcis** du manifest (appui long sur l'icône) vers chaque niveau, via `?niveau=`.

Tout le reste est conforme : mêmes listes de nombres, même tirage (pool mélangé, distracteurs pris hors bonne réponse, ordre re-mélangé), mêmes bornes de réglages (questions 1→taille du pool, choix 2→6, chrono 3→60 s), même barème de résultat (90 / 70 / 50 %), chrono qui ne tourne que tant qu'on n'a pas répondu.

## Limites connues

- **Synthèse vocale** : voix `en-GB` si disponible, repli sur une autre voix anglaise. iOS exige une interaction utilisateur avant le premier `speak()` — c'est le cas ici, tout passe par un bouton. La vitesse est réglée à `0.8` (l'original utilisait `rate = 0.4`, soit un peu sous la vitesse normale d'AVSpeechUtterance).
- **Aucune persistance** : réglages et scores repartent de zéro à chaque lancement, comme dans l'app d'origine (le modèle SwiftData `Item` n'y était pas utilisé).
