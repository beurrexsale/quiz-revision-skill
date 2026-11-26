# quiz-revision — Skill Claude

Un skill pour [Claude](https://claude.ai) qui transforme vos documents de cours (PDF, notes, diapos…) en **quiz de révision interactif**, publié comme page web que vous retrouvez dans vos artifacts.

## Ce que fait le quiz

- **Questions tirées uniquement de vos documents**, chacune avec sa source (document, page, court extrait).
- **Découpage par chapitres ou notions**, avec un menu pour lancer n'importe quel chapitre directement.
- **Correction après chaque réponse** : la bonne réponse, une explication et la source.
- **Score en direct**, retour aux questions précédentes, possibilité de **passer une question** et d'y revenir à la fin.
- **Refaire uniquement ses erreurs**, par chapitre ou sur tout le quiz.
- **Bilan par chapitre et par niveau de difficulté**, pour repérer ses points faibles.
- **Progression sauvegardée** dans le navigateur, avec un historique des tentatives.
- **Types de questions variés** : QCM, plusieurs bonnes réponses, vrai/faux justifié, appariement, remise en ordre, calcul.
- **Difficulté progressive** : mémorisation, compréhension, application.
- **Taille au choix** : Claude propose un nombre adapté à vos documents, ou 10, 20, 40, 60, 80, 100, ou un autre nombre.

## Des réponses qu'on ne devine pas

Un script de vérification bloque la publication tant que :
- les réponses d'une question n'ont pas des longueurs proches (écart max d'environ 20 %) ;
- la bonne réponse est la plus longue trop souvent ;
- la bonne réponse n'est pas répartie équitablement entre A, B, C et D ;
- il reste des options du type « toutes les réponses ci-dessus ».

## Installation

**Dans Claude (web ou application)**
1. Téléchargez ce dépôt (bouton **Code > Download ZIP**), ou seulement le dossier `quiz-revision/` compressé en `.zip`.
2. Dans Claude : **Customize > Skills > + > Upload a skill**, puis choisissez le `.zip`.
3. Les skills nécessitent l'exécution de code et la création de fichiers activées.

**Dans Claude Code**
```bash
git clone https://github.com/beurrexsale/quiz-revision-skill.git
cp -r quiz-revision-skill/quiz-revision ~/.claude/skills/
```

## Utilisation

Envoyez vos documents de cours et demandez par exemple :
> Fais-moi un quiz pour réviser ce cours.

Claude lit les documents, vous demande le nombre de questions, puis publie le quiz.

