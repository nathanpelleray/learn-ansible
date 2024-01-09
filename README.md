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

Si on a besoin de l'élévation de privilège, on peut utiliser les options:
- --become: demande une élévation de privilège
- --ask-become-pass: demande le mot de passe (pour sudo)
```bash
ansible all -i inventory -a "grep user /etc/shadow" -u user --become --ask-become-pass
```

## Les groupes dans l'inventaire
Il y a possibilité de mettre chaque hôte distant dans un ou plusieurs groupes.
```
[dev]
192.168.1.161

[prod]
192.168.1.162
192.168.1.163
```

Ensuite pour lancer un module, il suffit de remplacer le all par le nom du/des groupes.
```bash
ansible dev -i inventory -m ping -u user
```

## Utilisateur par défaut
On peut pour chaque machine distante de définir un utilisateur par défaut. Cela permet de ne plus mettre -u user
```
[dev]
192.168.1.161 ansible_user=user
```


## Mini Projet: Installation d'un serveur web
On commence par créer le fichier inventaire
```
[web]
192.168.1.161 ansible_user=user
```

Pour éviter d'utiliser -i inventory, on peut surcharger le fichier de configuration par défaut de Ansible.
Dans ansible.cfg:
```
[defaults]
inventory = ./inventory
```

Ensuite, on écrit notre fichier playbook. Puis, on lance le playbook avec la commande:
```bash
ansible-playbook playbook.yml --ask-become-pass
```

**Remarque:** On voit l'utilisation d'un template avec le fichier ip.php.j2 (cela peut être utile pour les fichiers de configuration)