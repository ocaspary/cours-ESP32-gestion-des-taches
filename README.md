# Les interruptions

L’ESP32 offre 32 interruptions pour chaque cœur (toutes ne sont pas utilisées avec le modèle NodeMCU). Chaque interruption a un certain niveau de priorité et peut être caractérisée en deux catégories :

\-        **Les interruptions matérielles**, dites « Hardware interrupts», qui se produisent en réponse à un événement externe. Par exemple, une interruption GPIO (quand on appuie sur un bouton) ou une interruption tactile lié au toucher d’une broche (Touch Interrupt).

\-        **Les interruptions logicielles**, dites « Software interrupts », qui se produisent en réponse à une instruction logicielle. Par exemple, une interruption liée à un simple temporisateur (Simple Timer Interrupt) ou liée à une horloge de surveillance (Watchdog Timer Interrupt).

Les interruptions sont utilisées pour déclencher un évènement. Elles peuvent nous aider à résoudre des problèmes de timing. Surtout, elles nous évitent de vérifier constamment l’état d’une broche. Avec les interruptions, quand un changement se produit (par exemple liée à la valeur d’un capteur), une action est déclenchée par l’appel d’une fonction.

Avec l’Arduino IDE, pour mettre une interruption, on utilise la fonction _**attachInterrupt()**_ avec les paramètres d’entrée suivants :

**attachInterrupt( GPIOPin , ISR, Mode) ;**

GPIOPin : définit la broche GPIO comme une broche d’interruption que l’ESP32 doit surveiller

ISR (Interrupt Service Routine) : est le nom de la fonction qui sera appelée chaque fois que l’interruption sera déclenchée

Mode : définit le mode de déclenchement de l’interruption. 5 modes sont prédéfinies :&#x20;

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p><em>GPIO Interrupt Modes</em></p></figcaption></figure>

Normalement, on devrait aussi utiliser la fonction _**digitalPinToInterrupt()**_, pour mettre une broche GPIO actuelle comme broche d’interruption, soit :

**attachInterrupt(digitalPinToInterrupt(GPIO), function, mode);**

Quand vous ne voulez plus que l’ESP32 surveille une broche, vous pouvez appeler la fonction _**detachInterrupt()**_ avec la syntaxe suivante :

**detachInterrupt(GPIOPin) ;**

&#x20;

La routine de service d’interruption (ISR) est invoquée quand une interruption se produit sur n’importe quelle broche GPIO. Sa syntaxe est la suivante :

**Void IRAM\_ATTR ISR() {**

&#x20;           **Statements ;**

**}**



Dans l’ESP32, les ISRs possèdent des fonctions spéciales qui ont des règles uniques que la plupart des autres fonctions n’ont pas :

\-        L’ISR doit avoir un temps d’exécution le plus court possible parce qu’il bloque l’exécution normale du programme

\-        L’ISR doit posséder l’attribut IRAM\_ATTR d’après la documentation de l’ESP32

En signalant une partie de code avec l’attribut **IRAM\_ATTR**, nous déclarons que le code compilé sera placé dans la mémoire RAM interne (Internal RAM ou IRAM) de l’ESP32, autrement le code est placé en mémoire Flash et cette dernière est beaucoup plus lente que la RAM interne.

En effet, si le code que nous voulons exécuter est une ISR, nous voulons que son exécution soit la plus rapide possible. Celle-ci risque d’être compromise si nous devons attendre que le code soit chargé à partir de la mémoire Flash.
