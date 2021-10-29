---
layout: post
title:  "Comment publier vos librairies sur github package registry ?"
summary: "Dans ce mini tutoriel, je vous montre comment publier votre librairie open source sur github packages en quelques étapes"
author: cenyo
date: '2021-10-28 14:35:23 +0530'
category: Tutoriel
thumbnail: /assets/img/posts/package-registry.png
keywords: Github, Actions Github, maven project,comment publier sur github packages, github package registry
permalink: /blog/comment-publier-sur-github-packages/
usemathjax: true
---

Vous avez réalisé une librairie que vous voulez mttre à disposition de tout le monde mais vous ne savez pas par oû commencer ? Github packages est l'une des solutions 
qui s'offre à vous.

C'est quoi github package registry?

C'est un service de github permettant de conditionnant votre livrable (librairie) dans un registre permettant plus facilement sa publication et son exploutation de manière privée ou publique.

Il supporte la plupart des outils de gestions de paquets familiers à savoir NPM, RubyGems, NuGet, Maven et bien d'autres.

L'exemple que nous allons voir aujourd'hui a été réalisé avec Maven, c'est celui que j'utilise le plus.

Prérequis :

- Un projet java déjà fonctionnel
- Intégrer Maven comme outil de gestion des paquets sur votre projet.
- Votre projet téléversé sur votre repository github


## Etape 1: Préparer votre le pom.xml de votre projet

Rajouter la balise ```<distributionManagement> ```:

```xml
    <distributionManagement>
        <repository>
            <id>github</id>
            <name>GitHub ccenyo Apache Maven Packages</name>
            <url>https://maven.pkg.github.com/ccenyo/Gfycat4J</url>
        </repository>
    </distributionManagement>
```

*  ```<id> ``` est importante puisqu'elle sera utilisé plus tard dans le fichier de configuration de maven settings.xml

* ```<url> ``` fait référence à votre repository github : https://maven.pkg.github.com/USERNAME/REPO_NAME. Dans mon cas USERNAME = ccenyo et REPO_NAME = Gfycat4J.



## Etape 2: Créer votre action github

Une action github vous permet de lancer des processus grâce à des évenements tels que push, pull requests ou création d'une nouvelle version.
Celà nous permettra dans ce cas, de créer un package dès que nous créons une nouvelle version de notre librairie sur github.

Créer un fichier ``maven-publish.yml `` dans le dossier ``.github/workflows`` à la racine de votre projet.


`````yaml
name: Publish package github packages registry
on:
  release:
    types: [created]
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Packages Repository
        uses: actions/setup-java@v2
        with:
          java-version: '15'
          distribution: 'adopt'
      - name: Publish package
        run: | 
          rm ~/.m2/settings.xml
          echo "<settings><servers><server><id>github</id><username>ccenyo</username><password>${GITHUB_TOKEN}</password></server></servers></settings>" > ~/.m2/settings.xml
          mvn --batch-mode deploy
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

``````

Il est primordiale que vous modifiez votre fichier settings.xml pour y integrer la balise ``<server>`` correspondant à github. Ce code a déjà été intégré à au fichier ``maven-publish.yml``.

Dans votre ``server.xml``
````xml
<settings>
  <servers>
    <server>
      <id>github</id>
      <username>ccenyo</username>
      <password>${GITHUB_TOKEN}</password>
    </server>
  </servers>
</settings>
````

``${GITHUB_TOKEN}`` est un code automatiquement généré par github au début de chaque exécution. Vous n'avez pas à vous souciez de ça.

## Etape 3: Créer une version 

Suivez les étapes suivantes :

![github-repo-page](/assets/img/posts/tuto1.png)


Créer une nouvelle version 

![github-release-page](/assets/img/posts/tuto2.png)

Vous verrez votre action s'exécuter automatiquement après la création de votre nouvelle version. N'oublier pas de mettre à jour votre numéro de version dans votre fichier ``pom.xml`` 


![github-publish-page](/assets/img/posts/tuto3.png)

Vous trouverez votre librairie déployé sur le registry.

![github-release](/assets/img/posts/tuto4.png)


# Conclusion

 Il y a beacoup de choses à dire sur les actions github, j'y reviendrai surement dans d'autres tutoriels.

Sources du projet qui a servi d'exemple.

https://github.com/ccenyo/Gfycat4J
