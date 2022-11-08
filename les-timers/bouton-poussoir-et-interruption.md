# Bouton-poussoir et interruption

Montage :

Réalisez le montage suivant avec simplement un bouton poussoir relié à l’ESP32 par la broche 18 (GPIO 18) comme entrée et la masse (GND). Aucune résistance n’est nécessaire pour assurer le fonctionnement du bouton poussoir car l’ESP32 possède une résistance interne dit de « pull-up » sur la quasi-totalité de ses broches GPIO. A configurer avec la fonction **pinMode(Pin, INPUT\_PULLUP)** afin de ne pas laisser la broche dans un état flottant indéterminé. Le mode Pull-up câble une résistance interne entre la broche considérée et la broche d’alimentation de l’ESP32. L’appui sur le bouton-poussoir entraîne des rebonds d’où un nombre d’appuis recensé plus élevé que la réalité. Différentes techniques permettent d’y remédier quelque peu logiciellement, par exemple en ajoutant un petit retard ( delay(250) ). L’appui sur le bouton entraîne un état logique 0 sur la broche GPIO.

Pour plus de détail, voir la référence : [https://projetsdiy.fr/esp32-entrees-sorties-numeriques-gpio-code-arduino/](https://projetsdiy.fr/esp32-entrees-sorties-numeriques-gpio-code-arduino/) .

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Créer un bouton-poussoir à l’aide d’une structure appelé Button :

```
struct Button {
  const uint8_t PIN;
  uint32_t numberKeyPresses;
  bool pressed;
};
```

Une structure est une collection de variables de types différents sous un même nom.

Création d’une instance button1 de la structure Button avec des valeurs d’initialisation :

```
Button button1 = {18, 0, false};
```

Au bout d’une minute, la broche 18 n’est plus sous surveillance, grâce à un timer de 60 secondes et à la fonction **detachInterrupt()**.

Le code complet est donné ci-après dans l’exemple Interrupt\_simple.ino .

**Fichier Interrupt\_simple.ino**

**(voir l’affichage du bouton pressé dans le moniteur série, interruption au bout d’un minute)**

```arduino
struct Button {
  const uint8_t PIN;
  uint32_t numberKeyPresses;
  bool pressed;
};

Button button1 = {18, 0, false};

void IRAM_ATTR isr() {
  button1.numberKeyPresses += 1;
  button1.pressed = true;
}

void setup() {
  Serial.begin(115200);
  pinMode(button1.PIN, INPUT_PULLUP);
  attachInterrupt(button1.PIN, isr, FALLING);
}

void loop() {
  if (button1.pressed) {
     delay(250);
      Serial.printf("Button 1 has been pressed %u times\n", button1.numberKeyPresses);
      button1.pressed = false;
  }

  //Detach Interrupt after 1 Minute
  static uint32_t lastMillis = 0;
  if (millis() - lastMillis > 60000) {
    lastMillis = millis();
    detachInterrupt(button1.PIN);
	Serial.println("Interrupt Detached!");
  }
}
```

**Comprendre le code de l’exemple, puis téléversez-le dans l’ESP32.**

Pour aller plus loin, voir et comprendre l’exemple GPIOInterrupt.ino dans le menu de l’IDE Arduino :

Fichiers -> Exemples -> ESP32 -> GPIO -> GPIOInterrupt.ino
