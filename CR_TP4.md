# Compte rendu TP4
## Administration systèmes
BERARD Lucas

DUFRENE Mélic

### Exercice 1. Gestion des utilisateurs et des groupes

#### 1. Commencez par créer deux groupes groupe1 et groupe2

```
sudo addgroup groupe1 
sudo addgroup groupe2
```
#### 2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell

```
sudo useradd -m -s /bin/bash u1
sudo useradd -m -s /bin/bash u2
sudo useradd -m -s /bin/bash u3
sudo useradd -m -s /bin/bash u4
```

#### 3. Ajoutez les utilisateurs dans les groupes créés 

```
sudo gpasswd -M u1 u2 u4 groupe1
sudo gpasswd -M u2 u3 u4 groupe2
```
*Il se trouve que gpasswd -M supprime les utilisateurs déjà présents auparavant dans le groupe.*

#### 4. Donnez deux moyens d’afficher les membres de groupe2

```
cat /etc/group | grep groupe1
getent group groupe1
```

#### 5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

```
sudo chgrp -R groupe1 /home/u1
sudo chgrp -R groupe1 /home/u2
sudo chgrp -R groupe2 /home/u3
sudo chgrp -R groupe2 /home/u4
```

#### 6. Remplacez le groupe primaire des utilisateurs : groupe1 pour u1 et u2 et groupe2 pour u3 et u4

```
sudo usermod u1 -g groupe1
sudo usermod u2 -g groupe1
sudo usermod u3 -g groupe2
sudo usermod u4 -g groupe2
```

#### 7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.

```
sudo mkdir /home/groupe1
sudo mkdir /home/groupe2
sudo chgrp -R groupe1 /home/groupe1
sudo chmod -R g+w /home/groupe1
sudo chgrp -R groupe2 /home/groupe2
sudo chmod -R g+w /home/groupe2
```

#### 8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier?

```
sudo chmod +t /home/groupe1
```
*Activation du stickybit.*

#### 9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

```
su u1
```
*Nous n'avons pas encore créé de mot de passe, le compte est donc inactif jusqu'à définition d'un mot de passe.*

#### 10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.

```
sudo passwd u1
-> mdp
-> mdp
su u1
-> mdp
```
*La connexion à fonctionnée, hourra !*

#### 11. Quels sont l’uid et le gid de u1 ?

```
id u1
-> uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)
```
*L'uid est donc 1001 et le gid 1001 aussi.*

#### 12. Quel utilisateur a pour uid 1003 ?

```
id 1003
```
*Cela se trouve être l'utilisateur u3.*

#### 13. Quel est l’id du groupe groupe1 ?

```
getent group groupe1 | cut -d: -f3
```
*L'id est de 1001.*

#### 14.Quel groupe a pour gid 1002?

```
getent group 1002 | cut -d: -f1
```
*C'est le groupe2. Cette methode permet d'éviter les groupes s'appellant 1002.*

#### 15.Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.

```
sudo gpasswd -d u3 groupe2
```
*Lorsque l'on regarde les groupes, u3 n'appartient pas au groupe2. Néanmoins, si l'on regarde les users, u3 semble appartenir encore au groupe.*

#### 16.Modifiez le compte de u4

```
sudo usermod -e 2020-06-01 u4   ->  il expire au 1er juin 2020
chag -M 90 u4                   ->  il faut changer de mot de passe avant 90 jours
chag -m 5 u4                    ->  il faut attendre 5 jours pour modifier un mot de passe
chag -w 14 u4                   ->  l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
chag -I 30 u4                   ->  le compte sera bloqué 30 jours après expiration du mot de passe
```

#### 17.Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?

```
cat /etc/passwd | grep root | cut -d: -f7
```
*L’interpréteur de commandes est /bin/bash*


#### 18.à quoi correspond l’utilisateur nobody ?

*C'est le nom donné au compte d'utilisateur à qui aucun fichier n'appartient, qui ne se trouve dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles de tout autres utilisateurs.
Il est en général utilisé pour lancer les processus en arrière plan.*

#### 19.Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe?

```
sudo vim /etc/sudoers
Defaults timestamp_timeout=0
```
*Par defaut, sudo conserve le mot de passe 15 minutes. Il faut ecrire la ligne précedente dans le fichier /etc/sudoers.*

### Exercice 2. Gestion des permissions


#### 1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?

```
mkdir test
vim test/fichier
ll
-> -rw-rw-r--
cd ..
ll | grep test
-> drwxrwxr-x
```
*Les droits pour fichier sont de lecture pour tout le monde, et écriture pour l'utilisateur courant et son groupe.
Les droits pour test sont de rentrer dans le dossier et d'afficher son contenu pour tout le monde, et modification pour l'utilisateur courant et son groupe.*

#### 2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher entant que root. Conclusion?

```
sudo chmod a=---
sudo -i
sudo vim test/fichier
```
*Nous sommes à présent en lecture seule sur vim mais on peut forcer l'écriture.*

#### 3. Redonnez-vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits?

```
sudo chmod u+wx fichier
echo "echo Hello" > fichier
```
*Les droits permettent à présent de modifier le fichier.*

#### 4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne? Et avec sudo? Expliquez.

```
./fichier
sudo ./fichier
```
*Les deux fonctionnent car on a les droits d'execution.*

#### 5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu de fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test

```
sudo chmod u-r .
ls
./fichier
sudo chmod u-r .
```
*Pour la commande ls, la permission est refusée mais le fichier reste executable. On peut donc acceder au contenu si l'on sait déjà ce qu'il y a dedans.*

#### 6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droiten écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations?

```
mkdir sstest
vim nouveau
sudo chmod u-w nouveau
sudo chmod u-w .
```
*Le fichier n'est pas modifiable, il est en lecture lecture seule. Après retablissement des droits, il reste non modifiable
mais la suppression est possible après confirmation.*

#### 7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’enlister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires?

```
cd ~
sudo chmod u-x test
vim test/fichier
rm test/fichier
touch test/fichier2
```
*On ne peut rien faire. Le droit d'execution permet uniquemment d'aller à l'interieur d'un repertoire.*

#### 8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant? Peut-on retourner dans le répertoire parent avec ”cd..”? Pouvez-vous donner une explication?

```
sudo chmod u+x test
cd test
sudo chmod u-x .
touch fichier2
rm fichier
cd sstest
ls
cd ..
```
*On ne peut rien faire. Seul sortir du repertoire (cd ..) est possible.*


#### 9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

```
sudo chmod u+x test
sudo chmod g+r-w test/fichier
```

#### 10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

```
umask 177
umask -S
mkdir newrep
touch newfichier
ll
```
*On verifie bien que les droit correspondent.*

#### 11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

```
umask 022
umask -S
mkdir newrep
touch newfichier
ll
```
*On verifie bien que les droit correspondent.*

#### 12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

```
umask 037
umask -S
mkdir newrep
touch newfichier
ll
```
*On verifie bien que les droit correspondent.*

#### 13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa

```
chmod 243 fic          ->   chmod u=rx,g=wx,o=r fic
chmod 175 fic          ->   chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
chmod u-x,g+r,o+w fic  ->   chmod 653 fic en sachant que les droits initiaux de fic sont 711
chmod 257 fic          ->   chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---
```

#### 14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

```
whereis passwd
cd /usr/bin
ls -l passwd
-> - rws r-x r-x
cd /etc
ls -l | grep passwd
-> - rw- r-- r-- 
```
*Personne n'a le droit d'execution sur le fichier /etc/passwd. C'est pour cela que seul sudo passwd peut fonctionner.
Cependant, tout le monde peut executer passwd pour changer son mot de passe.*
