---
layout: post
title: "Static blogging"
date: 2013-01-24 01:24
comments: true
categories: 
---
J'ai voulu me lancer dans la grande aventure qu'est le blogging fin 2012. Je n'en avais jamais fait auparavant et c'est un passe-temps qui me plait mais qui prend vraiment beaucoups de temps. De plus, je n'avais pas envie de faire un wordpress c'est trop mainstream et c'est du PHP (c'est contre ma religion).

Donc j'ai regardé les alternatives existantes. J'ai eu l'idée de dev mon blog en scala pour tout d'abord faire du scala et puis parce que j'aime bien dev. Mais n'ayant pas tant de temps libres (trop de meetup et jug/hug/psug...), je me suis rabatu sur une solution qui m'a paru logique au vu du nombre d'articles que je mettrais sur ce blog : un blog statique!

On écrit les articles sur sa machine, on lance de la dark magic, ça génère un site statique que l'on peut déployer sur n'importe quel hébergement en ligne sans se soucier du support du langage.

L'application s'appelle Jekyll. J'ai débuté avec en m'essayant à bootstrap par la même occasion. Mais ne connaissant pas Ruby et n'ayant aucune connaissance en design web, j'ai cherché des thèmes pour Jekyll, j'ai découvert Octopress une surcouche à Jekyll qui est principalement fait pour les github pages. 

Il possède plusieurs thèmes facilement personalisables et responsives. En Ruby, rake est l'équivalent de make mais avec une syntaxe Ruby. Les principales commandes sont generate (compile articles), preview (launch local server) and deploy (deploy generated files on gh-pages branch).

After that, data will be published on github pages which are rendered and served like a normal host. You can also configure a dns for your github pages : instead of user.github.com you can use your own dns by putting it in a file CNAME on root of your repository.

