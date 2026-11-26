---
name: quiz-revision
description: Crée un quiz de révision interactif publié en artifact à partir des documents de cours fournis. À utiliser quand l'utilisateur partage des supports et demande un quiz, un QCM ou des questions pour réviser une matière.
---

# Quiz de révision à partir de documents

Objectif : un quiz qui fait vraiment réviser la matière des documents fournis, publié comme **artifact** (page HTML autonome) que l'utilisateur retrouve dans ses artifacts. Jamais seulement dans la discussion, et jamais avec le widget de quiz intégré au chat.

## Règles de base

- **Uniquement à partir des documents fournis.** S'il n'y a aucun document, demande-les. Ne génère pas de questions depuis ta mémoire d'un manuel.
- **Langue** : celle des documents (ou celle de l'utilisateur s'il le demande).
- **Nombre de questions** : demandé à l'utilisateur (voir « Choix du nombre de questions »). Maximum 100, sauf si l'utilisateur en demande explicitement plus. Au moins 3 questions par chapitre.
- **Découpage en chapitres/notions** : le quiz est toujours divisé en chapitres ou en notions. Reprends la structure des documents. S'ils n'en ont pas, regroupe toi-même les questions par notion (5 à 15 questions par groupe), avec des titres clairs.
- **Une question = une notion vérifiable dans le document**, avec sa source.

## Déroulé

1. **Lire tous les documents en entier** (utilise les skills de lecture de fichiers ou de PDF si besoin). Repère la structure : chapitres, sections, thèmes. Calcule le nombre de questions recommandé.
2. **Demander le nombre de questions** (voir ci-dessous), sauf si l'utilisateur l'a déjà donné.
3. **Écrire les questions** dans `quiz.json` (schéma ci-dessous), rangées par chapitre, en suivant les règles anti-devinette. Répartis-les entre les chapitres selon leur importance dans les documents.
4. **Lancer le script de vérification** (plus bas). Corrige et relance jusqu'à ce qu'il n'y ait plus d'erreur.
5. **Construire `index.html`** : une seule page autonome (HTML, CSS et JS dans le même fichier), le JSON intégré dans un `<script type="application/json" id="quiz-data">`.
6. **Tester** : vérifie la syntaxe du JS (`node --check` sur le script extrait). Si un navigateur est disponible, ouvre la page, lance directement un chapitre du milieu de la liste depuis le menu, puis fais une partie complète : répondre, passer, revenir en arrière, finir, refaire les erreurs, revenir au menu, recharger la page.
7. **Publier** avec l'outil Artifact (liste les types d'abord. Si aucun type ne correspond à un quiz, publie la page faite à la main). Pour une nouvelle version d'un quiz existant, republie le même artifact.
8. **Répondre** en une ou deux lignes : nombre de questions, chapitres couverts. Ne répète pas le lien, la carte de l'artifact l'affiche déjà.

## Choix du nombre de questions

**Calcul de la recommandation** : environ une question par notion importante, soit 2 à 4 par page de cours dense (moins pour des diapos très aérées). Garde ce nombre entre 10 et 100, et assure au moins 3 questions par chapitre.

**Question à poser** après la lecture des documents, avec l'outil de questions à choix (AskUserQuestion). Il accepte 4 options maximum, et un champ « Autre » est ajouté automatiquement pour taper un nombre libre.
- Question : « Combien de questions veux-tu dans le quiz ? », avec l'en-tête « Taille ».
- Option 1 : « Selon les documents : ~N (Recommandé) », où N est la recommandation calculée. Ajoute une description courte, par ex. « 4 chapitres, environ 30 pages ».
- Options 2 à 4 : les trois valeurs de la liste 10, 20, 40, 60, 80, 100 les plus proches de N, en excluant celle égale à N, dans l'ordre croissant.
- Le champ « Autre » sert pour n'importe quel autre nombre. Au-delà de 100, accepte si c'est le choix de l'utilisateur. Si le nombre est trop petit pour couvrir 3 questions par chapitre, dis-le en une ligne et propose de regrouper des chapitres.

**Cas particuliers**
- Si l'utilisateur a déjà donné un nombre dans sa demande, ne pose pas la question.
- Si l'outil de questions à choix n'est pas disponible, pose la question en texte avec la liste complète : selon les documents (~N), 10, 20, 40, 60, 80, 100, ou un autre nombre.
- Si personne ne peut répondre (tâche planifiée ou sans surveillance), prends la recommandation N et indique-le dans la réponse.
- Si le contenu est trop court pour le nombre demandé sans répéter les mêmes notions, fais le maximum de questions vraiment différentes et dis combien tu en as fait et pourquoi.

## Règles anti-devinette (essentielles)

La bonne réponse ne doit jamais se repérer à sa forme :

- **Longueurs équilibrées** : toutes les options d'une question ont à peu près la même longueur (écart max d'environ 20 %). Raccourcis la bonne réponse ou allonge les distracteurs. La bonne réponse ne doit pas être la plus longue plus souvent que le hasard ne le voudrait.
- **Même niveau de précision** : même vocabulaire technique, même nombre de chiffres ou de détails, même structure grammaticale. Pas de nuance (« en général », « souvent ») uniquement sur la bonne réponse.
- **Position équilibrée** : la bonne réponse est répartie de façon à peu près égale entre A, B, C et D sur l'ensemble du quiz.
- **Distracteurs plausibles** : des confusions réelles (une notion voisine du cours, une erreur de calcul fréquente, une inversion de cause et d'effet). Jamais de réponse absurde ou hors sujet.
- **Interdit** : « toutes les réponses ci-dessus », « aucune des réponses », les doubles négations, les indices grammaticaux (accord, article) qui trahissent la réponse.
- **Plusieurs bonnes réponses** : les options correctes et incorrectes ont la même longueur moyenne. Le nombre de bonnes réponses varie d'une question à l'autre.

## Types de questions

Varie les types (au moins 3 différents sur un quiz de 20 questions ou plus) :

- `single` : QCM, 4 options, une seule bonne.
- `multi` : plusieurs bonnes réponses (2 ou 3 sur 4-5). Le point n'est accordé que si la sélection est exacte, mais la correction montre option par option ce qui était juste.
- `truefalse` : Vrai/Faux, puis le choix d'une justification parmi 3. Juste seulement si les deux le sont.
- `match` : appariement de 3 à 5 paires (listes déroulantes).
- `order` : remettre 3 à 6 étapes dans l'ordre (boutons monter/descendre, affichage mélangé au départ).
- `numeric` : calcul ou valeur, avec une tolérance et une unité.

## Difficulté progressive

Chaque question a un niveau : `memorisation` (définition, fait), `comprehension` (expliquer, comparer, pourquoi) ou `application` (cas concret, calcul, situation nouvelle). Mélange visé : environ 30 % / 40 % / 30 %. Applique ce mélange à chaque chapitre autant que possible. Dans chaque chapitre, les questions vont de la plus simple à la plus difficile. Le niveau s'affiche en badge sur chaque question.

## Source de chaque question

Chaque question a une `source` : le nom du document, l'emplacement (page, diapo, section) et un court extrait (25 mots max) qui justifie la réponse. La correction affiche cette source sous l'explication.

## Schéma `quiz.json`

```json
{
  "id": "thermo-ch1-3",
  "title": "Thermodynamique — chapitres 1 à 3",
  "chapters": [{"id": "c1", "title": "Premier principe"}],
  "questions": [
    {
      "id": "q1", "type": "single", "chapter": "c1", "difficulty": "memorisation",
      "prompt": "...",
      "options": ["...", "...", "...", "..."],
      "answer": 2,
      "explanation": "Pourquoi la bonne est bonne, et pourquoi chaque piège est faux.",
      "source": {"doc": "cours_thermo.pdf", "location": "p. 12, §2.3", "excerpt": "..."}
    }
  ]
}
```

Format de `answer` selon le type :
- `single` : un indice.
- `multi` : une liste d'indices.
- `truefalse` : `"answer": true|false` plus `"justification": {"options": [...3], "answer": i}`.
- `match` : `"left": [...]`, `"right": [...]`, `"answer": [indice dans right pour chaque élément de left]`.
- `order` : `"items"` dans l'ordre correct (la page les mélange) et pas de champ `answer`.
- `numeric` : `"answer": 3.14`, `"tolerance": 0.01`, `"unit": "J"`.

L'explication doit dire pourquoi la bonne réponse est juste **et** pourquoi les pièges principaux sont faux.

Dans `questions`, range les questions par chapitre, dans l'ordre de `chapters`.

## Script de vérification

Écris-le dans un fichier séparé (par ex. `check_quiz.py`) et lance `python3 check_quiz.py quiz.json N`, où N est le nombre de questions choisi. Ne publie pas tant qu'il signale des erreurs.

```python
import json, sys, re, collections
d = json.load(open(sys.argv[1], encoding="utf-8"))
target = int(sys.argv[2]) if len(sys.argv) > 2 else None
Q = d["questions"]; errs = []; warns = []
BAN = re.compile(r"toutes les r[ée]ponses|aucune des r[ée]ponses|all of the above|none of the above", re.I)
chap_ids = {c["id"] for c in d["chapters"]}

def balanced(opts, qid, label="options"):
    L = [len(o) for o in opts]
    if max(L) > 1.2 * min(L) and max(L) - min(L) > 8:
        errs.append(f"{qid}: longueurs des {label} déséquilibrées {L}")
    for o in opts:
        if BAN.search(o): errs.append(f"{qid}: option interdite « {o} »")
    return L

seen = set(); pos = collections.Counter(); n_single = 0; longest = 0
for q in Q:
    i = q.get("id", "?")
    if i in seen: errs.append(f"{i}: id en double")
    seen.add(i)
    for k in ("type", "chapter", "difficulty", "prompt", "explanation", "source"):
        if not q.get(k): errs.append(f"{i}: champ '{k}' manquant")
    if q.get("chapter") not in chap_ids: errs.append(f"{i}: chapitre inconnu")
    if q.get("difficulty") not in ("memorisation", "comprehension", "application"):
        errs.append(f"{i}: difficulté invalide")
    s = q.get("source") or {}
    if not (s.get("doc") and s.get("location")): errs.append(f"{i}: source incomplète")
    if len((s.get("excerpt") or "").split()) > 25: warns.append(f"{i}: extrait > 25 mots")
    t = q.get("type")
    if t == "single":
        L = balanced(q["options"], i); a = q["answer"]
        n_single += 1; pos[a] += 1
        if L[a] == max(L) and L.count(max(L)) == 1: longest += 1
    elif t == "multi":
        L = balanced(q["options"], i); good = set(q["answer"])
        g = [L[k] for k in good]; b = [L[k] for k in range(len(L)) if k not in good]
        if not b or not g: errs.append(f"{i}: multi sans bonne ou sans mauvaise option")
        elif sum(g)/len(g) > 1.15 * sum(b)/len(b): errs.append(f"{i}: bonnes options plus longues en moyenne")
    elif t == "truefalse":
        balanced(q["justification"]["options"], i, "justifications")
    elif t == "numeric":
        if "tolerance" not in q: errs.append(f"{i}: tolérance manquante")
    elif t == "match":
        if sorted(q["answer"]) != list(range(len(q["right"]))): errs.append(f"{i}: appariement invalide")
    elif t != "order":
        errs.append(f"{i}: type inconnu {t}")

if n_single >= 8:
    if longest / n_single > 0.35:
        errs.append(f"bonne réponse la plus longue dans {longest}/{n_single} QCM")
    for p in range(4):
        if abs(pos[p] - n_single / 4) > max(2, 0.15 * n_single):
            errs.append(f"position {'ABCD'[p]} : {pos[p]}/{n_single}, répartition déséquilibrée")
types = {q["type"] for q in Q}
if len(Q) >= 20 and len(types) < 3: warns.append(f"seulement {len(types)} types de questions")
per_chap = collections.Counter(q["chapter"] for q in Q)
for c in chap_ids:
    if per_chap[c] < 3: warns.append(f"chapitre {c} : {per_chap[c]} question(s)")
idx = {c["id"]: k for k, c in enumerate(d["chapters"])}
order = [q.get("chapter") for q in Q]
if order != sorted(order, key=lambda c: idx.get(c, 999)):
    errs.append("questions non rangées par chapitre")
if target is not None and len(Q) != target:
    warns.append(f"{len(Q)} questions au lieu des {target} choisies : explique l'écart dans la réponse")
if len(Q) > 100 and (target is None or target <= 100):
    warns.append(f"{len(Q)} questions : au-delà de 100 seulement si l'utilisateur l'a demandé")
diff = collections.Counter(q["difficulty"] for q in Q)
print("Questions :", len(Q), "| Par chapitre :", dict(per_chap))
print("Difficultés :", dict(diff), "| Types :", sorted(types), "| Positions :", dict(pos))
for w in warns: print("AVERTISSEMENT", w)
for e in errs: print("ERREUR", e)
sys.exit(1 if errs else 0)
```

## Fonctionnement de la page

**Écran d'accueil : menu des chapitres**
- La liste de tous les chapitres ou notions. Pour chacun : son nombre de questions, sa progression (répondues / total), son meilleur score et son état (pas commencé, en cours, terminé).
- Un clic sur un chapitre le lance directement, sans avoir fait les autres. Aucun chapitre n'est verrouillé.
- Un bouton **Tout le quiz** enchaîne tous les chapitres dans l'ordre.
- Un bouton **Mes erreurs (tous chapitres)** apparaît dès qu'il existe des erreurs enregistrées.
- Pendant une session, un bouton **Menu** ramène à cet écran à tout moment. La session du chapitre est conservée et reprend où elle s'était arrêtée.

**Pendant le quiz**
- En-tête : titre du chapitre, barre de progression (« Question 7/20 »), score en direct (bonnes / répondues).
- Sur chaque question : badge du chapitre et badge du niveau.
- **Valider** affiche aussitôt la correction : juste ou faux, la bonne réponse mise en évidence (et l'erreur de l'utilisateur en rouge), l'explication, puis la source (document, emplacement, extrait).
- **Précédent** : revenir à une question déjà vue. Si elle a déjà été répondue, elle s'affiche avec sa réponse et sa correction, sans pouvoir la modifier.
- **Passer** : la question va dans une file « passées ». Une fois la dernière question atteinte, les questions passées sont reproposées avant les résultats. Un compteur indique combien il en reste.
- **Suivant** : disponible après validation.

**Résultats**
- Score de la session en nombre et en pourcentage.
- **Bilan par chapitre** (calculé sur les derniers résultats de chaque chapitre, même s'ils ont été faits séparément) : un tableau chapitre / bonnes / total / %, trié du plus faible au plus fort, avec les chapitres faibles (moins de 60 %) mis en évidence. Les chapitres pas encore faits sont indiqués comme tels.
- Bilan par niveau de difficulté.
- La liste des questions ratées ou non répondues, chacune avec sa correction et sa source.
- Boutons **Refaire uniquement mes erreurs** (une nouvelle session avec les questions fausses et laissées sans réponse de la session qui vient de se terminer, même fonctionnement), **Recommencer ce chapitre** (ou tout le quiz) et **Retour au menu des chapitres**.

**Progression sauvegardée**
- Chaque chapitre a sa propre session (question courante, réponses, file des passées, mode « tout » ou « erreurs »), enregistrée dans `localStorage` sous la clé `quiz-<id>` après chaque action. Les derniers résultats de chaque chapitre y sont enregistrés aussi.
- Dans le menu, un chapitre en cours propose **Reprendre** ou **Recommencer**.
- Un **historique des tentatives** (date, chapitre ou quiz entier, mode, score) s'affiche sur l'écran d'accueil et sur l'écran des résultats.
- Mets toutes les lectures et écritures dans un `try/catch` : si le stockage n'est pas disponible, le quiz fonctionne quand même, sans sauvegarde, avec un petit message discret.

**Présentation**
- Page claire et lisible, adaptée au mobile, avec de grandes zones cliquables. Des couleurs vert et rouge pour la correction, toujours accompagnées d'une icône ou d'un texte (pas seulement la couleur).
- Tout en un seul fichier, sans dépendance externe.
