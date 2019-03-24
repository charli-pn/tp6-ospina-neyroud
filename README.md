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
Pour vérifier que la création de leur dossier "home" est bien effective on saisit la commande ```ll /home/```.

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
L'argument "-R" permet de d'appliquer les droits de manière récursive. 

#### Question 6 : Remplacez le groupe primaire des utilisateurs :
* groupe1 pour u1 et u2 ```sudo usermod u1 -g groupe1``` ```sudo usermod u2 -g groupe1```
* groupe2 pour u3 et u4 ```sudo usermod u3 -g groupe2``` ```sudo usermod u4 -g groupe2```

#### Question 7 : Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
On créer tout d'abord les deux répertoires ```sudo mkdir groupe1 groupe2```.  
On attribue ensuite les droits spécifiques aux dossiers :
* ```sudo chown -R :groupe1 /home/groupe1``` ```sudo chmod g+w groupe1```
* ```sudo chown -R :groupe2 /home/groupe2```  ```sudo chmod g+w group2```

#### Question 8 : Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
On peut activer le sticky bit grâce à la commande : 
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
On saisit la commande ```cat /etc/group | grep groupe1``` qui nous retourne alors ```groupe1:x:1001:u1,u2,u4```. L'ID du groupe 1 est donc 1001.

#### Question 14 : Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)
On saisit la commande ```grep :1002: /etc/group``` qui nous retourne alors ```groupe2:x:1002:u2,u3,u4```. C'est donc le groupe 2 qui a pour guid 1002.

#### Question 15 : Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
On saisit la commande ```sudo gpasswd -d u3 groupe2```.  
On obtient alors ```Retrait de l'utilisateur u3 du groupe groupe2```.

#### Question 16 : Modifiez le compte de u4 de sorte que :
* il expire au 1 er juin 2019 (-E)
* il faut changer de mot de passe avant 90 jours (-M)
* il faut attendre 5 jours pour modifier un mot de passe (-m)
* l’utilisateur est averti 14 jours avant l’expiration de son mot de passe (-W)
* le compte sera bloqué 30 jours après expiration du mot de passe (-I)
  
On saisit la commande ```sudo chage -E 2019-06-01 -M 90 -m 5 -W 14 -I 30  u4```.

#### Question 17 : Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
Il s'agit de Bash puisque la commande ```echo $SHELL``` retourne ```/bin/bash```.

#### Question 18 : à quoi correspond l’utilisateur nobody ?
Le user nobody est un "non-privileged user". Comme le nom l'indique il n'a aucun droit (si on regarde dans /etc/shadow il n'y a pas de mot de passe), mais néanmoins il joue un grand rôle pour le lancement de certains service (deamon).

#### Question 19 : Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?
"sudo" conserve le mot de passe en mémoire pendant 15 minutes par défaut. Pour forcer l'oubli, on utilise "sudo -k".


## Exercice 2. Gestion des permissions

#### Question 1 : Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier1 contenant quelques lignes de texte. Quels sont les droits sur test et fichier1 ?
Création du dossier ```test``` dans notre $HOME : ```mkdir test```.  
On obtient alors comme droits avec la commande ```ll``` : ```drwxrwxr-x ```.
  
Création du fichier ```fichier1``` dans notre dossier test ```echo "contenu" > test/fichier1.txt```.  
On obtient alors comme droits avec la commande ```ll test/``` : ```-rw-rw-r--```. 

#### Question 2 : Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?
On saisit la commande ```chmod 0 fichier1``` afin de retirer tous les doits sur ce fichier.  
On va ensuite passer root : ```su root```.  
On tape ensuite : ```nano fichier1```. Il est alors possible de modifier et lire ```fichier1``` puisque root possède quand même les droits sur tout.

#### Question 3 : Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
On saisit la commande ```chmod 300 fichier1``` pour se donner les droits w+x sur le fichier.  
On saisit ensuite la commande ```echo "echo Hello" > fichier1```.  
  
Si on n'a pas les droits d'écriture, il ne peut pas remplacer le contenu d'un fichier déjà existant. Cependant ayant les droits d'écriture et bien nous pouvons.

#### Question 4 : Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.
En saisissant la commande ```./fichier1``` le fichier ne s'éxécute pas.  
En saisissant la commande ```sudo ./fichier1``` le fichier s'éxécute !  
  
Cela est normal car même si on a les droits d'exécution, on n'a pas les droits de lecture et comme il s'agit d'un script et non d'un fichier binaire, il ne peut pas le lire. Avec ```sudo``` cependant, on peut car le superuser peut évidemment lire le fichier.

#### Question 5 : Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier essai. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test
On saisit la commande ```cd test/``` puis ```chmod -r .``` pour se retirer les droit en lecture de ce répertoire.  
La saisie de la commande ```cat fichier1``` va fonctionner mais ```ll``` ne fonctionnera pas sans droits de lecture.  
On rétablit alors le droit en lecture sur test ```chmod +r .```.

#### Question 6 : Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations ?
On se place dans le dossier ```test``` : ```cd test/```.  
On va créer un fichier ```nouveau``` : ```touch nouveau``` et le répertoire ```sstest``` : ```mkdir sstest```.  
On va retirer au fichier ```nouveau``` le droit en écriture : ```chmod -w nouveau .```.  
Un ```echo "bonjour" > nouveau``` ne fonctionnera pas sans les droits en écriture.  
On va donner au dossier ```test``` le droit en écriture : ```chmod +w .```.  
Un ```echo "bonjour" > nouveau``` ne fonctionnera toujours pas sans les droits d'écriture.  
Un ```rm nouveau " > nouveau``` ne fonctionnera pas sans les droits d'écriture.  
  
Nous pouvons donc en déduire que le changement des droits sur un dossier ne modifie pas les droits sur le fichier contenu dans ce dossier (absence de recursivité).

#### Question 7 : Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
On va se positionner dans le répertoire personnel ```cd```.  
On va retirer le droix en exécution du répertoire ```test``` : ```chmod -x test/```.  
On va ensuite effecteur diverses manipulations dans ce répertoire : ```touch test/nouveau2``` -> ne marche pas, ```nano test/nouveau``` -> ne marche pas, ```cat test/nouveau``` -> ne marche pas, ```rm test/nouveau``` -> ne marche pas, ```cd test``` -> ne marche pas.  
Avec la commande ```ll test``` on ne peut lire aucune information sauf le nom des fichiers/dossier contenu dans le dossier ```test```.  
  
Le droit x sur les dossier permet de se déplacer dedans avec cd par exemple. Les actions ci-dessus ne sont pas possible car le chemin n'est plus accessible sans le droit x.

#### Question 8 : Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?
On attribue au dossier test le droit en éxécution : ```chmod +x test```.  
On se place dans le dossier test en lui retirant son droit en éxécution : ```cd test``` , ```chmod -x .```.  
On essaye de créer un nouveau fichier : ```touch nouveau2``` -> ne marche pas.  
On essaye diverses manipulations sur le fichier ```nouveau``` : ```nano nouveau``` ne marche pas, ```cat nouveau``` ne marche pas et ```rm nouveau``` ne marche pas.   
La commande ```ll .```ne marche pas.  
La commande ```cd sstest``` ne marche pas.  
Seule la commande ```cd ..``` va fonctionner car elle utilise la variable ```pwd``` et enlève le dernier dossier.  
  
On ne peut pas obtenir le contenu du dossier actuel car on a pas les droit sur le chemin demandé (chemin relatif), par contre on peut le faire avec un chemin absolu car on possède les droits sur le dossier parent (chemin absolu).

#### Question 9 : Rétablissez le droit en exécution du répertoire test. Attribuez au fichier essai les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
Afin de donner les droit de lecture à un fichier à une autre personne du groupe de l'utilisateur il suffit d'utiliser la commande ```chmod``` comme suivant :
```
chmod +x test
cd test
chmod 740 nouveau
```

#### Question 10 : Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
Afin de restreindre totalement l'accès des fichier créée par l'utilisateur il suffit de modifier le umask comme ceci :
```
umask 077
mkdir test
touch testfile
```
On obtiendra alors pour les droits de : 
* test : ```drwx------```
* testfile : ```-rw--------``` 

#### Question 11 : Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper-toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
Pour un umask moins restrictif (droit de lecture et de traversée du répértoire) pour l'utilisateur :
```
umask 522
mkdir test 
touch testfile
```
On obtiendra alors pour les droits de : 
* test : ```d-w-r-xr-x```
* testfile : ```--w-r--r--``` 

#### Question 12 : Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
Pour un umask équilibré (accès en lecture aux membres du groupes) :
```
umask 033
mkdir test 
touch testfile
```
On obtiendra alors pour les droits de : 
* test : ```drwxr--r--```
* testfile : ```drwxr--r--``` 

#### Question 13 : Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
* chmod u=rx,g=wx,o=r fic
* chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
* chmod 653 fic en sachant que les droits initiaux de fic sont 711
* chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---  
  
Pour ```chmod u=rx,g=wx,o=r fic``` <=> ```chmod 534 fic```.  
Pour ```chmod uo+w,g-rx fic``` <=> ```chmod 602 fic```.  
Pour ```chmod 653 fic``` <=> ```chmod u-x,g+r,o+w fic```.  
Pour ```chmod u+x,g=w,o-r fic``` <=> ```chmod 520 fic```.  

#### Question 14 : Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?
La commande ```ll /usr/bin/passwd``` va nous renvoyer : ```-rwsr-xr-x root root```.  
On remarque que le droit d'exécution est remplacé par un ```s```. Cela signifie qu'à l'exécution, le programme va se lancer comme si c'était l'owner qui le lançait (ici, ```root```) sans avoir besoin d'utiliser ```sudo``` par exemple.
  
La commande ```ll /etc/passwd``` va nous renvoyer : ```-rw-r--r-- root root```.  
Seul l'owner (```root```) possède les droits d'écriture sur ce fichier. Ainsi, le bit de setuid (le ```s```) permet à n'importe quel utilisateur d'utiliser le programme en tant que ```root``` et donc de modifier ce fichier.

#### Question 15 : Access Control Lists (ACL) : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/acl.

#### Question 16 : Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.
