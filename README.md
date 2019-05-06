# TP 6 - Gestion des disques / Tâches d’administration 

Dans ce sixième TP, nous allons manipuler divers outils de gestion des disques et partitions, ainsi que le partitionnement LVM. Nous verrons comment créer des points de montage et monter des partitions.

## Exercice 1. Disques et partitions

#### Question 1 : Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement
On utilise l'nterface graphique de VirtualBox pour répondre à cette question.

#### Question 2 : Vérifiez que ce nouveau disque dur est bien détecté par le système
La commande ```cfdisk -l``` permet de lister les disques durs connectés au système.

#### Question 3 : Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83), et une seconde partition de 3 Go en NTFS (n°7)
La commande ```sudo fdisk sdb``` permet de lancer l'outil fdisk sur le disque sdb.  
On appuie ensuite sur la touche ```n```pour ajouter une partition.  
On laisse par défaut tout les éléments sauf la fin de partition ou il faut mettre ```+2G``` pour préciser que l'on veut une partition de 2 Go (et ```+3G``` pour préciser que l'on veut une partition de 3 Go).  
Pour changer le type d'une partition il faut appuyer sur la touche ```t``` puis entrer le numéro hexadécimal du type, ```83``` pour le format de type Linux et ```7``` pour le format de type NTFS.

#### Question 4 : A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions (pensez à consulter le manuel !)
On formate la partition sdb1 au format ext4 (le plus récent des formats de fichier) avec la commande ```mkfs.ext4 /dev/sdb1```.  
Idem pour l'autre partition sdb2 avec la commande ```mkfs.ext4 /dev/sdb2```.

#### Question 5 : Pourquoi la commande df -T, qui affiche le type de système de fichier des partitions, ne fonctionne-telle pas sur notre disque ?


#### Question 6 : Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des UUID en raison de l’impossibilité d’effectuer des copier-coller)


#### Question 7 : Utilisez la commande mount puis redémarrez votre VM pour valider la configuration


#### Question 8 : Montez votre clé USB dans la VM


#### Question 9 : Créez un dossier partagé entre votre VM et votre système hôte (rem. il peut être nécessaire d’installer les Additions invité de VirtualBox


## Exercice 2. Partitionnement LVM

Dans cet exercice, nous allons aborder le partitionnement LVM, beaucoup plus flexible pour manipuler
les disques et les partitions.

#### Question 1 : On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes du fichier /etc/fstab


#### Question 2 : Supprimez les deux partitions du disque, et créez une patition unique de type LVM


#### Question 3 :  A l’aide de la commande pvcreate, créez un volume physique LVM. Validez qu’il est bien créé, en utilisant la commande pvdisplay


#### Question 4 : A l’aide de la commande vgcreate, créez un groupe de volumes, qui pour l’instant ne contiendra que le volume physique créé à l’étape précédente. Vérifiez à l’aide de la commande vgdisplay.


#### Question 5 : Créez un volume logique appelé lvData occupant l’intégralité de l’espace disque disponible.


#### Question 6 : Dans ce volume logique, créez une partition que vous formaterez en ext4, puis procédez comme dans l’exercice 1 pour qu’elle soit montée automatiquement, au démarrage de la machine, dans /data.


#### Question 7 : Eteignez la VM pour ajouter un second disque (peu importe la taille pour cet exercice). Redémarrez la VM, vérifiez que le disque est bien présent. Puis, répétez les questions 2 et 3 sur ce nouveau disque.


#### Question 8 : Utilisez la commande vgextend <nom_vg> <nom_pv> pour ajouter le nouveau disque au groupe de volumes


#### Question 9 : Utilisez la commande lvresize (ou lvextend) pour agrandir le volume logique. Enfin, il ne faut pas oublier de redimensionner le système de fichiers à l’aide de la commande resize2fs.

## Exercice 3. Exécution de commandes en différé : at et cron

#### Question 1 : Programmez une tâche qui affiche un rappel pour la réunion qui aura lieu dans 3 minutes. Vérifiez entre temps que la tâche est bien programmée.

#### Question 2 : Est-ce que le message s’est affiché ? Si la réponse est non, essayez de trouver la cause du problème (par exemple en vous aidant des logs, du manuel...)

#### Question 3 : Pour tester le fonctionnement de cron, commencez par programmer l’exécution d’une tâche simple, l’affichage de “Il faut réviser pour l’examen !”, toutes les 3 minutes.

#### Question 4 : Programmez l’exécution d’une commande tous les jours, toute l’année, tous les quarts d’heure

#### Question 5 : Programmez l’exécution d’une commande toutes les cinq minutes à partir de 2 (2, 7, 12, etc.) à 18 heures les 1er et 15 du mois :

#### Question 6 : Programmez l’exécution d’une commande du lundi au vendredi à 17 heures

#### Question 7 : Modifiez votre crontab pour que les messages ne soient plus envoyés par mail, mais redirigés dans un fichier de log situé dans votre dossier personnel

#### Question 8 : Videz votre crontab

