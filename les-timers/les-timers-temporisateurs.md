# Les timers (temporisateurs)

Quand nous utilisons la fonction **delay()**, le programme est bloqué et doit attendre la fin d’exécution de cette fonction. Dans le cas où il s’agit de la seule tâche à exécuter, pas de souci, mais si nous avons besoin que plusieurs tâches se produisent au même moment, nous ne pouvons pas employer **delay().**

C’est pourquoi, dans la plupart des projets, les timers sont employés à sa place, de manière à pouvoir déclencher des évènements par interruptions. Les timers s’appuient sur la fonction **millis()** qui est un compteur qui retourne le nombre de millisecondes écoulées depuis que le programme a été lancé.

Attention, le compteur de millisecondes est au format _**unsigned long int**_, donc sur 32 bits. Il est initialisé au reset et revient à zéro au bout de 232 ms, soit 49,71 jours environ, disons 50 jours. En s’y prenant correctement, le retour à zéro du compteur ne pose aucun problème et les temporisations pourront fonctionner indéfiniment. Respecter la manière de faire de l’exemple **Blink\_Without\_Delay.ino** ci-après.

Imaginons qu’on souhaite déclencher une action périodiquement toutes les 30 secondes.

On déclare une variable pour stocker la valeur du compteur au moment au fait l’action.

On déclare une variable contenant l’intervalle convertie en millisecondes ou une constante si on ne souhaite pas modifier l’intervalle.

Ces deux variables sont de type **unsigned long** pour pouvoir contenir des valeurs sur 32 bits.

```
unsigned long previousMillis=0 ;
unsigned long interval = 30000L;

```

On peut initialiser previousMillis à zéro ou bien avec **millis()**.

Dans la boucle infinie (fonction loop), on compare la valeur courante du compteur (millis()) à sa valeur lors de la dernière action ( previousMillis ). Lorsque l’écart dépasse l’intervalle (l’écart ne pourra pas dépasser de plus de 1ms si votre boucle dure moins de 1ms, si elle dure plus, le retard maximum pourra être égal à la durée de la boucle), on déclenche l’action et on sauve la valeur du compteur dans previousMillis. La durée de l’action doit être très inférieure à l’intervalle.

```
void loop() {
  .....
  if( millis() - previousMillis >= interval) {
    previousMillis = millis();   
    DoAction() ;
    }
....
}

```

Règles à respecter pour le bon fonctionnement d’un timer :

\-        Utiliser des variables de type unsigned long

\-        Travailler par soustraction entre la valeur du compteur et la valeur qu’il avait lors de la dernière action

\-        Tester votre programme en ajoutant une valeur proche du débordement

(**Référence** : [http://arlotto.univ-tln.fr/arduino/article/utilisez-correctement-la-fonction#nb2](http://arlotto.univ-tln.fr/arduino/article/utilisez-correctement-la-fonction#nb2) )
