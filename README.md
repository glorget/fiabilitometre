# Fiabilitomètre

Outil interactif d'évaluation de la fiabilité scientifique d'une publication, 
à destination des élèves de collège (cycle 4, 5ème–3ème), en lien avec les attendus 
du projet de programme SVT cycle 4 (juillet 2025) sur le développement de l'esprit 
critique et la capacité à distinguer savoir scientifique éprouvé, opinion et croyance.
Utilisable avec d'autres niveaux et dans d'autres disciplines.

L'élève complète une évaluation de chaque source en dix questions et un score de fiabilité 
est alors calculé. Il peut être comparé avec le score attribué préalablement par le professeur 
qui a fourni la source.

## Utilisation

Ouvrir `index.html` dans un navigateur, ou accéder directement à la page en ligne :  
👉 https://glorget.github.io/fiabilitometre/

## Structure

Le projet repose sur trois fichiers :

- `index.html` — l'interface interactive côté élève (moteur de l'application, ne nécessite pas de modification)
- `editor.html` — l'éditeur en ligne côté enseignant (voir ci-dessous)
- `data.json` — les contenus : thèmes, publications, réponses attendues et feedbacks

Pour **ajouter un nouveau thème ou une nouvelle publication**, deux options : éditer `data.json` 
directement sur GitHub à la main, ou utiliser l'éditeur en ligne (voir ci-dessous), sans jamais 
toucher à `index.html`.

## Éditeur en ligne (pour les enseignants collaborateurs du dépôt)

👉 https://glorget.github.io/fiabilitometre/editor.html

Permet d'ajouter ou modifier un thème/une publication sans éditer le JSON à la main, avec une 
assistance IA (Gemini) qui propose automatiquement les 10 réponses et feedbacks à partir de 
l'URL de la source — à relire et corriger avant sauvegarde. Nécessite :

- Un accès en écriture au dépôt sur GitHub (être ajouté comme collaborateur), puis un token 
  d'accès personnel (PAT) — les instructions de création exactes sont dans l'aide en ligne 
  de la page elle-même.
- Optionnellement, une clé API Gemini gratuite (aistudio.google.com) pour l'assistance IA — 
  sans clé, le formulaire reste utilisable, à remplir manuellement.

Le token et la clé restent uniquement dans le navigateur de l'enseignant, jamais transmis 
ailleurs qu'à GitHub et Google.

La sélection d'un thème se fait par filtrage multi-critères : matière, niveau, puis thème.
Les matières disponibles sont : SVT, SNT, Histoire-Géographie, Physique-Chimie, Français, EMI,
ainsi qu'une option "Toutes matières" pour les thèmes transversaux. Les niveaux couvrent 
la 6e jusqu'à la Terminale, avec une option "Tous niveaux".

## Thèmes disponibles

- Soutien-gorge et cancer du sein — SVT, 4e–Terminale  
- Boissons énergisantes et santé — SVT, 4e–Terminale

## Origine

Adapté et développé à partir de la ressource **Fiabilitomètre** élaborée par Anne Merlin, 
professeure de SVT, et Mélanie Serret, professeure documentaliste, au lycée Giraux Sannier 
(Saint-Martin-Boulogne), dans le cadre d'un projet TraAM EMI 2025-2026, publiée sur le site 
de l'académie de Lille :  
https://pedagogie.ac-lille.fr/prof-doc/fiabilito/

Signalée par Nicolas Louisot, EF2D Amérique du Nord, 2026.  
Codage par Claude (Anthropic).

## Licence

© 2026. Ce contenu est sous licence [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/).  
Réutilisation et adaptation autorisées avec attribution, sans usage commercial.
