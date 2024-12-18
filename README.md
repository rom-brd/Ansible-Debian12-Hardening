# 🚀 Ansible Server Setup - Debian 12
Un projet Ansible pour configurer rapidement et sécuriser un serveur Debian 12 avec Docker !

## 🎯 Fonctionnalités
- ⚙️ Configuration système de base (paquets, utilisateurs, shell zsh)
- 🔒 Hardening complet (SSH, fail2ban, UFW, auditd...)
- 🐳 Installation et configuration de Docker
- 📝 Gestion des logs (logrotate)
- 👥 Gestion multi-utilisateurs avec clés SSH

## ⚠️ Prérequis
- Ansible 2.9+
- Serveur Debian 12 fraîchement installé avec :
  - Un utilisateur non-root
  - Un utilisateur root
  - OpenSSH installé (`apt install openssh-server`)

## 🏃 Démarrage rapide
1. Clonez le projet

2. Configurez vos serveurs dans `inventory/hosts`:
```ini
[debian_servers]
monserver ansible_host=X.X.X.X ansible_user=monuser
```

3. Configurez l'accès SSH au serveur :
```bash
# Générer une paire de clés SSH si vous n'en avez pas
ssh-keygen -t ed25519

# Copier votre clé publique sur le serveur
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@X.X.X.X
```

4. Configurez vos utilisateurs dans `inventory/group_vars/all.yml`:
```yaml
users:
  - username: monutilisateur
    groups: ['sudo']
    shell: /bin/zsh
    ssh_keys:
      - VOTRE_CLE_SSH
```

5. Lancez le playbook:
```bash
# Installation complète
ansible-playbook playbooks/site.yml --ask-become-pass

# Installation sélective avec tags
ansible-playbook playbooks/site.yml --ask-become-pass --tags "common,docker"  # Juste common et docker
ansible-playbook playbooks/site.yml --ask-become-pass --tags security         # Juste la sécurité
```

## 🏷️ Tags disponibles
- `common`: Configuration de base
- `docker`: Installation Docker
- `security`: Hardening serveur
- Plus spécifique: `ssh`, `fail2ban`, `ufw`, `users`, `packages`...

## 🤝 Contact
Des questions ? Des suggestions ? N'hésitez pas à ouvrir une issue !

Happy deploying! 🎉
