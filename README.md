# WeihnachtsfeierLicht
## ~avatar avatar @unplugged
gedulde dich noch ein wenig bis es soweit ist :-)

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
