# Qualité du code Expo bloquée par un score minimum

`rails-ci` fait échouer la CI quand le score RubyCritic passe sous le `minimum_score` du dépôt. On veut le même contrôle côté Expo avec code-multivitals, l'outil que le Scan de captive-platform utilise déjà pour le JavaScript/TypeScript. Or code-multivitals n'a pas de score minimum : il échoue dès qu'une seule fonction dépasse un seuil (`--max-errors`, `--max-warnings`, `--max-clones`). Sur un code existant, ce critère est soit rouge en permanence, soit désactivé par un plafond arbitraire.

`expo-ci` lance donc `quality-command`, **ignore son code de sortie** et lit le rapport JSON (`code-multivitals.json`) : la CI échoue si `averageHealthScore` passe sous `minimumHealthScore`, une clé de `.code-multivitals.json` que code-multivitals ne lit pas. C'est le Health Score que le Scan affiche. Comme dans `rails-ci`, la CI refuse de passer sans seuil déclaré, et l'absence de rapport fait échouer le job pour qu'un outil planté ne passe pas pour du code sain. L'étape reste facultative (`quality-command` vide par défaut), pour ne pas rendre rouges les dépôts qui n'ont pas encore de seuil.

Alternative écartée : une baseline (`--baseline`), qui ne bloque que les régressions mais oblige à versionner et régénérer un fichier d'état.
