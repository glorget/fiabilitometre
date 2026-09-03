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

Le projet repose sur deux fichiers :

- `index.html` — l'interface interactive (moteur de l'application, ne nécessite pas de modification)
- `data.json` — les contenus : thèmes, publications, réponses attendues et feedbacks

Pour **ajouter un nouveau thème ou une nouvelle publication**, il suffit d'éditer `data.json` 
directement sur GitHub, sans toucher à `index.html`.

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
