## CODEBYGPT

# Guide Complet de Proxmox

Playbook complet pour Proxmox Backup Server : couvre l'installation, la configuration réseau, la sécurisation avec iptables, l'intégration de Let's Encrypt, ainsi que la sauvegarde et la restauration de VMs. Un guide essentiel pour une gestion optimale de PBS.

## Paramètres du Playbook

- `run_backup: true` : Active l'envoi des sauvegardes vers PBS.
- `run_restore: false` : Active la restauration des sauvegardes depuis PBS.
- `start_vm_after_restore: true` : Démarre la VM après la restauration.
- `restart_vm_after_backup: true` : Redémarre la VM après la sauvegarde.

### Rejoignez notre communauté sur Discord pour obtenir du support et partager vos expériences.

[![Rejoignez le Discord !](https://img.shields.io/discord/347412941630341121?style=flat-square&logo=discord&colorB=7289DA)](https://discord.gg/rSfTxaW)

<p align="center">
  <img src="https://i.ibb.co/ZSwCpNd/DALL-E-2023-12-31-22-16-41-Une-repr-sentation-futuriste-et-sophistiqu-e-d-un-Proxmox-Setup-Playbook.png" alt="Proxmox Setup Playbook">
</p>

## Mise en place de l'environnement

Pour préparer votre environnement de travail, suivez ces étapes :

```bash
python3 -m venv venv
source venv/bin/activate
pip install ansible
```

### Quelques ajustements

Ajouter vos régles iptables et modifier sont nom `cp exemple.iptables.j2` par `iptables.j2`

Modifier le fichier `exemple.conf1.yml` par `conf1.yml` renseigner les variables dedans puis exécuté le playbook avec sont fichier de configuration `conf1.yml`
```
ansible-playbook -i inventaire.ini playbook.yml -e @conf1.yml
```

# Documentation pour `conf1.yml`

Ce fichier de configuration (`conf1.yml`) est utilisé pour spécifier les paramètres de réseau et d'autres configurations pour l'installation et la configuration de Proxmox VE. Voici une description des principales sections de ce fichier :

### Configurations réseau

Cette section concerne la configuration du réseau.

- `ipv4`: L'adresse IP IPv4.
- `ipv4_gw`: La passerelle IPv4.
- `ipv6`: L'adresse IP IPv6.
- `ipv6_gw`: La passerelle IPv6.
- `br_ipv4`: L'adresse IP du pont IPv4.
- `br_ipv6`: L'adresse IP du pont IPv6.
- `bridge`: Le nom du pont réseau (ex. vmbr0).


### Activation de Let's Encrypt

- `lets_encrypt_enabled`: Indique si Let's Encrypt doit être activé ou non pour obtenir un certificat SSL.
- `lets_encrypt_email`: L'adresse e-mail associée au certificat Let's Encrypt.
- `lets_encrypt_domain`: Le domaine pour lequel le certificat Let's Encrypt doit être obtenu.
- `acme_account_name`: Le nom du compte ACME pour la gestion des certificats Let's Encrypt.

### Proxmox Backup Serveur (PBS)

- `configurer_pbs`: Indique si la configuration de Proxmox Backup Serveur (PBS) doit être activée.
- `pbs_server_ip`: L'adresse IP du serveur PBS.
- `pbs_user`: Le nom d'utilisateur pour l'accès au serveur PBS.
- `pbs_password`: Le mot de passe pour l'accès au serveur PBS.
- `pbs_fingerprint`: L'empreinte du serveur PBS.
- `pbs_datastore`: Le datastore à utiliser pour les sauvegardes PBS.
- `pbs_storage_id`: L'identifiant du stockage PBS.

### Configuration iptables Proxmox

- `configurer_iptables`: Indique si la configuration des règles iptables Proxmox doit être activée.
- `internal_interface`: L'interface interne, souvent un pont.
- `allowed_ip`: L'adresse IP autorisée pour l'accès à l'interface web de Proxmox (maison).
- `allowed_ip_v6`: L'adresse IPv6 autorisée (maison).
- `allowed_ip_prive`: L'adresse IP privée autorisée pour SSH, etc. (VPN configuré sur une VM).
- `port_ssh`: Le port SSH Proxmox.
- `ip_wireguard`: L'adresse IP du serveur WireGuard (L'IP DE LA VM V4).
- `ip_v6_wireguard`: L'adresse IPv6 du serveur WireGuard (L'IP DE LA VM V6).
- `port_wireguard`: Le port du serveur WireGuard.

---

## Playbook

Le playbook fourni est utilisé pour installer et configurer Proxmox VE en utilisant les paramètres définis dans le fichier `exemple.conf1.yml`. Voici un aperçu des principales tâches effectuées par ce playbook :

1. Vérification de l'installation de Proxmox VE.
2. Ajout de la clé GPG de Proxmox VE et du dépôt Proxmox VE si nécessaire.
3. Mise à jour des paquets et installation de Proxmox VE et des dépendances.
4. Configuration du réseau en utilisant le nom de l'interface réseau principale.
5. Génération des fichiers de configuration iptables pour IPv4 et IPv6 si nécessaire.
6. Application des règles iptables.
7. Vérification de la connectivité IPv4 et IPv6.
8. Affichage des règles iptables actuelles.
9. Configuration de Proxmox Backup Serveur (PBS) si activé.

Ce playbook est conçu pour automatiser l'installation et la configuration de Proxmox VE de manière efficace. Assurez-vous d'avoir correctement configuré les paramètres dans le fichier `exemple.conf1.yml` avant de l'exécuter.
