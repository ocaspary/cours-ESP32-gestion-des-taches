# Exercice machine à états avec Touch

**Montage et exercice à réaliser : Gestion de la LED bleue**

Utiliser trois broches « Touch sensitive » de l’ESP32 pour gérer la LED bleue.

Quand vous touchez un fil relié à une broche (entre deux doigts), la LED bleue clignote tout le temps sur une période de 2 secondes (1 seconde allumée, 1 seconde éteinte).

Quand vous touchez un autre fil relié à une autre broche, la LED bleue clignote alors plus vite sur une période de 1 seconde (½ seconde allumée et ½ seconde éteinte).

Enfin, avec le troisième fil, vous retournez à l’état ou la LED bleue clignote toutes les 2 secondes.

Voir les exemples précédents. **Réfléchir à la structure de votre code avant de programmer**.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Trois fils pour le montage : touch1 à GPIO13, touch2 à GPIO12, touch3 à GPIO14

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Vous avez 3 états :

\-        L’état initial, l’état avec un clignotement de 2 s. et un état avec un clignotement d’1 s.

Pour changer d’états, vous avez 3 transitions réalisées par 3 fils à 3 broches « Touch sensitive » :

\-        Touch 1, Touch 2, Touch 3

Fonctions utiles : touchAttachInterrupt(), timerBegin(), timerAttachInterrupt(), timerAlarmWriter(), timerAlarmEnable(), void IRAM\_ATTR onTimer().

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Photo d'illustration</p></figcaption></figure>
