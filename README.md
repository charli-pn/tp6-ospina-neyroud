# TP 4 - Utilisateurs, groupes et permissions

Dans ce quatrième TP, nous allons voir comment gérer les utilisateurs, les groupes et les permissions sur notre système,
ainsi que les quotas disque.

## Exercice 1. Gestion des utilisateurs et des groupes

#### Question 1 : Commencez par créer deux groupes groupe1 et groupe2

```bash
sudo groupadd groupe1
sudo groupadd groupe2
```

#### Question 2 : Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec leur dossier ”home” et ayant bash pour shell

```bash
sudo useradd -m -s /bin/bash u1
sudo useradd -m -s /bin/bash u2
sudo useradd -m -s /bin/bash u3
sudo useradd -m -s /bin/bash u4
```

#### Question 3 : Placez les utilisateurs dans les groupes :
* u1, u2, u4 dans groupe1 ```sudo adduser u1 groupe1``` ```sudo adduser u2 groupe1``` ```sudo adduser u4 groupe1```
* u2, u3, u4 dans groupe2 ```sudo adduser u2 groupe2``` ```sudo adduser u3 groupe2``` ```sudo adduser u4 groupe2```

#### Question 4 : Donnez deux moyens d’afficher les membres de groupe2

On peut utiliser la commande :
  * ```sudo cat /etc/group | grep 'groupe2'``` par exemple qui nous retourne ```groupe2:x:1002:u2,u3,u4```
  * ```members groupe2``` non installée par défaut qui retoune ```u2 u3 u4```

#### Question 5 : Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

```sudo chown -R :groupe1 /home/u1 /home/u2```  
```sudo chown -R :groupe2 /home/u3 /home/u4```

#### Question 6 : Remplacez le groupe primaire des utilisateurs :
* groupe1 pour u1 et u2 ```sudo usermod u1 -g groupe1``` ```sudo usermod u2 -g groupe1```
* groupe2 pour u3 et u4 ```sudo usermod u3 -g groupe2``` ```sudo usermod u4 -g groupe2```

#### Question 7 : Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.

On créer tout d'abord les deux répertoires ```sudo mkdir groupe1 groupe2```.  
On attribue ensuite les droits spécifiques aux dossiers :
* ```sudo chown -R :groupe1 /home/groupe1``` ```sudo chmod g+w groupe1```
* ```sudo chown -R :groupe2 /home/groupe2```  ```sudo chmod g+w group2```

#### Question 8 : Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
```sudo chmod +t groupe1```
```sudo chmod +t groupe2```

#### Question 9 : Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

Non puisque nous ne lui avons pas encore assigné de mot de passe.

#### Question 10 : Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.

On définit un mot de passe pour u1 avec la commande ```sudo passwd u1```.

#### Question 11 : Quels sont l’uid et le gid de u1 ?

On se connecte avec l'utilisateur u1 : ```su u1```. On saisit ensuite la commande ```id```.
On obtient alors ```uid=1001(u1) gid=1001(groupe1) groupes=1001(groupe1)```.

#### Question 12 : Quel utilisateur a pour uid ?

Il s'agit de l'utilisateur u3 puisque ce dernier a été créer en troisième position.

#### Question 13 : Quel est l’id du groupe groupe1 ?



#### Question 14 : Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)

#### Question 15 : Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.

#### Question 16 : Modifiez le compte de u4 de sorte que :
* il expire au 1 er juin 2019
* il faut changer de mot de passe avant 90 jours
* il faut attendre 5 jours pour modifier un mot de passe
* l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
* le compte sera bloqué 30 jours après expiration du mot de passe


#### Question 17 : Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?


#### Question 18 : à quoi correspond l’utilisateur nobody ?

#### Question 19 : Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?


## Exercice 2. Gestion des permissions


#### Question 1 : Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier1 contenant quelques lignes de texte. Quels sont les droits sur test et fichier1 ?


#### Question 2 : Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?


#### Question 3 : Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?


#### Question 4 : Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.


#### Question 5 : Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier essai. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test


#### Question 6 : Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations ?


#### Question 7 : Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?


#### Question 8 : Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?


#### Question 9 : Rétablissez le droit en exécution du répertoire test. Attribuez au fichier essai les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.


#### Question 10 : Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.


#### Question 11 : Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper-toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.


#### Question 12 : Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture auxmembres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.


#### Question 13 : Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vouspourrez vous aider de la commande stat pour valider vos réponses) :
* chmod u=rx,g=wx,o=r fic
* chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
* chmod 653 fic en sachant que les droits initiaux de fic sont 711
* chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---

#### Question 14 : Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?


#### Question 15 : Access Control Lists (ACL) : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/acl.

#### Question 16 : Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.
