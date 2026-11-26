# quiz-revision — Skill Claude

🇫🇷 [Français](#français) · 🇬🇧 [English](#english)

---

## Français

Un skill pour [Claude](https://claude.ai) qui transforme vos documents de cours (PDF, notes, diapos…) en **quiz de révision interactif**, publié comme page web que vous retrouvez dans vos artifacts.

### Ce que fait le quiz

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

### Des réponses qu'on ne devine pas

Un script de vérification bloque la publication tant que :
- les réponses d'une question n'ont pas des longueurs proches (écart max d'environ 20 %) ;
- la bonne réponse est la plus longue trop souvent ;
- la bonne réponse n'est pas répartie équitablement entre A, B, C et D ;
- il reste des options du type « toutes les réponses ci-dessus ».

### Installation

**Dans Claude (web ou application)**
1. Téléchargez ce dépôt (bouton **Code > Download ZIP**), ou seulement le dossier `quiz-revision/` compressé en `.zip`.
2. Dans Claude : **Customize > Skills > + > Upload a skill**, puis choisissez le `.zip`.
3. Les skills nécessitent l'exécution de code et la création de fichiers activées.

**Dans Claude Code**
```bash
git clone https://github.com/beurrexsale/quiz-revision-skill.git
cp -r quiz-revision-skill/quiz-revision ~/.claude/skills/
```

### Utilisation

Envoyez vos documents de cours et demandez par exemple :
> Fais-moi un quiz pour réviser ce cours.

Claude lit les documents, vous demande le nombre de questions, puis publie le quiz.

---

## English

A [Claude](https://claude.ai) skill that turns your course documents (PDFs, notes, slides…) into an **interactive revision quiz**, published as a web page you can find in your artifacts.

> The skill's instructions are written in French, but the quiz is generated in the language of your documents.

### What the quiz does

- **Questions drawn only from your documents**, each with its source (document, page, short excerpt).
- **Split by chapter or topic**, with a menu to start any chapter directly.
- **Feedback after every answer**: the correct answer, an explanation and the source.
- **Live score**, go back to previous questions, **skip a question** and come back to it at the end.
- **Retry only your mistakes**, per chapter or across the whole quiz.
- **Results by chapter and by difficulty level**, to spot weak areas.
- **Progress saved** in the browser, with a history of attempts.
- **Varied question types**: single choice, multiple answers, true/false with justification, matching, ordering, numeric.
- **Progressive difficulty**: recall, understanding, application.
- **Size of your choice**: Claude suggests a number based on your documents, or 10, 20, 40, 60, 80, 100, or any other number.

### Answers you can't guess

A check script blocks publishing as long as:
- a question's options are not of similar length (about 20% max difference);
- the correct answer is the longest one too often;
- correct answers are not spread evenly across A, B, C and D;
- options like "all of the above" remain.

### Installation

**In Claude (web or app)**
1. Download this repository (**Code > Download ZIP**), or just the `quiz-revision/` folder zipped.
2. In Claude: **Customize > Skills > + > Upload a skill**, then choose the `.zip`.
3. Skills require code execution and file creation to be enabled.

**In Claude Code**
```bash
git clone https://github.com/beurrexsale/quiz-revision-skill.git
cp -r quiz-revision-skill/quiz-revision ~/.claude/skills/
```

### Usage

Share your course documents and ask, for example:
> Make me a quiz to revise this course.

Claude reads the documents, asks how many questions you want, then publishes the quiz.
