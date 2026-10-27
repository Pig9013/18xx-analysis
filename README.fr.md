[中文](README.md) · [English](README.en.md) · **Français** · [日本語](README.ja.md) · [Русский](README.ru.md)

# Analyse de données de parties 18xx · Démarche et rapports

> Transformer des centaines de journaux publics de [18xx.games](https://18xx.games/) en rapports d'analyse approfondie **publiables et traçables au chiffre près**.
> Chaque tableau vient d'un dépouillement partie par partie et d'un rapprochement joueur par joueur ; chaque conclusion porte **effectif + fréquence + taux de victoire + numéro de cas** ; sans données, nous écrivons qu'il n'y en a pas.

**Version du dépôt : v1.0.2** (2026-10-25)

**Version actuelle**

| Composant | Version | Description |
|:--|:--|:--|
| skill `18xx-analysis` | **v1.4.1** | Processus en cinq étapes (règles → ingestion → analyse → statistiques → rapport) + 18 règles absolues |
| Rapports | 4 titres | 1846 (672 parties), 1830 (532 parties), 1862 (419 parties), 1880 China (341 parties) |

---

## 1. Catalogue des rapports

| Titre | Parties | Sessions joueurs | Taux de rapprochement | Version | Rapport principal | Autres livrables |
|:--|--:|--:|:--|:--|:--|:--|
| **1846** | 672 | 2,740 | **2740/2740 = 100.0%** | v1.1 | [Rapport principal (chinois)](<works/1846/1846深度分析（672局）.md>) | [Diaporama de restitution, 55 pages](<works/1846/1846深度分析（672局）.pptx>) · [Version anglaise](<works/1846/1846 Deep Analysis (672 Games).md>) · [Glossaire EN](<works/1846/1846_EN_GLOSSARY.md>) |
| **1830** | 532 | 532 | **532/532 = 100%** | v1.1 | [Rapport principal (chinois)](<works/1830/1830深度分析（532局）.md>) | — |
| **1862** | 419 | 1,572 | **1511/1572 = 96.1%** (résidu médian £203, expliqué) | v2.5 | [Rapport principal (chinois)](<works/1862/1862深度分析（419局）.md>) | [Aide-mémoire de table (une page)](<works/1862/1862实战速查（单页）.md>) |
| **1880 China** | 341 | 1,564 | **1564/1564 = 100.00%** (résidu 0) | v1.3 | [Rapport principal (chinois)](<works/1880China/1880China深度分析（341局）.md>) | [Diaporama de restitution, 48 pages](<works/1880China/1880China深度分析（341局）.pptx>) · [Version anglaise](<works/1880China/1880China Deep Analysis (341 Games).md>) · [Glossaire EN](<works/1880China/1880China_EN_GLOSSARY.md>) |

> Les 341 parties de 1880 sont **l'échantillon statistique principal** : 384 parties extraites de 6 fichiers sources → 358 après déduplication → 341 après retrait des 17 parties terminées par « vote manuel ».

---

## 2. Méthode (résumé)

| Étape | Contenu | Garde-fou |
|:--|:--|:--|
| **S0 Règles** | Lire les règles anglaises de l'ouvrage clause par clause et lister les neuf différences mécaniques avec les titres déjà analysés | Erreur de règle = tout est faux |
| **S1 Ingestion** | Découpage de trois types de sources (partie unique / recueil / sans en-tête de partie) ; **déduplication par sha1 du contenu** ; numérotation canonique + `_mapping.csv` | Échantillon unique après déduplication |
| **S2 Analyse** | Écrire une machine d'état comptable propre au titre et reconstruire trésorerie / participations / actif net joueur par joueur | **Rapprochement joueur par joueur avec le règlement officiel final** |
| **S3 Extraction** | Réutiliser la machine d'état parse, en observation seule et sans rien modifier, et produire les `_full.json` en lot | Taux de rapprochement inscrit en tête du rapport |
| **S4 Statistiques** | 14 scripts thématiques (composition des premiers trains, train rush, trésorerie, ordre de jeu, cassage des cours, classements…) | Chaque chiffre renvoie à une sortie de script |
| **S5 Rapport** | Squelette fixe : données et rapprochement → couche joueurs → mécaniques et comportements → aide-mémoire → méthode et limites | Liste d'auto-contrôle avant livraison |

**Quelques conventions fermes** :

- Classement « top two » : 1re place +2, 2e place +1, à partir de la 3e −(rank−2) ; seules les paires « top two vs. non top two » sont construites ;
- Rang normalisé =(rang−1)/(joueurs−1), **le rang moyen par partie ne se compare pas entre effectifs différents** ;
- Le taux de top two ne se calcule que sur les parties à ≥4 joueurs ; les cellules avec n<20 sont indicatives ;
- Les attributs aléatoires ne se reportent pas d'une partie à l'autre (en 1862, les licences d'exploitation sont réattribuées à chaque partie et doivent être ventilées en « société × licence ») ;
- Corrélation ≠ causalité ; chaque suspicion causale est listée à part.

Le processus complet et les 18 règles absolues figurent dans le [skill](<skills/18xx-analysis/SKILL.md>).

---

## 3. Anonymisation des joueurs

Cette exigence s'applique à **chaque livrable** (depuis le 2026-09-24) :

- **Le groupe TOP garde son vrai nom** (joueurs de haut niveau sur des parties publiques, pour préserver la traçabilité) ;
- **Les groupes MID / BOT sont toujours notés « PlayerX »** (`PlayerX` dans la version anglaise), numérotés par score de force décroissant, le segment MID avant le segment BOT ;
- Les bornes de strate sont propres à chaque titre (1846 selon la liste imprimée, 1880 selon BT≥2.0, 1830 selon BT≥1700/≤1420, 1862 selon BT≥1700) ;
- Avant livraison, lancer `anonymize_18xx.py --check` ; régénérer les diaporamas avec `build_*_ppt.py` (ne jamais modifier le XML à la main).

---

## 4. Sources des données et remerciements

**Toutes les données de partie viennent de [18xx.games](https://18xx.games/) et des joueurs qui y jouent** — ce sont leurs parties publiées gratuitement qui ont rendu ces analyses possibles. Sans ces journaux publics, ce projet n'aurait pas une seule partie.

- Les remerciements figurent en tête et en fin de chaque rapport ;
- Les journaux de partie bruts et les scripts d'analyse d'origine **ne sont pas dans ce dépôt** (taille et périmètre des données) ; chaque chiffre des rapports porte le script et la convention qui le produisent, et toute vérification peut passer par une Issue.

---

## 5. Droits d'auteur et utilisation

- **Textes d'analyse, graphiques, diaporamas** : création originale de Pig9013 ; **en cas de reprise, citer l'auteur et la source, usage commercial interdit** ;
- **Données de partie** : les droits appartiennent à 18xx.games et aux joueurs concernés ; ce dépôt ne les cite qu'à des fins statistiques et ne reproduit aucun journal complet ;
- Les citations de règles suivent la version anglaise d'origine ; la traduction chinoise ne sert que de contrôle, et toute erreur de traduction repérée est signalée dans le rapport.

---

## 6. Structure du dépôt

`@
skills/18xx-analysis/    SKILL.md + references/ (modèles, pièges de données, anonymisation, livraison des diaporamas…)
works/1846/              rapport principal + diaporama + version anglaise + glossaire EN
works/1830/              rapport principal
works/1862/              rapport principal + aide-mémoire de table
works/1880China/         rapport principal + diaporama + version anglaise + glossaire EN
tools/verify_repo.py     auto-contrôle statique avant publication
`@

---

*Analyse : Pig9013 ｜ Modèle : Deepseek-flash ｜ Dernière mise à jour : 2026-10-25*
