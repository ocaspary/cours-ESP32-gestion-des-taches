# Les timers hardware (temporisateurs matériels)

Après la fonction **delay()**, après un timer « software », nous allons aborder maintenant  le timer « matériel », appelé HardwareTimer. En effet, l’ESP32 a 2 groupes de timers avec, pour chaque, 2 timers hardware (implantés dans l’ESP32), soit 4 timers en tout. Tous les timers utilisent des compteurs de 64 bits tandis que les diviseurs, dits « prescalers », sont eux sur 16 bits.

Comme son nom l’indique, le diviseur sert à diviser la fréquence d’un signal de base, habituellement 80 MHz pour l’ESP32, puis à incrémenter ou décrémenter le compteur du timer. Etant sur 16 bits, il peut diviser la fréquence d’horloge de 2 à 65526 (216). Des alarmes peuvent également être générées automatiquement quand une certaine valeur, définie logiciellement, est atteinte.

&#x20;Nous emploierons les fonctions suivantes :

**hw\_timer\_t \* timerBegin(uint8\_t num, uint16\_t divider, bool countUp)**

**num**: is order of Timer. We only have 4 timers so the order can be 0,1,2,3.

**divider**: it is a prescaler. To make 1 second scheduler, we will use divider value is 80. ESP32 main clock is 80MHZ so every tick will take T = 1/(80MHZ/80) = 1 microsecond. We need 1000000 ticks for 1 second.

**countUp**: if it is true the timer will count up and vice versa.

&#x20;

**void timerAttachInterrupt(hw\_timer\_t \*timer, void (\*fn)(void), bool edge)**

**fn**: is the callback function that will be invoked when timer timeout and timer interrupt will be invoked.

**edge**: if it is true, an alarm will generate an edge type interrupt.

&#x20;

**void timerAlarmWrite(hw\_timer\_t \*timer, uint64\_t alarm\_value, bool autoreload)**

**alarm\_value**: we set it to 1000000 as calculated above

**autoreload**: if it is true, timer will repeat.

&#x20;

**void timerAlarmEnable(hw\_timer\_t \*timer)** : enable the Timer.
