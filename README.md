# ANSIBLE

## Installation d'Ansible
```bash
python3 -m venv .env
source .env/bin/activate
pip3 install ansible
```

## Test de l'installation
```bash
ansible --version
```

## Premières commandes Ansible
Ansible utilise le protocole SSH afin de communiquer avec avec des machines distantes. On va donc ajouter notre clé public au fichier authorized_keys du serveur.
```bash
ssh-copy-id user@192.168.1.161
```

Ensuite, ansible lit les informations de nos machines dans le fichier inventaire. Ici, on peut stocker les adresses IP ou les noms de domaines de nos machines (mais aussi des groupes, des alias ou des variables).
```bash
nano inventory
192.168.1.161
```

Maintenant, on peut exécuter notre premier module. Les modules sont comme de petits programmes qu'Ansible pousse vert nos machines distantes.
```bash
ansible all -i inventory -m ping -u user
```
- all: demande à Ansible d'exécuter la commande sur tous les serveurs de notre inventaire
- -i: spécifie le chemin d'un inventaire
- -m: on demande d'utiliser un module (ici ping)
- -u: on demande de lancer depuis un certain utilisateur (ici user)

