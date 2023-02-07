# defectDojo

## Déploiement

### Présentation

Lors de la vie d’une application, des tests de sécurité (manuels et/ou automatisés) sont réalisés. 

Pour réaliser ces tests, des pentesteurs et/ou des outils rentrent en jeu pour détecter des vulnérabilités. Une fois ces problèmes remontés dans des rapports souvent statiques, il est nécessaire d’assurer un suivi de ces vulnérabilités pour suivre et maitriser l’état de santé d’une application sur l’aspect sécurité.

Pour faciliter cette gestion, des outils d’agrégation/gestion de vulnérabilités comme DefectDojo permettent rendre cette tâche plus facile.

### Prérequis

Je me base sur la version 2022 de Kali, les versions approuvées:

* Kali (ISO ARM) & UTM pour macbook récent
* Kali (ISO amd64) et Virtualbox pour un pc sous linux/windows

Pour vérifier que l'outil defectDojo est bien présent dans les dépôts:

```
apt search defectdojo
```

Puis on l'installe

```
apt install defectdojo
```

Ensuite il suffit de lancer la commande _defectdojo_ sous le terminal pour que l'application se déploie (y compris via un service systemd). Kali précisera alors l'url et le port en écoute pour accès à la GUI, dans mon cas http://127.0.0.1:42003

### Création de l'admin

Nous n'avons pas le user/password par défaut, on va simplement créer un nouveau superuser. Pour cela toujours depuis le terminal

```
cd /usr/lib/defectdojo
sudo -u _defectdojo --python3 manage.py createsuperuser
```

Il nous reste simplement à ajouter un utilisateur, un mot de passe associé (il peut râler si celui ci est trop faible) et ensuite se connecter.

-> il est toujours possible de modifier le mot de passe admin, en utilisant la commande _changepassword_ plutôt que _createsuperuser_

## Accès à l'inteface graphique

Une fois l'interface graphique lancée, on entre donc notre utilisateur fraichement créé


