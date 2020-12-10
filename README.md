# WeihnachtsfeierLicht
## ~avatar avatar @unplugged
![PartyStern](https://github.com/r00b1nh00d/weihnachtsfeierlicht/blob/master/PartyStern3.gif?raw=true)

## ~ @unplugged
Du kannst für dieses Projekt den bereits gebauten Weihnachtsstern oder einen anderen Streifen des RGB-LED Bands nutzen. Beginnen wir erstmal mit einer einfachen Variante, bei der ein Lautstärkebalken vom ersten bis zum letzten Pixel gezeichnet wird.



## Variante 1
Dazu musst du ``||basic:beim Start||`` dem Calliope wieder beibringen, wo der Neopixel angeschlossen ist. <br>
In die ``||basic:dauerhaft||``- Schleife kannst du den Block ``||neopixel.strip:zeige Balkendiagramm||`` und eine ``||basic:Pause||`` einfügen. <br>
Das Balkendiagramm soll von der gemessenen ``||inputs: lautstärke||`` bis zu einem Maximalwert gezeichnet werden (bei mir war 90 ein guter Maximalwert, diesen kannst du von 255 langsam verringern und experimentell herausfinden).

```blocks
let strip = neopixel.create(DigitalPin.P0, 12, NeoPixelMode.RGB)
basic.forever(function () {
    strip.showBarGraph(input.soundLevel(), 90)
    strip.show()
    basic.pause(10)
})
```

##  ~ @unplugged Variante 2
Die zweite Variante, wie sie auch in der Anfangsanimation zu sehen war ist etwas aufwendiger zu programmieren. Hier wird automatisch eine Liste von Messwerten erstellt und daraus ein Maximalwert gebildet. In abhängigkeit zur Lautstärke werden dann dauerhaft vom ersten und vom letzten Pixel aus bis zur Mitte die Pixel angesteuert.<br>
Um dies zu programmieren musst du dieses Tutorial verlassen und [hier im nächsten Tutorial ](https://makecode.calliope.cc/#tutorial:https://github.com/r00b1nh00d/weihnachtsfeierlicht2) weiter arbeiten.

