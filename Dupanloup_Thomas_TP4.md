# TP4 - Utilisateurs, groupes et permissions
## Dupanloup - Thomas

### Exercice 1

__1. Commencez par créer deux groupes groupe1 et groupe2__

```groupadd groupe1```

```groupadd groupe2```

__2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell__

On utilise les commandes
```
useradd u1 -g 1001
useradd u2 -g 1001
useradd u3 -g 1002
useradd u4 -g 1002
```

On execute ce script: 
```
chmod u+x users.sh
./users.sh
```

__4. Donnez deux moyens d’afficher les membres de groupe2__

La premiere technique consiste à utiliser la commande ``` members groupe2 ```
La seconde technique consiste à utiliser le script suivant : 
```bash 
#!/bin/bash

groupid=$(cat /etc/group | cut -d: -f3 | grep $1);
groupid=$( printf ':%01d' $groupid );
cat /etc/passwd  | cut -d: --fields=1,4 | grep "$groupid" >/tmp/.list_users ;
cat /tmp/.list_users | cut -d: -f1
rm /tmp/.list_users;
```

__5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4__

On utilise les commandes suivantes : 
```bash
sudo chgrp groupe1 /home/u1 /home/u2
sudo chgrp groupe2 /home/u3 /home/u4
```


__6. Remplacez le groupe primaire des utilisateurs :__ 

On utilise les commandes :
``` bash 
usermod u1 -g groupe1 
usermod u2 -g groupe1 
usermod u3 -g groupe2
usermod u4 -g groupe2
```

__7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.__

On uilise les commandes : 
```bash 
sudo mkdir /home/groupe1
sudo chgrp groupe1 /home/groupe1
chmod g+x /home/groupe1

sudo mkdir /home/groupe2
sudo chgrp groupe1 /home/groupe2
chmod g+x /home/groupe2
```

__8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?__

Pour que dans ces groupes seulement l'utilisateur propriétaire du fichier puisse écrire dans le fichier il faut utiliser le sticky bit. 
```bash
sudo chmod +t groupe1
sudo chmod +t groupe2
```
__9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?__

Il n'est pas possible de se connecter car aucun mot de passe pour cette utilisateur a été configuré. Il n'est donc pas un utilisateur actif. Ainsi il faut configurer un mot de passe. 


__10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.__

Pour activer un compte utilisateur on utilise la commande suivante : 
```sudo passwd u1``` et on entre deux fois le mot de passe que l'on souhaite associer à cet utilisateur.


__11. Quels sont l’uid et le gid de u1 ?__

Lorsque l'on regarde le contenu du fichier /etc/passwd la 3ème colonne correspond à l'uid et la 4ème colonne correspond au gid.
Pour l'obtenir on utilise le script suivant : 
```bash
#!/bin/bash
echo "l uid de  $1 est : $(cat /etc/passwd | grep $1 |cut -d: -f3)";
echo "le gid de $1 est : $(cat /etc/passwd | grep $1 |cut -d: -f4)";
```

__12. Quel utilisateur a pour uid 1003 ?__

Pour savoir quel utilisateur a pour uid 1003 on utilise la commande suivante
```bash cat /etc/passwd | cut -d: --fields=1,4 | grep 1003 | cut -d: -f2
```

__13. Quel est l’id du groupe groupe1 ?__

Pour le savoir on utilise la commande : 
``` cat /etc/group | grep groupe1 | cut -d: -f3 ```

__14. Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)__

Pour connaitre le group dont le gid est 
```  cat /etc/group | grep ":1001:" | cut -d: -f1 ``` 

__15. Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.__

On utilise la commande ```sudo gpasswd -d u3 groupe2``` pour supprimer l'utilisateur u3 du groupe 2.
Cependant cela nous renvoit que u3 n'est pas membre de groupe2.
Cela signifie que groupe2 n'est pas un groupe secondaire de u3. En effet il s'agit bien de son groupe principal. 

__16. Modifiez le compte de u4 de sorte que :__

— il expire au 1er juin 2020
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de passe

On utilise la commande suivante : ```sudo chage -d 06/01/20 -M 90 -m 5 -W 14 -E 06/30/2020 ```

__17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?__

On obtient cette information en utilisant la commande : 
```cat /etc/passwd | grep root | cut -d: -f7```

L'autre solution est de devenir root en utilisant la commande  ```su``` 
Puis on utilise la commande :
```echo $SHELL```
On obtient que le shell utilisé est bien ```/bin/bash```

__18. à quoi correspond l’utilisateur nobody ?__

L'utilisateur nobody est l'utilisateur avec le moins de permissions du systeme. Initialement cet utilisateur était utilisé pour des raisons de sécurité. Si celui ci se fait hacker les dégats sont très limités car il ne dispose pas d'un shell et de droit sur aucun fichiers. 


__19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?__

Dans une configuration standard la commande sudo garde en mémoire notre mot de pass pour 15 minutes.
Si l'on souhaite supprimé le mot de passe mise en mémoire par la commande sudo on utilise l'option -k. 


### Exercice 2

__1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques
lignes de texte. Quels sont les droits sur test et fichier ?__

On créer un dossier dans notre répertoire $HOME ```mkdir /home/admin/test``` 
On créer un fichier avec la commande : ```echo "ceci est un fichier test" > fichier ```

__2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ? __

On supprime tous les droits associé au fichier avec la commande suivante  :
```bash sudo chmod ugo-rwx fichier ```

On étant root on peut encore modifier et lire le fichier. 

__3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo
Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un
fichier s’il existe déjà. Que peut-on dire au sujet des droits ? __

On se redonne les droits avec la commande ```sudo chmod u+wx fichier ```
On écrit avec ```echo "hello" > fichier```
On remarque que cela fonctionne bien cependant on ne peut toujours pas lire le fichier. 
Cela veut dire qu'écrire correspond aussi à remplacer le contenu.

__4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez__ 

On ne peut pas executer le fichier sans pouvoir le lire. En effet l'interperteur a besoin de lire le fichier afin de l'executer. 

__5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?
Rétablissez le droit en lecture sur test__

Lorsque l'on execute la commande ```ls test``` après avoir enlevé le droit de lecture sur le fichier on se rend compte que nous n'avons pas l'autorisation de lire le contenu du dossier.
Cependant nous pouvons l'ouvrir et voir que notre fichier est présent. Il est possible d'en voir le contenu si ce fichier est lisible par l'utilisateur. 
On peut aussi l'executer si nécéssaire. 

__6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au
répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit
en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?__

Avec ce nouveau fichier on peut en voir le contenu et l'ouvrir avec un éditeur de texte comme nano. Cependant on ne peut pas écrire dedans et donc le modifier. On ne peut pas non plus le supprimer. Cependnant si on donne les droits d'écriture au dossier test on se rend alors compte que le fichier nouveau n'est toujours pas modifiable mais qu'il est bel et bien supprimable. 

__7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.
Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en
lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?__

Lorsqu'un dossier n'est plus executable on ne peut plus l'ouvrir mais on ne peut aussi plus créer de fichier à l'interieur de ce dernier ni en supprimer. 
On peut donc en conclure que le droit d'execution sur un dossier le rend completement imperméable par rapport à l'extérieur. 


__8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui
à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire
test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des
droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd
..” ? Pouvez-vous donner une explication ?__

On redonne les droits avec la commande ```chmod a+x test``` puis une fois dedans on supprime les droits d'executions avec la commande ```chmod a-x .```. 
On ne peut plus voir le contenu du dossier. On ne peut plus non plus acceder au dossier sstest ni en lister le contenu. On peut en conclure que même si l'on arrive à se retrouver dans un dossier ou ne sommes pas censé être, si nous ne possédons pas les droits sur ce dossier nous ne pouvons rien faire dessus. 
Tout de même nous pouvons utiliser la commande ```cd ..``` , on peut suppose que "../" n'est pas associé a "./" et que donc les droits pour l'executions de la commande cd sont ceux du dossier parent du dossier courant. 

__9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.__

Pour qu'une autre personne puisse acceder au dossier en lecture mais pas en écriture. On utilise l'octal avec la commande chmod de la façon suivante : ```chmod 750 test```


__10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.__ 

 On utilise les commandes suivante afin de paramétrer notre masque: 
```bash 
umask u+rwx
umask g-rwx
umask o-rwx
```

Ensuite lorsqu'on créer un fichier avec ```touch test/nouveau``` ou alors un dossier avec ```mkdir test/ssdir``` notre fichier est bien les permission read et write uniquement pour l'utilisateur propriétaire et pas de permission execute. Alors que notre dossier a bien read write et execute uniquement pour l'utilisateur propriétaire.

__11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.__ 

On créer le masque répondant aux attentes avec la commande ```umask 0022```

__12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux
membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.__ 

On créer le masque désiré avec la commande ```umask 0033```

__13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :__ 

-- chmod u=rx, g=wx , o=r fic devient chmod 0534

-- chmod uo+w, g-rx fic en sachante les droits initiaux de fic sont r--r-x---
	Devient chmod 0602

-- chmod 654 fic en sachant que les droits initiaux de fic sont 711
	Devient u=rw,g=rx,o=wx fic

-- chmod u+x, g=w, o-r fic en sachant que les droits initiaux de fic sont r--r-x---
	Devient chmod 520 fic


__14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier
/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?__ 

On utilise la commande : ```ls -l /usr/bin/ | grep " passwd"```

On lit ainsi que les droits de la commande passwd sont -rwsr-xr-x

Maintenant lorsqu'on regarde droits du fichier /etc/passwd on utilise la commande :
``` stat /etc/passwd``` 
On obtient le résultat -rw-r--r-- 

Cela est logique : Les utilisateurs qui ne sont pas root peuvent lire et executer le programme mais pas le modifier. De plus le fichier /etc/passwd peut être modifié uniquement par un root mais reste lisible par tout le monde. 







