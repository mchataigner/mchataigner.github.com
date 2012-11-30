---
layout: post
title: "Shipper des API, du Web avec Scala et de l'Android en mode hackathon"
description: ""
category: "community"
tags: ["scala","android","hackathon"]
comment: true
---
<!--{% include JB/setup %}-->

<!--Retour sur la soirée ‘Shipper des API, du Web avec Scala et de l'Android en mode hackathon’-->

Ce jeudi 21 novembre une quinzaine de personnes se sont retrouvées pour une soirée en mode ‘hackathon’ animée par @un_Jon, @Piwai et @Randhindi.
Le but de la soirée était de nous présenter dans les grandes lignes ce qu’est un hackathon et de nous donner billes pour y être un minimum préparé. 

##Un hackathon, qu’est-ce c’est?
La plupart du temps, c’est un événement qui se déroule sur un week-end. Cela se présente sous la forme d’un concours entre plusieurs équipes composées de 1 à 5 personnes. Chaque groupe a entre 24-36h pour développer une application, la mettre en production et la présenter devant un jury. À la différence des startup-weekend, ici il faut que l’application soit fonctionnelle pour être présentée. Très souvent, des sponsors sont rattachés à ces événements et c’est l’occasion de gagner de nombreux lots. Par exemple, AngelHack, un hackathon organisé à l’échelle internationale le week-end du 1er décembre 2012 à Paris, offrira à l’équipe gagnante un séjour de 6 semaines pour la Silicon Valley pour y rencontrer des investisseurs, des médias et des boîtes orientées technologies. Il est intéressant de noter que plus de 30% des finalistes trouvent des financements et montent des startups à la suite de ces événements.

###Quelques conseils

Voici quelques conseils pour tous ceux qui auraient envie de se lancer:

- **L’équipe**: former une équipe équilibrée avec 1 product manager, 2 front et 2 back. Le product manager prend tout son sens pour la coordination du front (UI + UX), back (API), et c’est lui qui aura la lourde tâche de faire la présentation/démo devant le jury. Pas besoin d’y aller avec une équipe déjà constituée, les participants se répartissent sur place en fonction de leurs idées de produit. Faire attention à avoir une application modulaire, pour pouvoir couper une partie à tout moment et faciliter la répartition des tâches au sein de l’équipe. Il est important aussi de ne pas rester du début à la fin sur les mêmes features et communiquer régulièrement pour voir où on en est !

- **Les technos**: Utiliser une technologie que l’on connaît (pour éviter de passer son temps à lire la doc), enlever la partie test (pour gagner du temps, l’objectif est le prototype!), et les println suffisent amplement ! Enfin, venir avec son environnement de développement prêt à travailler en préparant en amont le squelette d’une application (combien de temps on perd à tout configurer, IDE, maven/sbt, dépendances...).

- **Le produit**: choisir un produit simple et cool (KISS) pour avoir un ‘WOW effect’ durant la présentation et choisir un nom qui claque. Penser à soigner les UI+UX, car c’est la première chose que le jury va voir. 

- **Le pitch**: c’est le rôle du PM ! Commencer à penser à la présentation du produit bien avant la fin de la réalisation, et ne pas hésiter à adapter son discours suite aux réactions du jury face aux autres produits (si on ne passe pas en premier bien sûr :) ).

- **Autres conseils pratiques**: faire des pauses régulièrement, dormir mais pas trop, ne pas manger trop lourd...et éviter de boire trop de redbull.


##Maintenant c’est à nous de coder!

A cette soirée découverte hackathon, @un_Jon (maître Monoïd) et @Piwai (maître Android) ont chacun défendus leur technologie.

###Pourquoi Scala?
Scala est un langage moins verbeux que Java, typesafe, avec des fonction de plus haut niveau sur les collection notamment. Il permet aussi d’obtenir un résultat rapidement sans avoir recours à des frameworks “exotiques”. Combiné avec Unfiltered, on peut shipper une API web REST en idiomatic Scala. Play2 peut aussi apporter des solutions intéressantes avec les iteratees. Attention à ne pas se risquer à utiliser une version trop récente de Scala\* (comme la 2.10), qui peut être instable et offre des fonctionnalités qui n’apporteraient rien de plus. En résumé, Scala ça poutre\*\* !

\* A noter que le mot monoïd n’a étonnement pas été prononcé...

\*\* citation bien connue

###Pourquoi SBT?
Parce que cet outil de gestion de dépendances s’intrègre totalement avec Scala et qu’il offre une console qui roxxe des mamans ours. Inutile donc de s’embêter avec Maven.

###Pourquoi Android?
Parce que c’est l’OS qui est en train de devenir maitre du monde! Et que cette soirée était organisé par @Piwai (très bon professeur Android). Plus sérieusement, il faut garder en tête, que ce que le jury verra en premier, c’est le visuel de l’application. Inutile de partir dans des choses trop compliquées, il faut rester simple et limiter le cadre dès le début. La rotation de l’écran est une feature pas évidente, on peut tout à fait s’en passer pour un hackathon en figeant les écrans. Pour le squelette, penser à androidkickstartr.com, pour les libs android-ui-utils.googlecode.com, androiduipatterns.com, Spring RestTemplates ou encore androidannotations.org.


Enfin, s’il y a une idée à garder en tête, c’est celle-ci:  **il faut que le produit marche! Pas besoin de fournir un super beau code!**



##L’implémentation (partie scala uniquement)

Passons maintenant à l’implémentation. Nous sommes partis d’une application déjà existante, avec des fonctionnalités à rajouter ou compléter. Les squelettes sont d’ailleurs disponibles sur GitHub pour les parites scala et Android.
Pour suivre le format d’un hackathon, nous avons formé des petits groupes de 4 personnes et nous nous sommes répartis des tâches après avoir établi ce qui nous semblait être le plus prioritaire.


Voici tout d’abord une rapide présentation de l’application.

###Le squelette de l’application

Ce soir, il s’agit d’implémenter une application nommée ‘kplaner’ qui propose un doodle pour choisir une/des séance(s) de cinéma. Elle est composée d’une partie UI en Android, et d’une partie serveur en Scala avec l’API JSON. La partie Web Service REST va être gérée grâce à le toolkit Unfiltered.

Les spécifications:
- je peux choisir une date et un ciné
- on peut me proposer la liste des films qui sont autour de ces horaires, je peux checker ceux qui me branchent
- je peux envoyer des invitations à mes potes par tous les moyens qui me semblent bons

###Unfiltered
Voici une rapide présentation de Unfiltered:

- la déclaration d’un web service avec l’opération HTTP Get et une réponse au format JSON s’écrit sous forme d’un contrat (intent) et ses chemins :

{% highlight scala %}

/** unfiltered plan */
trait KApp extends unfiltered.filter.Plan {
  self: KinoService with KPlanerCreationService =>

  def intent = {
    case GET(Path("/kinolist")) => jsonResponse(listKinos())
    case GET(Path("/filmlist")) => jsonResponse(listFilms())
    case GET(Path("/sessions")) => jsonResponse(listShowing())
  }
}
{% endhighlight %}

- le lancement du serveur:

{% highlight scala %}

/** embedded server */
object Server {
  def main(args: Array[String]) {
    val http = unfiltered.jetty.Http(8080) // this will not be necessary in 0.4.0
    http.context("/assets") { _.resources(new java.net.URL(getClass().getResource("/www/css"), ".")) }
      .filter(new KApp with InMemoryKinoFixtureService with InMemoryKplanerCreationService).run({ svr =>
    {
      unfiltered.util.Browser.open(http.url)
    }
      }, { svr =>
        println("shutting down server")
      })
  }
}
{% endhighlight %}

Un des avantages de Scala, est la possibilité de mettre du code xml/html inline sans passer par des strings comme le montre l’exemple suivant :

{% highlight scala %}
case GET(Path("/newdoodle")) => Ok ~> HtmlContent ~> Html(
    <html><body><form action="/kplaner" method="POST">
      <input type="text" name="kinoid"/>
      <input type="text" name="date"/>
      <input type="submit"/>
    </form></body></html>
  )
{% endhighlight %}

###Notre production

Après quelques petits problèmes de configurations d’environnement (n’est-ce pas @nivdul :D), on est rentré dans le code.

Nous avons identifié 3 améliorations du squelette :

- ajout d’un nouveau doodle (correspondant à un cinéma et une date) à l’aide d’un formulaire.

Aperçu du service de sauvegarde in memory des nouveaux doodles créés:

{% highlight scala %}
trait InMemoryKplanerCreationService extends KPlanerCreationService {
  lazy val kplanerMap = new collection.mutable.HashMap[String, Kplaner]()

  def newKplaner(kino: Kino, date: DateTime) = {
    val uid = UUID.randomUUID().toString
    val kplaner = Kplaner(uid, kino, date, Set.empty)
    kplanerMap += uid -> kplaner
    kplaner
  }
…
}
{% endhighlight %}

- affichage d’une liste filtrée sur un cinéma et une date dans un intervalle de 2h.

{% highlight scala linenos=table %}
case GET(Path(Seg("sessions" :: timestamp :: Nil))) => jsonResponse(listShowing().filter(x =>
  {

    val showingTimestamp = x.datetime.toDate.getTime
    x.datetime.toDate.getTime > timestamp.toLong - ONE_HOUR && showingTimestamp < timestamp.toLong + ONE_HOUR
  }
{% endhighlight %}


{% codeblock %}
case GET(Path(Seg("sessions" :: timestamp :: Nil))) => jsonResponse(listShowing().filter(x =>
  {

    val showingTimestamp = x.datetime.toDate.getTime
    x.datetime.toDate.getTime > timestamp.toLong - ONE_HOUR && showingTimestamp < timestamp.toLong + ONE_HOUR
  }
{% endcodeblock %}

- ajout d’une vue de vote pour le film de son choix.

Par manque de temps, on n’a pas fait le lien entre le formulaire de vote du choix du film d’un doodle et la partie API serveur, mais on y était proche.

###Bonus
Premier soucis au niveau des dates: comment gérer une date sur différentes UI? 
Un très bon article traite ce sujet d’ailleurs. Après plusieurs échanges au seins de l’équipe, il apparaît qu’il est plus simple de passer par des timestamp (nombre de (milli)secondes depuis le 1er Jan 1970) pour les transferts et d’utiliser l’API Joda Time côté Java/Scala.

exemple de code


Pour faciliter les tests\*, à noter que les 2 extensions chrome JSONView et Advanced REST client sont assez simple d’utilisation et ont un rendu plutôt sympa.

\* Penser à pointer sur le bon serveur ;) cc @mchataigner

Enfin pour la partie Web, il aurait été intéressant de rajouter un ‘datepicker’ pour faciliter l’entrée de la date à l’utilisateur, car trouver le datetime de tête... nous on sait pas faire.


##Conclusion

##Ludwine
La soirée a été très intéressante en terme de développement, puisque j’ai appris à implémenter des services REST grâce à Unfiltered. J’ai également pu constater qu’il était assez simple de gérer des formulaires html en scala. Enfin, cette soirée m’a vraiment donné envie de participer à un hackathon.

##Mathieu
C’était une expérience sympa. Du coup, j’ai envie d’aller au hackathon AngelHack. Cependant, je ne suis pas sûr d’être au niveau pour shipper les features assez rapidement pour gagner, je ne suis pas encore fluide en Scala. Mais rappelons-le, l’essentiel c’est de participer. Et puis on a jusqu’au 30 novembre pour s’entraîner. 

Et toi, viendras-tu au AngelHack?