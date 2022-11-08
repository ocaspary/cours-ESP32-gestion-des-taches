# Usage du watchdog

**Usage du temporisateur de surveillance (watchdog) pour des données de température**

En dehors de la surveillance de l’ESP32, il peut être intégré dans un programme qui transmet des données à distance, par exemple en Wifi.

Exemple d’usage : mettre le WDT à 10 secondes, lire la donnée de température (capteur LM35 ou autre), transmettre cette donnée en Wifi, éteindre l’alimentation (voir les modes Sleep de l’ESP32) pour économiser de l’énergie, puis reset automatique au bout de 8 secondes et on recommence …

**Pour aller plus loin dans l’usage des watchdogs :**

\- Voir la documentation officielle de chez Espressif :

[https://docs.espressif.com/projects/esp-idf/en/latest/api-reference/system/wdts.html](https://docs.espressif.com/projects/esp-idf/en/latest/api-reference/system/wdts.html) &#x20;

Attention, des fonctions disponibles peuvent ne pas fonctionner avec l’IDE Arduino : si nécessaire, écrire votre programme en langage C.

&#x20;\- Nécessité parfois d’un troisième watchdog si l’on ne revient jamais dans la boucle principale **loop()**, voir le sketch avec la fonction BadModule() :

[https://www.sigmdel.ca/michel/program/esp8266/arduino/watchdogs\_fr.html](https://www.sigmdel.ca/michel/program/esp8266/arduino/watchdogs\_fr.html)

&#x20;
