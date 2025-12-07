# Déploiement via action Gitea (SSH)

Ce document explique comment configurer une action Gitea qui se connecte à un serveur distant et exécute `git pull` dans un répertoire.

## Fichiers créés

- `.gitea/workflows/deploy.yml` — workflow qui se déclenche sur `push` vers `main` et exécute un `ssh` pour faire `git pull`.

## Secrets attendus (à ajouter dans les paramètres du dépôt Gitea)

- `SSH_PRIVATE_KEY` : contenu de la clé privée (PEM) générée localement (sans passphrase de préférence pour l'automatisation).
- `SSH_HOST` : nom d'hôte ou IP du serveur distant (ex: `example.com` ou `192.0.2.10`).
- `SSH_USER` : utilisateur SSH utilisé pour se connecter (ex: `deploy`).
- `REMOTE_PATH` : chemin absolu sur le serveur où se trouve le dépôt (ex: `/var/www/myapp`).

## Étapes pour préparer le serveur et les secrets

1. Générer une paire de clés SSH (sur votre machine de développement) :

   Sous Linux / macOS / Git Bash :

   ```bash
   ssh-keygen -t rsa -b 4096 -C "deploy@myrepo" -f ~/.ssh/gitea_deploy_key
   # vous pouvez laisser la passphrase vide pour automatiser
   ```

   Sous Windows (cmd.exe avec OpenSSH installé) :

   ```cmd
   ssh-keygen -t rsa -b 4096 -C "deploy@myrepo" -f %USERPROFILE%\.ssh\gitea_deploy_key
   ```

2. Copier la clé publique (`gitea_deploy_key.pub`) sur le serveur distant, dans le fichier `~/.ssh/authorized_keys` de l'utilisateur `SSH_USER` :

   ```bash
   # exemple depuis votre machine
   ssh-copy-id -i ~/.ssh/gitea_deploy_key.pub deploy@your-server.example.com
   ```

   Si `ssh-copy-id` n'est pas disponible, copiez manuellement le contenu du fichier `.pub` dans `~/.ssh/authorized_keys` et vérifiez les permissions :

   ```bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   echo "<contenu-de-la-clé-pub>" >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

3. Dans l'interface Gitea du dépôt, ajoutez les secrets indiqués plus haut. Collez le contenu complet de `gitea_deploy_key` (clé privée) dans `SSH_PRIVATE_KEY`.

## Remarques de sécurité

- Préférez une clé dédiée pour l'action (ne pas réutiliser une clé personnelle).
- Si possible, restreignez la clé sur le serveur (ex: via des restrictions dans `authorized_keys` comme `command="/bin/false"` ou des contrôles IP) selon vos besoins.
- Au premier déploiement, si `known_hosts` n'est pas rempli, le workflow ajoute l'hôte via `ssh-keyscan`.

## Tester localement avant de pousser

Depuis votre machine (remplacez les chemins/valeurs) :

```bash
ssh -i ~/.ssh/gitea_deploy_key deploy@your-server.example.com "cd /var/www/myapp && git pull"
```

Sous Windows cmd.exe (avec OpenSSH) :

```cmd
ssh -i %USERPROFILE%\.ssh\gitea_deploy_key deploy@your-server.example.com "cd /var/www/myapp && git pull"
```

Si la commande fonctionne en local, le workflow Gitea devrait aussi fonctionner si les secrets sont correctement configurés.

## Personnalisation

- Branche déclenchante : par défaut `main`. Modifiez `.gitea/workflows/deploy.yml` si vous voulez un autre branche ou un déclencheur manuel.
- Vous pouvez remplacer `ssh-keyscan` et `StrictHostKeyChecking=yes` par `no` temporairement si vous avez des problèmes de host key (mais évitez pour la production).

## Utilisation d'un runner auto-hébergé (self-hosted)

Si vous avez enregistré un runner self-hosted (comme dans votre exemple `deployServe`), adaptez le workflow pour cibler les labels du runner. Dans l'exemple fourni, le runner a les labels `ubuntu` et `master` et le workflow utilise :

```
runs-on: [self-hosted, ubuntu, master]
```

Vérifiez :

- Dans la machine où le runner est installé, le service doit être en cours d'exécution et connecté à l'instance Gitea.
- Les labels doivent correspondre exactement (espace et casse) à ceux déclarés lors de l'enregistrement du runner.
- Le runner doit disposer des outils nécessaires (ssh, git). Sur un runner Debian/Ubuntu, installez :

```bash
sudo apt update && sudo apt install -y openssh-client git
```

Si le runner est en Windows ou une autre distribution, adaptez l'installation des dépendances.

Pour dépanner, regardez les logs du runner sur la machine hébergeant le runner et assurez-vous que le runner affiche l'état "online" dans l'interface Gitea.

## Dépannage rapide

- Permission denied (publickey) : vérifiez que la clé publique est bien dans `authorized_keys` pour l'utilisateur correct.
- Host key verification failed : assurez-vous que l'hôte est dans `known_hosts` ou autorisez `StrictHostKeyChecking=no` temporairement.
- `git pull` échoue pour cause d'authentification côté serveur : vérifiez que le répertoire distant est bien un dépôt Git et que l'utilisateur a les droits nécessaires.

---

Si vous voulez, je peux :
- adapter le workflow pour exiger une branche différente, ou faire un `fetch && git reset --hard origin/<branch>` au lieu de `git pull`,
- ajouter une étape de backup avant le pull,
- fournir une variante qui utilise `rsync`/déploiement atomique.

Dites-moi ce que vous préférez.