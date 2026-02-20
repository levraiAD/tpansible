# TP  
## Conception, Déploiement et Administration d’un Réseau d’Entreprise  
## Axel DAMIEN et Martin DECOBECQ
### TechSecure

---

# 1. Introduction

Dans le cadre de ce TP, nous avons mis en place une infrastructure hybride pour l’entreprise fictive **TechSecure** à l’aide d’Ansible.

L’objectif était d’automatiser :

- L’administration de serveurs Linux et Windows
- Le déploiement d’un serveur Web
- La configuration d’un firewall
- L’application de mesures de hardening

L’infrastructure est composée de serveurs Linux, d’un serveur Windows, d’un serveur Web et d’un firewall centralisé.

---

# 2. Architecture de l’infrastructure

## 2.1 Serveur Linux

- Administration système
- Renforcement de la configuration SSH
- Installation d’outils de supervision et de sécurité

## 2.2 Serveur Windows

- Installation des rôles DNS et DHCP
- Application d’une politique de mot de passe sécurisée

## 2.3 Serveur Web

- Installation de Nginx
- Hébergement d’une page Web
- Ouverture des ports HTTP et HTTPS

## 2.4 Firewall

- Politique restrictive par défaut
- Autorisation uniquement des ports nécessaires (22, 80, 443)

---

# 3. Structure du projet Ansible
```txt
tpansible/
│
├── inventory/
│ └── hosts.ini
│
├── playbooks/
│ ├── linux.yaml
│ ├── windows.yaml
│ ├── web.yaml
│ └── firewall.yaml
│
├── roles/
│ ├── linux/tasks/main.yml
│ ├── windows/main.yaml
│ ├── web/main.yaml
│ └── firewall/tasks/main.yaml
```

## Explication

- **inventory/** : contient la définition des machines cibles.
- **playbooks/** : orchestrent l’exécution des rôles.
- **roles/** : contiennent les tâches spécifiques à chaque type de serveur.

---

# 4. Automatisation réalisée

## 4.1 Administration Linux

### Actions mises en place :

- Installation des paquets :
  - htop
  - fail2ban
  - ufw
- Désactivation du login root SSH
- Désactivation de l’authentification par mot de passe
- Activation de fail2ban
- Activation du firewall UFW

### Objectifs de sécurité :

- Réduction des attaques par brute-force
- Suppression des accès root directs
- Limitation des connexions SSH non sécurisées

---

## 4.2 Administration Windows

### Actions mises en place :

- Installation des rôles :
  - DNS
  - DHCP
- Application d’une politique de mot de passe :
  - Longueur minimale : 12 caractères
  - Durée maximale : 30 jours
- Désactivation de SMBv1

### Objectifs de sécurité :

- Renforcement de l’authentification
- Suppression d’un protocole vulnérable
- Centralisation des services réseau

---

## 4.3 Déploiement du serveur Web

### Actions réalisées :

- Installation de Nginx
- Activation et démarrage du service
- Déploiement automatique d’une page Web
- Ouverture des ports 80 et 443

### Résultat :

Le serveur Web est accessible via HTTP et héberge une page déployée automatiquement par Ansible.

---

## 4.4 Configuration du Firewall

### Règles appliquées :

- Politique par défaut : DENY incoming
- Autorisation des ports :
  - 22 (SSH)
  - 80 (HTTP)
  - 443 (HTTPS)

### Objectif :

Application du principe du moindre privilège afin de réduire la surface d’attaque.

---

# 5. Hardening appliqué

## 5.1 Linux

- Désactivation du login root
- Désactivation de l’authentification par mot de passe
- Activation de fail2ban
- Activation du firewall

## 5.2 Windows

- Désactivation de SMBv1
- Politique mot de passe renforcée
- Installation contrôlée des rôles

---

# 6. Tests et validation

## Commandes utilisées
```txt
ansible-playbook -i inventory/hosts.ini playbooks/linux.yaml
ansible-playbook -i inventory/hosts.ini playbooks/windows.yaml
ansible-playbook -i inventory/hosts.ini playbooks/web.yaml
ansible-playbook -i inventory/hosts.ini playbooks/firewall.yaml
```

## Résultats

- Tous les playbooks s’exécutent sans erreur.
- Les services sont correctement installés.
- Les règles firewall sont appliquées.
- Le serveur Web est fonctionnel.
- Exemple de connexion ssh : 
```txt
TASK [Ping the web server] **********************************************
ok: [192.168.1.59] => {
    "msg": "Le serveur web est joignable via SSH."
}
```
