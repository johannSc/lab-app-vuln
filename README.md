# APP Vuln

- [DVWA](#dvwa)
- [NMPA](#nmap)
- [ZAP](#zap)

## DVWA

Le but ici est l'installation d'une application trouée de tous les cotés. Nous n'utiliserons pas les vulnérabilités à proprement parlé, puisque c'est redondant avec Juiceshop. Par contre nous aurons donc une appli qui pourra être auditée avec les outils que nous allons voir plus loin.

Attention, installation ici via docker: https://hub.docker.com/r/vulnerables/web-dvwa

## NMAP

NMAP, le couteau swisse bien utile. NMPA est plutôt associé à des test notamment au niveau du réseau. Cependant il peut être utilisé dans le cadre de scan de CVE et autres vulns, pour cela il faudra installer des scripts supplémentaires.

Installation via les dépôts

```
apt install nmap
```

Installation des scripts:

Nmap-vulners, vulcan et vuln sont les scripts de détection CVE les plus courants et les plus populaires dans le moteur de recherche Nmap. Ces scripts vous permettent de découvrir des informations importantes sur les failles de sécurité du système.

### Nmap-vulners

```
cd /usr/share/nmap/scripts/
git clone https://github.com/vulnersCom/nmap-vulners.git
```

Exemple d'utilisation:

```
nmap -sV --script vulners [--script-args mincvss=<arg_val>] <target> -oX rapport.xml
```

Pour une meilleure lecture on peut passer le rapport en html:

```
xsltproc rapport.xml -o rapport.html
```

### Nmap-vulscan

```
cd /usr/share/nmap/scripts/
git clone https://github.com/scipag/vulscan.git
ln -s `pwd`/scipag_vulscan /usr/share/nmap/scripts/vulscan 
```

Vulscan utilise des bases de données pré-configurées enregistrées localement sur notre machine. Pour mettre à jour la base de données, accédez au répertoire de mise à jour. Tapez la commande suivante dans un terminal pour accéder au répertoire de mise à jour.

```
cd vulscan/utilities/updater/
chmod +x updateFiles.sh
 ./updateFiles.sh
```

Utilisation: 

```
nmap -sV --script vulscan <target>
```

Enfin on peut également appeler plusieurs scripts via une unique commande nmap:

```
nmap -sV --script=exploit,vuln,auth,default localhost -oX 
```

## ZAP

Le scanner plus poussé: 

### Déploiement

Attention, installation ici via les dépôts:

apt install dvwa
dvwa-start

localhost:42001

puis reset de la BDD

### Paramétrage de ZAP

https://augment1security.com/authentication/dvwa-authentication/


## L'outil qui permet de faire des métriques: DerfectDojo

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

### Product

Un product est une application ou un ensemble d’applications qui est testé dans DefectDojo. Elle peut être développée en interne, être développée par un tiers ou être sur étagère.

Pour définir un produit, outre le nom et la description, un ensemble d’informations non obligatoires sont proposées. 

Pour cela rendez vous dans le **Menu de gauche, première case et _Add product_**

Une fois votre product créé, cliquez dessus. Vous arrivez alors sur la page récapitulative


### Benchmark

L'onglet benchmark permet de définir un niveau cible pour l’application d'ASVS (Application Security Verification Standard) ainsi que les règles déjà couvertes 

A noter que pour définir les technologies et langages utilisés par le produit, ** il est nécessaire de passer par l’interface d’administration de Django** :

    Technologies : http://<url_defectdojo>/admin/dojo/app_analysis/
    Langages : http://<url_defectdojo>/admin/dojo/languages/

-> dans notre cas:

* technologies: confidence level 1
* languages: java

### Engagements

Une fois le produit défini, les engagements peuvent être créés. Ils correspondent à un ensemble de tests sur un produit. Cette campagne est définie par une version de l’application à tester, une date de début et de fin, un responsable, un statut et un ensemble d’autres paramètres.

C’est dans ces engagements que les résultats de tests (findings) des différents outils seront importés.

source: https://aymericlagier.com/2019/12/05/gestion-des-analyses-de-securite-avec-defectdojo/

https://subscription.packtpub.com/book/security/9781789802023/14/ch14lvl1sec08/approach-3-security-findings-management-defectdojo


