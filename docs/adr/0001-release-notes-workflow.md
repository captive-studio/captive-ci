# Workflow réutilisable pour les notes de release GitHub

Plusieurs repos Captive (`cae/serveur`, `cae/application`) commençaient à dupliquer le même job GitHub Actions pour générer les notes de release sur push de tag `v*` (`softprops/action-gh-release`, `generate_release_notes: true`). `captive-ci` centralise déjà les workflows de CI réutilisables (`ruby-ci.yml`, `rails-ci.yml`) ; on y ajoute `release.yml` sur le même modèle (`workflow_call`, sans input requis), appelé via `uses: captive-studio/captive-ci/.github/workflows/release.yml@main` depuis un `release.yml` local déclenché sur `push: tags: v*`.

Le bump de version (fichier `VERSION`, `package.json`...) et la création du tag restent gérés localement à chaque repo (`script/release`) : ce sont des conventions propres à chaque stack, pas un besoin de CI partagée.
