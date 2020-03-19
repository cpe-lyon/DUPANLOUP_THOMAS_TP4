# TP4 - Utilisateurs, groupes et permissions
## Dupanloup - Thomas

### Exercice 1

__1. Commencez par créer deux groupes groupe1 et groupe2__

```groupadd groupe1```

```groupadd groupe2```

__2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell__

On utilise les comman
```
useradd u1 -g 1001
useradd u2 -g 1001
useradd u3 -g 1002
useradd u4 -g 1002
```

On éxecute ce script: 
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
