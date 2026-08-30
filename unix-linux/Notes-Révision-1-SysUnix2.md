# Notes Cours PS Parti 1

- gestion du processeur graçe au l'ordonnancement (Scheduling)
- Unix est multi tache graçe au concept de "temps partagé"
- Shell est un interpréteur de commande
- Méta-caractères du shell (`*`, `?`, `[]`)
- le système de fichiers d'Unix est une vaste arborescence dont les noeuds sont des répertoires et des fichiers.
- Unix est un système qui distingue les caractères majuscules et minuscules (case sensitive)

## Types de fichiers

- les fichers ordinaires
- les répertoires
- les liens symboliques
- les périphériques (`/dev`)
- les turbes nommés

## Les i-nœuds

à chaque fichier correspond un i-noeud contenant:

- le type du fichier et les droites d'accès des différents utilisateurs
- le propriétaire du fichier
- la taille du fichier (pas de sens pour les fichier spéciaux)
- le nombre de liens physiques sur le fichier
- la date de dernière modification/consultation (écriture/lecture)
- la date de dernière modification du noeud (modification d'attribus)
- l'identification de la ressource associée (pour les fichiers spéciaux)

## Liens symboliques et physiques

### Symboliques

- Si le fichier original est supprimé le lien sybmolique devient cassé (dangling link)

### Physiques

- même inode, deux nom pour même fichier

## Quelques Commandes

```shell
# déplacer un fichier avec confirmation
mv -i

# voir les différents disks et le nombre d'inodes
df -i

# [c]reate an archive and write it to a [f]ile:
# tar cf {{path/to/target.tar}} {{path/to/file1 path/to/file2 ...}}
tar cf themes.tar themes/

# -p: create parent directories if needed
mkdir -p  A01/etudiant1/journal A01/etudiant2/journal

# list directories recursively
ls -R
```

## Les Droits d'accès

- chaque login fait correspondre un numéro d'identification uid (User Identifier) et un gid (Group Identifier)
- utilisateur peut appartenir à plusieurs groups
- root utilisateur il est le seut qu'il peut ajouter de nouveaux utilisateurs au système.

---

### concepts de bases

- sujet (utilisateur ou processus)
- objet (fichier)
- opérations (actions R - W - X)

---

### commands

- chown
- chgrp (2 conditions, etre le propriétaire du fichier, etre membre du groupe auquel on veut donner le fichier)
- chmod
- umask

---

Gestion des droits d'accès: exercice

- chmod o-rx RepertoireUn
- chmod g+w fichierUn
- chmod u+x fichierUn

---

### Mode symbolique & ocal

- mode symbolique: g+rwx 
- mode octal: 775
- example: 764 en octal => chmod u+rwx, g+rw, o+r

### umask

- Fichiers : `666 - 022 = 644` → `rw-r--r--`
- Répertoires : `777 - 022 = 755` → `rwxr-xr-x`

777-666 s'appelle le complément.

---

### Les droits d'accès pour répertoire:

- **r (read)** → can list filenames inside (`ls TP1`)
- **w (write)** → can create, delete, rename files **inside** the directory
- **x (execute)** → can **enter** the directory (`cd TP1`) and access files

**Directory Authority:** In Linux, deleting a file is technically a change to the directory (unlinking an entry), not the file itself. If you own the folder, you can delete any file inside it.

---