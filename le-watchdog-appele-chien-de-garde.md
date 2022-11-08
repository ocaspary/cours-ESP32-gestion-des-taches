# Le watchdog (appelé chien de garde)

En bon français, on devrait parler de **temporisateur de surveillance** pour « watchdog timer » ou WDT. C’est un circuit électronique ou un logiciel utilisé en électronique numérique pour s'assurer qu'un automate ou un ordinateur ne reste pas bloqué à une étape particulière du traitement qu'il effectue (source Wikipédia).

En principe un temporisateur de surveillance doit réagir rapidement à un dysfonctionnement matériel ou logiciel d'un dispositif en rétablissant son fonctionnement normal, le plus souvent, en effectuant une réinitialisation. Et comment vérifier que le dispositif fonctionne correctement ? Ce dernier doit confirmer à intervalles réguliers que la situation est normale. Si ce signalement n'est pas reçu dans un délai prévu, le système de surveillance passe à l'action.

Un chien de garde peut être vu comme un compteur qui est régulièrement décrémenté. Il « mord » quand la valeur du compteur atteint zéro. Il est « alimenté » quand le compteur est remis à sa valeur initiale. Tant que le logiciel d'un dispositif comme un microcontrôleur alimente le chien de garde avant que la valeur critique du compteur ne soit atteinte, le logiciel peut continuer avec ses autres tâches normales. Si une erreur de programmation, une surtension transitoire, ou autre, écarte le logiciel de son fonctionnement prévu, vraisemblablement ce dernier n'alimentera plus le chien de garde et conséquemment l'appareil sera réinitialisé.

L’ESP32 dispose de 2 types de watchdogs :

\-        le watchdog timer d’interruption ou IWDT (pour Interrupt WDT)

\-        le watchdog timer de tâches ou TWDT pour (Task WDT)

Le watchdog d’interruption est responsable de la détection des cas où la commutation des tâches de FreeRTOS est bloquée pendant une période prolongée. Sans commutation, aucune autre tâche ne peut être exécutée, comme la tâche Wifi par exemple, car elle ne peut pas obtenir du temps CPU pour son exécution. Ce cas de figure se produit quand un programme est exécuté dans une boucle infinie avec des interruptions désactivées ou bien « coincé » dans une interruption. Dans un environnement de production, on préférera faire un reset de l’ESP32. Travail du IWDT :

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Le watchdog de tâches (TWDT) est chargé de détecter les cas de tâches exécutées sans qu’il y ait la moindre modification de programme pendant une période prolongée, autrement dit des tâches « inactives », des tâches coincées dans des boucles infinies. Il faudra définir les tâches que le TWDT devra surveiller. Chaque tâche peut être individuellement remise à zéro (reset).

Autrement dit, les watchdogs jouent un rôle important dans la stabilité de l’ESP32.

Vérifiez que vous ne restez pas trop longtemps dans la boucle **loop()** de votre programme pour l’ESP32 (IDE Arduino). En effet, quand vous la quittez pour y rentrer à nouveau, le logiciel qui gère votre ESP32 effectue plusieurs tâches en arrière-plan et réinitialise votre watchdog timer d’interruption (IWDT).

Au niveau du code, ne jamais écrire :

void loop() {

&#x20; while(1) {

&#x20;   do\_some\_work();

&#x20; }

}



Mais écrire :

void loop() {

&#x20; do\_some\_work();

}

&#x20;

Et si vous devez vraiment faire plus de travail dans la boucle **loop()**, et que le temporisateur de surveillance (watchdog timer) le permet, assurez-vous d’appeler de temps en temps les fonctions **yield()** ou **delay()**, qui « nourrissent » le chien de garde et donc remettent à zéro le timer WDT. Sinon, quand le temporisateur du watchdog arrive à zéro, un reset se produira.

Exemple intéressant de mise en œuvre du WDT, voir le programme ci-après et l’explication :

[https://iotassistant.io/esp32/enable-hardware-watchdog-timer-esp32-arduino-ide/](https://iotassistant.io/esp32/enable-hardware-watchdog-timer-esp32-arduino-ide/)
