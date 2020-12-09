# WeihnachtsfeierLicht
## ~avatar avatar @unplugged
![PartyStern](https://github.com/r00b1nh00d/weihnachtsfeierlicht/blob/master/PartyStern3.gif?raw=true)

## ~ @unplugged
Du kannst für dieses Projekt den bereits gebauten Weihnachtsstern oder einen anderen Streifen des RGB-LED Bands nutzen. Beginnen wir erstmal mit einer einfachen Variante, bei der ein Lautstärkebalken vom ersten bis zum letzten Pixel gezeichnet wird.

gedulde dich noch ein wenig bis es soweit ist :-)


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
Die zweite Variante, wie sie auch in der Anfangsanimation zu sehen war ist etwas aufwendiger zu programmieren. Hier wird automatisch eine Liste von Messwerten erstellt und daraus ein Maximalwert gebildet. In abhängigkeit zur Lautstärke werden dann dauerhaft vom ersten und vom letzten Pixel aus bis zur Mitte die Pixel angesteuert.


## Schritt 1 
Beginne mit einer Liste, welche ``||basic:beim Start||`` erstellt und durch eine ``||loops:Schleife||`` mit Messwerten gefüllt wird.
```blocks

let maximum = 0
let aktueller_Messwert = 0
let strip = neopixel.create(DigitalPin.P1, 49, NeoPixelMode.RGB_RGB)
let messreihe: number[] = []
for (let index = 0; index < 48; index++) {
    messreihe.push(input.soundLevel())
}
```

## Schritt 2
Zum Beginn der ``||basic:dauerhaft||``- Schleife soll ein aktueller Messwert am Ende der Liste hinzugefügt werden und der erste, also der älteste Messwert entfernt werden.
Zudem soll ein Maximalwert aus dieser Liste herausgefunden werden.

```blocks
let maximum = 0
let aktueller_Messwert = 0
let strip = neopixel.create(DigitalPin.P1, 49, NeoPixelMode.RGB_RGB)
let messreihe: number[] = []
for (let index = 0; index < 48; index++) {
    messreihe.push(input.soundLevel())
}
basic.forever(function () {
    aktueller_Messwert = Math.round(input.soundLevel())
    messreihe.push(aktueller_Messwert)
    messreihe.shift()
    maximum = 6
    for (let Wert of messreihe) {
        maximum = Math.max(maximum, Wert)
       
    }
})

``` 
## Schritt 2
Für die Animation soll erstmal der Strip ``||neopixel.strip:ausgeschaltet||`` werden. <br>
Anschließend muss in je einer ``||loops:index-Schleife||`` die Farbe an der Index Position, bzw. von der anderen Seite gesehen am maximalen Pixel minus des Indexes eingestellt werden. Du kannst hier eine einfarbige Farbe nehmen oder mit dem ``||neopixel.strip:HSL Farbwert||`` die Pixel in unterschiedlichen Farben leuchten lassen.
```blocks
let maximum = 0
let aktueller_Messwert = 0
let strip = neopixel.create(DigitalPin.P1, 49, NeoPixelMode.RGB_RGB)
let messreihe: number[] = []
for (let index = 0; index < 48; index++) {
    messreihe.push(input.soundLevel())
}
basic.forever(function () {
    aktueller_Messwert = Math.round(input.soundLevel())
    messreihe.push(aktueller_Messwert)
    messreihe.shift()
    maximum = 6
    for (let Wert of messreihe) {
        maximum = Math.max(maximum, Wert)
    }
    strip.clear()
    for (let Index = 0; Index <= Math.map(aktueller_Messwert, 0, maximum, 0, 24); Index++) {
        strip.setPixelColor(Index, neopixel.hsl(69 + Index * 60, 100, 20))
    }
    for (let Index = 0; Index <= Math.map(aktueller_Messwert, 0, maximum, 0, 24); Index++) {
        strip.setPixelColor(48 - Index, neopixel.hsl(60 + Index * 60, 100, 20))
    }
    strip.show()
    serial.writeLine("" + aktueller_Messwert)
    basic.pause(10)
})

```
