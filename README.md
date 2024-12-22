# 🚀 Ansible Server Setup - Debian & Ubuntu
Un projet Ansible pour configurer rapidement et sécuriser un serveur Debian ou Ubuntu avec Docker !

## 🎯 Fonctionnalités
- ⚙️ Configuration système de base (paquets, utilisateurs, shell zsh)
- 🔒 Hardening complet (SSH, fail2ban, UFW, auditd...)
- 🐳 Installation et configuration de Docker
- 📝 Gestion des logs (logrotate)
- 👥 Gestion multi-utilisateurs avec clés SSH

## ⚠️ Prérequis
- Ansible 2.9+
- Configuration initiale requise :
  - Un utilisateur non-root
  - Un utilisateur root
  - OpenSSH installé (`apt install openssh-server`)

## 💻 Distributions supportées et testées
- ✅ Debian 12 (Bookworm)
- ✅ Ubuntu 24.04 (Noble Numbat)

## 🏃 Démarrage rapide
1. Clonez le projet

2. Copiez les fichiers d'exemple :
```bash
cp inventory/hosts.example inventory/hosts
cp inventory/group_vars/all.yml.example inventory/group_vars/all.yml
```

3. Configurez vos serveurs dans `inventory/hosts`:
```ini
[linux_servers]
server1 ansible_host=X.X.X.X ansible_user=monuser
server2 ansible_host=Y.Y.Y.Y ansible_user=monuser2
```

4. Configurez l'accès SSH au serveur :
```bash
# Générer une paire de clés SSH si vous n'en avez pas
ssh-keygen -t ed25519

# Copier votre clé publique sur le(s) serveur(s)
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@X.X.X.X
```

5. Configurez vos variables dans `inventory/group_vars/all.yml`:

⚠️ **IMPORTANT: Configuration des mots de passe**
- Les mots de passe configurés ici sont uniquement pour l'élévation de privilèges (sudo/su)
- Il ne s'agit PAS des mots de passe de vos utilisateurs
- L'authentification de vos utilisateurs est déjà gérée par les clés SSH configurées à l'étape 4
- Pour Debian, utilisez le mot de passe root
- Pour Ubuntu, utilisez le mot de passe sudo de votre utilisateur
```yaml
# Mots de passe sudo/su par serveur
ansible_become_passwords:
  server1: "votre_mot_de_passe_server1"
  server2: "votre_mot_de_passe_server2"

ansible_become_password: "{{ ansible_become_passwords[inventory_hostname] }}"

# Configuration globale
global_config:
  timezone: 'Europe/Paris'
  locale: 'fr_FR.UTF-8'
  users:
    - username: monutilisateur
      groups: ['sudo']
      shell: /bin/zsh
      ssh_keys:
        - VOTRE_CLE_SSH
```

6. Lancez le playbook:
```bash
# Installation complète
ansible-playbook playbooks/site.yml

# Installation sélective avec tags
ansible-playbook playbooks/site.yml --tags "common,docker"  # Juste common et docker
ansible-playbook playbooks/site.yml --tags security         # Juste la sécurité
```

## 🏷️ Tags disponibles
- `common`: Configuration de base
- `docker`: Installation Docker
- `security`: Hardening serveur
- Plus spécifique: `ssh`, `fail2ban`, `ufw`, `users`, `packages`...

## 🤝 Contact
Des questions ? Des suggestions ? N'hésitez pas à ouvrir une issue !

Happy deploying! 🎉
