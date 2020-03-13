# TP4 - Utilisateurs, groupes et permissions
## Dupanloup - Thomas

### Exercice 1

__1. Commencez par créer deux groupes groupe1 et groupe2__

```groupadd groupe1```

```groupadd groupe2```

__2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell__

On créé un fichier avec nano qui contient les commandes à éxecuter pour obtenir la répartition voulue:

```
#!/bin/bash
useradd u1 -g 1001
useradd u2 -g 1001
useradd u3 -g 1001
useradd u2 -g 1002
useradd u3 -g 1002
useradd u4 -g 1002
```
On éxecute ce script: 
```
chmod u+x users.sh
./users.sh
```

__4. Donnez deux moyens d’afficher les membres de groupe2__

cat /etc/passwd

Pour connaître tous les utilisateurs / tous les groupes, il faut regarder la première
colonne des fichiers /etc/passwd / /etc/group 2
cut -d: (sépare en colonne à chaque ":") -f1-4 /etc/passwd (affiche les 4 premières colonnes de etc/passwd)


__5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4__



__6. Remplacez le groupe primaire des utilisateurs :__ 



__7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.__



__8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?__



__9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?__



__10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.__



__11. Quels sont l’uid et le gid de u1 ?__

__12. Quel utilisateur a pour uid 1003 ?__

__13. Quel est l’id du groupe groupe1 ?__

__14. Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)__

__15. Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.__

__16. Modifiez le compte de u4 de sorte que :__
— il expire au 1er juin 2020
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de passe

__17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?__

__18. à quoi correspond l’utilisateur nobody ?__

__19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?__
