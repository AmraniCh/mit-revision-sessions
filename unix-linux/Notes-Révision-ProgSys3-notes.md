# 1. Processus

**voir les processus avec parent ID:**
```bash
ps -eo pid,ppid,cmd --forest
```

## cat < fichier.txt

```
SHELL (parent)
   |
   |-- fork()
   |
ENFANT
   ├─ open("fichier")
   ├─ dup2 → stdin
   └─ exec(cat)
```

* Quand un processus parent meurt: ses enfants deviennent des processus orphelins

# 2. Descripteur de fichier

## ajout d'un nouvelle file descripteur

```bash
exec 3< toto2
cat toto2
hello
read -u 3 ligne
echo $ligne
```

## quand sont créés les descripteurs 0, 1 et 2 ?

Quand tu ouvres un terminal :

* le système crée un nouveau processus (par exemple bash)
* le noyau Linux lui crée une table de descripteurs de fichiers
* les descripteurs 0, 1 et 2 sont automatiquement ouverts

## Note

* Les redirections consistent à modifier ces descripteurs.

# 3. Deux Types de commands

Effet sur le shell courant

```bash
cd /tmp
```

* cd DOIT être built-in
* Si cd était externe → le changement de répertoire serait perdu

# 6. redirections

## Rappel STDIN / STDOUT

### Comprendre STDIN

**Cas 1: STDIN relié à un fichier:**

```bash
cat < fichier.txt
# Bash redirige le descripteur 0 (STDIN) vers fichier.txt
# Bash ne lit pas le fichier
# cat lit STDIN
# cat affiche ces données sur STDOUT
```

**Cas 2: STDIN relié à un pipe:**

```bash
echo "hello" | cat
# STDIN reçoit des données
# STDIN contient la chaîne "hello\n"
# cat affiche exactement ces données
```

### arguments et stdin

**Commandes basées uniquement sur des arguments:**

* `ls`, `cd`, `mkdir`, `rm`, `chmod`, `cp`, `mv`, `touch`, `pwd`

**Commandes qui lisent stdin:**

* `cat`, `bc`, `grep`, `sort`, `wc`, `awk`, `sed`, `tr`

Comment savoir si une commande lit stdin ?

```bash
echo "test" | ls # ➡ aucun effet → ls n’utilise pas stdin.
echo "test" | wc -c
```

## Examples de redirections (De Cours)

* La commande Linux bc (pour basic calculator): offre des fonctionnalités de calcul arithmétique

# 8. Les Scripts Shell


## 8.2 Rôle de la ligne #!/bin/bash (shebang)

```bash
bash --posix
```

### Exemple de fonctionnalités Bash non POSIX

```
[[ ... ]]          # test bash
(( i++ ))          # arithmétique bash
arr=(a b)          # tableaux
${var^^}           # expansion de casse
<<< "text"         # here-string
```

* Le standard POSIX et /bin/sh : L'utilisation de l'interpréteur /bin/sh force le développeur à respecter le standard POSIX. Cela garantit que le script sera capable de s'exécuter sur n'importe quel système de type Unix, même si Bash n'est pas installé. C'est le niveau de portabilité maximale.

## 8.4 Variables et Subtitution de Variables

### Double guillemets et Simples guillemets

```bash
# Il y a des espaces
fullname=shakir el amrani # erreur

# Il y a des caractères spéciaux
fullname=TP*
echo "$fullname" # TP*

# Avec expansion de variables
prenom=shakir
fullname="$prenom el"
echo $fullname # shakir el

# si tu ne veux aucune interprétation de Bash
# c'est le mode littéral. Le Shell traite tout ce qui est entre ' ' comme du texte brut.
echo '$HOME $USER $(date)'
```

## 8.6 Les codes de retour (Le statut de sortie)

```bash
ls /dossier_inexistant
echo "Je vérifie le résultat..."
echo $? # Affiche 0 !
```

### Une commande peut échouer sans rien écrire sur stderr

```bash
grep "existe pas" test.sh # affiche rien, stderr est vide
echo $? # 1
```

### Une commande peut réussir en écrivant sur stderr

```bash
echo "salut" >&2 # écrit sur stderr
echo $? # 0
```





