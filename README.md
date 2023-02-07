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

Ensuite il suffit de lancer la commande _defectdojo_ sous le terminal pour que l'application se déploie (y compris via un service systemd)

## Accès à l'inteface graphique


