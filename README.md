# TP 4 - Utilisateurs, groupes et permissions

Dans ce quatrième TP, nous allons voir comment gérer les utilisateurs, les groupes et les permissions sur notre système,
ainsi que les quotas disque.

## Exercice 1. Gestion des utilisateurs et des groupes

#### Question 1 : Commencez par créer ensuite deux groupes groupe1 et groupe2


#### Question 2 : Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec leur dossier ”home” et ayant bash pour shell


#### Question 3 : Placez les utilisateurs dans les groupes :
*- u1, u2, u4 dans groupe1
- u2, u3, u4 dans groupe2*  

#### Question 4 : Donnez deux moyens d’afficher les membres de groupe2

#### Question 5 : Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

#### Question 6 : Remplacez le groupe primaire des utilisateurs :
*- groupe1 pour u1 et u2
- groupe2 pour u3 et u4*  


#### Question 7 : Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.


#### Question 8 : Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?


#### Question 9 : Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?


#### Question 10 : Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.


#### Question 11 : Quels sont l’uid et le gid de u1 ?


#### Question 12 : Quel utilisateur a pour uid 1003 ?

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


#### Question 1 : 


#### Question 2 : 


#### Question 3 : 


#### Question 4 : 


#### Question 5 : 


#### Question 6 : 


#### Question 7 : 


#### Question 8 : 


#### Question 9 : 


#### Question 10 : 


#### Question 11 : 


#### Question 12 : 


#### Question 13 : 


#### Question 14 : 


