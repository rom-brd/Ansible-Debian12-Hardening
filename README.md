# 🚀 Ansible Server Setup - Debian 12

Un projet Ansible pour configurer rapidement et sécuriser un serveur Debian 12 avec Docker !

## 🎯 Fonctionnalités

- ⚙️ Configuration système de base (paquets, utilisateurs, shell zsh)
- 🔒 Hardening complet (SSH, fail2ban, UFW, auditd...)
- 🐳 Installation et configuration de Docker
- 📝 Gestion des logs (logrotate)
- 👥 Gestion multi-utilisateurs avec clés SSH

## 🏃 Démarrage rapide

1. Clonez le projet
2. Configurez vos serveurs dans `inventory/hosts`:
```ini
[debian_servers]
monserver ansible_host=X.X.X.X ansible_user=monuser
```

3. Configurez vos utilisateurs dans `inventory/group_vars/all.yml`:
```yaml
users:
  - username: monutilisateur
    groups: ['sudo']
    shell: /bin/zsh
    ssh_keys:
      - "ssh-rsa VOTRE_CLE_SSH"
```

4. Lancez le playbook:
```bash
# Installation complète
ansible-playbook playbooks/site.yml

# Installation sélective avec tags
ansible-playbook playbooks/site.yml --tags "common,docker"  # Juste common et docker
ansible-playbook playbooks/site.yml --tags security        # Juste la sécurité
```

## 🏷️ Tags disponibles

- `common`: Configuration de base
- `docker`: Installation Docker
- `security`: Hardening serveur
- Plus spécifique: `ssh`, `fail2ban`, `ufw`, `users`, `packages`...

## ⚠️ Prérequis

- Ansible 2.9+
- Serveur Debian 12 fraîchement installé
- Accès SSH root ou sudo

## 🤝 Contact

Des questions ? Des suggestions ? N'hésitez pas à ouvrir une issue !

Happy deploying! 🎉