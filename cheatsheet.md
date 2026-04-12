adapted and corrected from [heroheman](https://gist.github.com/heroheman)
**Es gilt die Unvollständigkeitsgarantie.**
## 1. Grundlegende Syntax und Muster
### a. Basic-Syntax

| Zeichen | Funktion         | Beispiel     | Beschreibung                                                                                    |
| :-----: | ---------------- | ------------ | ----------------------------------------------------------------------------------------------- |
|  `s()`  | Sample abspielen | `s("bd")`    | Spielt das Bassdrum-Sample                                                                      |
|  `n()`  | Noten abspielen  | `n("c4")`    | Spielt die Note C4                                                                              |
|   //    | Kommentar        | `//comment`  | Kommentar betrifft aktuelle Zeile                                                               |
|   ""    | Mini-Notation    | `"c (g,g)"`  | besondere Notation für Musik                                                                    |
|   ''    | Regulärer String | `'a string'` | ein normaler String                                                                             |
|   $:    | Parallel-Start   | `s("bd")`    | Wird verwendet, um mehrere Patterns gleichzeitig im REPL abzuspielen.<br>Stummschalten mit `_$` |
### b.  Mini-Notation
Mini-Notation kann verwendet werden, um **zeitliche Abfolgen** von musikalischen Events anzugeben, egal ob Töne, Drums, Effekt-Parameter...
Immer in doppelten Anführungszeichen verwenden!
Beispiel: `"0 2 4 <[6,8] [7,9]>"`

| Zeichen | Funktion       | Beispiel       | Beschreibung                                    |
| :-----: | -------------- | -------------- | ----------------------------------------------- |
|  `< >`  | Sequenz        | `<a b c>`      | Spielt `a`, dann `b`, dann `c`                  |
|  `[ ]`  | Sub-sequenz    | `[bd [hh hh]]` | Unterteilt Zeit in 0,5 bd, 0,25 hh, 0,25 hh     |
|   `/`   | Division       | `bd/2`         | x-mal langsamer                                 |
|   `*`   | Multiplikation | `sd*4`         | x-mal schneller                                 |
|   `!`   | Wiederholung   | `sd!3`         | Wählt 3 Ereignisse aus dem Pattern zufällig aus |
|   `~`   | Pause / Stille | `<a ~ b>`      | Spielt `a`, dann Pause, dann `b`                |
|   `,`   | Parallel       | `a,b`          | Spielt a und b gleichzeitig                     |
|  `\|`   | Zufällige Wahl | `<a\|b\|c>`    | Wählt zufällig eins von `a`, `b` oder `c`       |
|   `?`   | Zufall         | `sd?`          | zu 50 % sd spielen (`sd?0.1` für 10%)           |
|   `@`   | Verlängerung   | `bd@2`         | verdoppelt zeitliche Länge von bd               |
|  `()`   | Euklidisch     | `bd(3,8)`      | Verteilt 3 Schläge gleichmäßig über 8           |

---

## 2. Samples und Instrumente

Strudel hat eine Reihe von integrierten Samples und kann auch externe Pakete laden.
Auf diese kann mit `sound("*Soundname*")` zugegriffen werden.

| Shortcut | Sample         | Beschreibung         |
| :------: | :------------- | :------------------- |
|   `bd`   | `kick`         | Bassdrum             |
|   `sd`   | `snare`        | Snare-Drum           |
|   `cp`   | `clap`         | Händeklatschen       |
|   `hh`   | `hi-hat`       | Hi-Hat (geschlossen) |
|   `oh`   | `open hi-hat`  | Hi-Hat (offen)       |
|   `cr`   | `crash cymbal` | Crash-Becken         |
|  `ride`  | `ride cymbal`  | Ride-Becken          |
|   `lt`   | `low tom`      | Low Tom              |
|   `mt`   | `mid tom`      | Mid Tom              |
|   `ht`   | `high tom`     | High Tom             |

`bank`
**Drum-Samples:**
- `AkaiLinn`
- `RhythmAce`
- `RolandTR808`
- `RolandTR909`
- `RolandTR707`
- `ViscoSpaceDrum`

**Allgemeine Instrumente:**
* `s("synth")`: Ein klassischer analoger Synthesizer.
* `s("fm")`: Ein FM-Synthesizer.
* `s("superfm")`: Ein FM-Synthesizer mit SuperSaw-ähnlichem Oszillator.
* `s("string")`: Eine Streicher-ähnliche Klangerzeugung.
* `s("am")`: Ein AM-Synthesizer.
* `s("sine")`: Ein einfacher Sinuswellen-Oszillator.

**General MIDI (GM) Sounds:**
* `s("gm_acoustic_grand_piano")`: Akustisches Klavier
* `s("gm_acoustic_guitar_steel")`: Akustische Stahlgitarre
* `s("gm_electric_bass_finger")`: E-Bass
* `s("gm_violin")`: Violine
* ... und viele mehr, erreichbar über `gm_[instrumentname]`.

---

## 3. Effekte und Parameter-Modifikatoren

Effekte werden mit der Punkt-Syntax an ein Pattern angehängt.

| Funktion      | Parameter                    | Beispiel                    | Beschreibung                                  |
| :------------ | :--------------------------- | :-------------------------- | :-------------------------------------------- |
| `gain()`      | `0.0` - `1.0`                | `.gain(.5)`                 | Stellt die Lautstärke ein                     |
| `room()`      | `0.0` - `1.0`                | `.room(.7)`                 | Fügt Hall hinzu                               |
| `delay()`     | Zeit in Zyklen               | `.delay(.25)`               | Fügt ein Echo hinzu                           |
| `pan()`       | `0` (links) bis `1` (rechts) | `.pan("0 1")`               | Position im Stereo-Panorama                   |
| `lpf()`       | Frequenz (Hz)                | `.lpf(1000)`                | Tiefpassfilter                                |
| `hpf()`       | Frequenz (Hz)                | `.hpf(8000)`                | Hochpassfilter                                |
| `shape()`     | `0.0` - `1.0`                | `.shape(.3)`                | Verzerrungseffekt                             |
| `crush()`     | `1` - `16`                   | `.crush(8)`                 | Reduziert die Bit-Qualität (Bit-Crusher)      |
| `phaser()`    | Frequenz                     | `.phaser(4)`                | Phaser-Effekt                                 |
| `speed()`     | Wert                         | `.speed(.5)`                | Ändert die Abspielgeschwindigkeit von Samples |
| `n().scale()` | Index und Tonleiter          | `n("0 1").scale("c:minor")` | Spielt ersten und zweiten Ton aus C-Moll      |
| `chord()`     | Akkordname                   | `chord("Cmaj7")`            | Spielt einen Akkord                           |

---

## 4. Zeit, Skalen und Generative Funktionen

| Funktion   | Beispiel               | Beschreibung                                                             |
| :--------- | :--------------------- | :----------------------------------------------------------------------- |
| `setcps()` | `setcps(0.75)`         | Setzt die Geschwindigkeit in **Cycles per Second** (Zyklen pro Sekunde)  |
| `setbpm()` | `setbpm(120)`          | Setzt die Geschwindigkeit in **Beats per Minute** (Schläge pro Minute)   |
| `rand()`   | `rand.range(0,1)`      | Erzeugt eine zufällige Zahl                                              |
| `perlin()` | `perlin.range(.6, .9)` | Erzeugt sanft fließende Zufallswerte (Perlin-Noise)                      |
| `sine()`   | `sine.range(1, 10)`    | Erzeugt eine Sinuswelle                                                  |
| `scale()`  | `scale("c:minor")`     | Interpretiert `n()`-Werte als Noten einer Tonleiter                      |
| `setcps()` | `setcps(0.75)`         | Setzt die Geschwindigkeit in **Cycles per Second**.                      |
| `setcpm()` | `setcpm(60)`           | Setzt die Geschwindigkeit in **Cycles per Minute** (entspricht oft BPM). |
| `fast()`   | `.fast(2)`             | Beschleunigt das Pattern (entspricht `*` in Mini-Notation).              |
| `slow()`   | `.slow(2)`             | Verlangsamt das Pattern (entspricht `/` in Mini-Notation).               |
| `rev()`    | `.rev()`               | Spielt das Pattern rückwärts ab.                                         |
| `jux()`    | `.jux(rev)`            | Wendet eine Funktion (z.B. `rev`) nur auf dem rechten Stereo-Kanal an.   |
| `range()`  | `sine.range(1, 10)`    | Skaliert einen Wertebereich (z.B. für Oszillatoren).                     |

---
## 5. Sonstiges
### Start/ Stopp
- **Starten & Aktualisieren** mit `STRG + ENTER`
- Stoppen mit `STRG + .`
### eigene Funktionen / Variablen
```
const effectChain = register('effectChain', (pat) => pat
  .s("sawtooth")
  .cutoff(500)
  //.delay(0.5)
  .room(0.5)
)
note("a3 c#4 e4 a4").effectChain()
```
### Visualisierung
Versucht `.pianoroll()`, `` oder `.punchcard()`, um euch die Events graphisch anzeigen zu lassen.
`._pianoroll()` oder `._punchcard()`für Inline.
Ebenso `.spectrum()`, `.pitchwheel()`, `scope()`, `spiral()`
### Arrangements
Instrumenten können auch Namen zugewiesen und später so aufgerufen werden.
Mit Stack können wir mehrere gleichzeitig aufrufen.
```js
setcpm(98/4)

// Define each instrument pattern
const kick = s("[bd ~ ~ ~][~ ~ ~ bd][~ ~ bd ~][~ ~ ~ bd]").bank("RolandTR909")
const closedHat = s("[hh hh ~ ~][ hh ~ hh ~][hh ~ hh ~][hh ~ hh ~]").bank("RolandTR909")
const openHat = s("[~ ~ oh ~][~ ~ ~ ~][~ ~ ~ ~][~ ~ ~ ~]").bank("RolandTR909")
const clap =  s("[~ ~ ~ ~][cp ~ ~ ~][~ ~ ~ ~][cp ~ ~ ~]").bank("RolandTR909")

arrange(
  [1, kick],
  [1, stack(kick, clap)],
  [2, stack(kick, clap, openHat, closedHat)],
)
```
---
## 6.  Further Reading
- Strudel Tutorial: https://strudel.cc/workshop/getting-started
-  kleine Übersicht: https://strudel.patternclub.org/workshop/recap/
- Curated Link List: https://github.com/terryds/awesome-strudel
- schönes ausführliches Tutorial: https://glfmn.io/presentations/algorave/
- (!Achtung, teils abweichende Syntax!) Tidal Cycles Reference: https://tidalcycles.org/docs/reference/cycles
- Sample Collection: https://strudel-samples.alternet.site/
## 7. vollständige Beispiele
**Code:**
- https://tinyurl.com/mr42pmz3
- https://tinyurl.com/2unemh74
- https://tinyurl.com/bdfnxmz2
- 
**Videos**
- techno for crazy people: https://www.youtube.com/watch?v=HkgV_-nJOuE&t=190s
- https://www.youtube.com/shorts/bucFM2nnVc8
