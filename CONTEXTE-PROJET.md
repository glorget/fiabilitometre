# Contexte du projet Fiabilitomètre

## Objectif du projet
Outil pédagogique interactif (HTML/JS statique) permettant aux élèves d'évaluer
la fiabilité scientifique d'une publication à partir d'une grille de 10 critères.
Hébergé sur GitHub Pages : https://glorget.github.io/fiabilitometre/
Dépôt : github.com/glorget/fiabilitometre (public)
Licence : CC BY-NC 4.0

## Origine
Adapté d'une ressource d'Anne Merlin (SVT) et Mélanie Serret (documentaliste),
académie de Lille (TraAM EMI 2025-2026). Développé et codé avec Claude (Anthropic).

## Structure actuelle du dépôt
- `index.html` — l'application élève (interface + logique), ~1170 lignes,
  fichier unique autonome, aucune dépendance externe sauf Google Fonts
- `editor.html` — éditeur en ligne pour les enseignants collaborateurs du dépôt
  (ajout/modification de thèmes et publications dans `data.json`, avec
  assistance IA Gemini), fichier unique autonome, même conventions
- `data.json` — toutes les données (thèmes, publications, réponses, feedbacks)
- `README.md` — documentation

## Architecture de `index.html`

### Palette de couleurs (variables CSS)
```css
--rouge: #C0392B;       --rouge-bg: #FDEDEC;
--vert: #27AE60;        --vert-bg: #EAFAF1;
--texte: #1A1A1A;       --texte-sec: #666666;
--violet: #7B1FA2;      --violet-light: #F3E5F5;
```
Couleur anthracite du logo/groupe niveau : `#2F383F`
Couleur "Collège/Lycée" (groupe niveau) : terracotta `#8B4A3A` / `#c8a99a`
Couleur neutre N/E : texte `#2F383F`, fond `#818c8e`
Polices : 'Source Serif 4' (titres), 'DM Sans' (corps)

### Header
- Logo : chat noir et blanc avec loupe, image GIF encodée en base64 inline
  (`<img src="data:image/gif;base64,...">`), 128×128px
- Titre "Fiabilitomètre" (32px, rouge) + sous-titre (15px, gris)
- Encadré crédit à droite (1/3 de la largeur du header), bordure et texte
  `#818c8e`, interligne 1.15, contient un lien vers la ressource académie de Lille

### Flux de sélection en cascade (dans cet ordre)
1. **Niveau(x)** — pastilles cliquables multi-sélection (checkboxes stylées en pilules)
   - Deux pastilles "groupe" : "Collège" (coche auto 6e/5e/4e/3e) et "Lycée"
     (coche auto Seconde/Première/Terminale), séparées par une barre verticale fine
   - Pastilles individuelles : 6e, 5e, 4e, 3e, Seconde, Première, Terminale
   - Cocher/décocher un niveau individuel met à jour l'état de la pastille groupe
2. **Matière** — menu déroulant qui n'apparaît qu'après avoir choisi au moins un niveau
   - Options filtrées dynamiquement selon les niveaux cochés (voir
     `MATIERES_PAR_NIVEAU` en JS, liste complète des matières officielles
     collège/lycée français), triées alphabétiquement, "Toutes matières" toujours en dernier
   - Mapping labels → valeurs internes dans `MATIERE_VALUES` (ex: "Sciences de la
     vie et de la Terre (SVT)" → valeur "SVT")
3. **Thèmes disponibles** — encadré blanc (`.themes-preview`) sous le bloc de
   sélection, liste à puces en italique gris clair des thèmes correspondant à
   la combinaison niveau+matière choisie, max-height ~200px avec scroll si
   plus de 8 lignes. Disparaît dès qu'une publication est sélectionnée.
4. **Thème à analyser** — menu déroulant, filtré par niveau+matière
5. **Publication à évaluer** — menu déroulant, dépend du thème choisi
   - Affiche un lien "Ouvrir la publication" vers l'URL source

### Zone d'évaluation (après sélection d'une publication)
- 10 critères répartis en 2 sections : "A. L'auteur et la source de la
  publication" (critères 1-4) et "B. Le contenu et les sources primaires"
  (critères 5-10)
- Chaque critère : texte + 3 boutons VRAI / FAUX / N/E (Non Évaluable)
- Légende en haut expliquant VRAI/FAUX/N/E sur une ligne
- Bouton "Agrandir le texte" (𝐀+/𝐀−) en haut à droite sous le sélecteur,
  bascule la taille des critères entre 18px et 25px via variable CSS
  `--criterion-size`
- Bouton "Calculer mon score" : si un critère est sans réponse, affiche un
  encadré neutre `#2F383F` "Répondez à tous les critères" SANS afficher aucun
  feedback individuel. Sinon, calcule et affiche :
  - Feedback par critère (vert=correct, rouge=erreur, gris=N/E non évaluable
    légitime). N/E sur un critère qui a une réponse attendue compte comme UNE ERREUR.
  - Score de fiabilité de LA PUBLICATION (pas la performance de l'élève) =
    nombre de critères "VRAI" dans les réponses attendues (`currentPub.answers`),
    affiché dans un encadré coloré selon l'échelle SCALE_COLORS (rouge→violet)
  - Ligne séparée : performance de l'élève ("Ton évaluation : X critère(s)
    correct(s) · Y erreur(s)")
  - Note pédagogique (`scoreNote`) uniquement si au moins une réponse donnée
- Échelle de confiance visuelle en bas : case "PUB NON ÉVAL." (15% largeur,
  fond `#2F383F`) + barre 1 à 10 (85% largeur, dégradé rouge→violet), avec
  légendes "très faible confiance" / "très forte confiance scientifique"
  (2 lignes, aligné à droite)
- Bouton "Recommencer"

## Structure de `data.json`
```json
{
  "themes": [
    {
      "id": "slug-unique",
      "label": "Nom affiché",
      "matiere": "SVT",              // valeur interne (voir MATIERE_VALUES)
      "niveaux": ["4e", "3e", "Seconde", ...],  // niveaux concernés
      "publications": [
        {
          "id": "slug-unique",
          "label": "Publication N — Nom de la source",
          "url": "https://...",
          "answers": [true, false, ...],  // 10 booléens, réponse attendue par critère
          "scoreExpected": 9,              // = nombre de "true" dans answers (informatif)
          "scoreNote": "Explication pédagogique du score, sans révéler les réponses",
          "feedbackTrue": { "0": "texte si critère 0 correct", ... },  // clés = index string "0"-"9"
          "feedbackFalse": { "0": "texte si critère 0 incorrect", ... }
        }
      ]
    }
  ]
}
```
Actuellement 2 thèmes, 6 publications au total (soutien-gorge/cancer du sein
avec 3 pub, boissons énergisantes avec 3 pub), tous en SVT.

Les 10 critères eux-mêmes (texte des questions) sont codés en dur dans le JS
(`const CRITERIA = [...]`) et communs à toutes les publications — ils ne sont
PAS dans data.json.

## Éditeur en ligne (`editor.html`)
Outil d'édition en ligne pour que des enseignants collaborateurs du dépôt
puissent ajouter/modifier des thèmes et publications dans `data.json` sans
éditer le JSON à la main. Implémenté avec :
- Authentification par token GitHub personnel (PAT fine-grained, scope
  `Contents: Read and write` limité à ce seul dépôt) par utilisateur — pas de
  mot de passe partagé ; GitHub attribue automatiquement les commits à
  l'auteur du token (traçabilité) et refuse l'écriture si le token n'a pas les
  droits — aucune logique d'autorisation custom n'a été nécessaire
- Formulaire : matière, niveau(x), thème (nouveau ou existant), publication
  (nouvelle ou modification d'une publication existante), URL de la source
- Appel à l'**API Gemini** (Google) — pas Claude — pour lire la publication à
  l'URL donnée et proposer automatiquement les 10 réponses VRAI/FAUX/N/E +
  feedbacks + scoreNote. Décision prise en cours de projet : les enseignants
  de l'établissement ont un accès gratuit à Gemini via leur compte Google
  Workspace. Chaque enseignant utilise sa propre clé API Gemini
  (aistudio.google.com), stockée uniquement dans son navigateur — zéro
  backend, l'outil URL context de l'API Gemini fait lire l'URL par les
  serveurs Google eux-mêmes (pas de blocage CORS côté navigateur). Si l'accès
  Gemini s'avère bloqué par l'admin Workspace de l'établissement, prévoir un
  petit proxy serverless (Cloudflare Worker/Vercel) tenant une clé partagée —
  non construit, à ajouter seulement si nécessaire
- L'enseignant valide/corrige la proposition avant sauvegarde (jamais
  d'écriture automatique sans relecture humaine)
- Écriture directe dans `data.json` sur GitHub via l'API GitHub (pas de
  passage par upload manuel), avec gestion du conflit d'édition concurrente
  (sha mismatch → rechargement + nouvelle tentative)
- Le propriétaire du dépôt (glorget) est notifié des changements via les
  notifications GitHub standard (Watch → All activity), déjà en place

⚠️ Point à vérifier empiriquement avec une vraie clé Gemini avant usage en
production : le nom exact du champ activant l'outil URL context sur
`generateContent` (candidat utilisé dans le code : `tools: [{ url_context: {} }]`,
non confirmé avec certitude par la documentation consultée — voir commentaire
dans `editor.html` à côté de `buildGeminiRequestBody`).

Décision prise : option "B" — passage par API GitHub avec token individuel
par utilisateur plutôt qu'un éditeur local ou un backend serveur dédié.
