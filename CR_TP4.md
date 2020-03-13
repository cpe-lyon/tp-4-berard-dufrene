# Compte rendu TP4
## Administration systèmes
BERARD Lucas
DUFRENE Mélic

### Exercice 1. Gestion des utilisateurs et des groupes

**1.** Commencez par créer deux groupes groupe1 et groupe2
```
sudo addgroup groupe1 
sudo addgroup groupe2
```
**2.** Créez ensuite 4 utilisateurs u1,u2,u3,u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell
```
sudo useradd -m -s /bin/bash u1
sudo useradd -m -s /bin/bash u2
sudo useradd -m -s /bin/bash u3
sudo useradd -m -s /bin/bash u4
```

**3.** Ajoutez les utilisateurs dans les groupes créés 
```
sudo gpasswd -M u1 u2 u4 groupe1
sudo gpasswd -M u2 u3 u4 groupe2
```
Il se trouve que gpasswd -M supprime les utilisateurs déjà présents auparavant dans le groupe.

**4.** Donnez deux moyens d’afficher les membres de groupe2
```
cat /etc/group | grep groupe1
getent group groupe1
```
**5.** Faites de groupe1 le groupe propriétaire de/home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4
```

```
**6.** Remplacez le groupe primaire des utilisateurs :-groupe1pouru1etu2-groupe2pouru3etu4
```

```
**7.** Créez deux répertoires/home/groupe1et/home/groupe2pour le contenu commun aux groupes, etmettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossierassocié.
```

```
**8.** Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommerou supprimer ce fichier?
```

```
**9.** Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
```

```
**10.** Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.
```

```
**11.** Quels sont l’uid et le gid de u1 ?
```

```
**12.** Quel utilisateur a pour uid 1003 ?
```

```
**13.** Quel est l’id du groupe groupe1 ?
```

```

### Exercice 2. Gestion des permissions
**1.** Dans votre$HOME, créez un dossiertest, et dans ce dossier un fichierfichiercontenant quelqueslignes de texte. Quels sont les droits surtestetfichier?
```

```

**2.** Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher entant queroot. Conclusion?
```

```

**3.** Redonnez vous les droits en écriture et exécution surfichierpuis exécutez la commandeecho "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’unfichier s’il existe déjà. Que peut-on dire au sujet des droits?
```

```

**4.** Essayez d’exécuter le fichier. Est-ce que cela fonctionne? Et avec sudo? Expliquez.
```

```

**5.** Placez-vous dans le répertoiretest, et retirez-vous le droit en lecture pour ce répertoire. Listez lecontenu du répertoire, puis exécutez ou aﬀichez le contenu du fichierfichier. Qu’en déduisez-vous?Rétablissez le droit en lecture surtest
```

```

**6.** Créez dans test un fichiernouveauainsi qu’un répertoiresstest. Retirez au fichiernouveauet aurépertoiretestle droit en écriture. Tentez de modifier le fichiernouveau. Rétablissez ensuite le droiten écriture au répertoiretest. Tentez de modifier le fichiernouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations?
```

```

**7.** Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoiretest.Tentez de créer, supprimer, ou modifier un fichier dans le répertoiretest, de vous y déplacer, d’enlister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires?
```

```

**8.** Rétablissez le droit en exécution du répertoiretest. Positionnez vous dans ce répertoire et retirez luià nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoiretest, de vous déplacer dansssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence desdroits que l’on possède sur le répertoire courant? Peut-on retourner dans le répertoire parent avec ”cd..”? Pouvez-vous donner une explication?
```

```

**9.** Rétablissez le droit en exécution du répertoiretest. Attribuez au fichierfichierles droits suﬀisantspour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
```

```

**10.** Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture,ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
```

```

**11.** Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper-toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
```

```

**12.** Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture auxmembres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
```

```

**13.** Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vouspourrez vous aider de la commandestatpour valider vos réponses) :-chmod u=rx,g=wx,o=r fic-chmod uo+w,g-rx ficen sachant que les droits initiaux deficsontr--r-x----chmod 653 ficen sachant que les droits initiaux deficsont711-chmod u+x,g=w,o-r ficen sachant que les droits initiaux deficsontr--r-x---
```

```

**14.** Aﬀichez les droits sur le programmepasswd. Que remarquez-vous? En aﬀichant les droits du fichier/etc/passwd, pouvez-vous justifier les permissions sur le programmepasswd?
```

```
