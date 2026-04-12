adapted and corrected from [heroheman](https://gist.github.com/heroheman)
**Es gilt die Unvollständigkeitsgarantie.**
## 1. Grundlegende Syntax und Muster
### a. Basic-Syntax

|       Zeichen       | Funktion            | Beispiel                           | Beschreibung                                                                                    |
| :-----------------: | ------------------- | ---------------------------------- | ----------------------------------------------------------------------------------------------- |
|        `//`         | Kommentar           | `//comment`                        | Kommentar betrifft aktuelle Zeile                                                               |
|   `""`<br>` `` `    | Mini-Notation       | `"c [g g]"`                        | besondere Notation für Musik,<br>Backticks für mehrere Zeilen                                   |
|        `''`         | Regulärer String    | `'a string'`                       | ein normaler String                                                                             |
|        `$:`         | Parallel-Start      | `s("bd")`                          | Wird verwendet, um mehrere Patterns gleichzeitig im REPL abzuspielen.<br>Stummschalten mit `_$` |
|        `s()`        | Sample abspielen    | `s("bd")`                          | Spielt das Bassdrum-Sample                                                                      |
|      `note()`       | Noten abspielen     | `note("c4")`                       | Spielt die Note C4                                                                              |
|    `n().scale()`    | Index und Tonleiter | `n("0 1").scale("c:minor")`        | Spielt ersten und zweiten Ton aus C-Moll                                                        |
| `chord().voicing()` | Akkordname          | `chord("C7").voicing()`            | Spielt einen Akkord                                                                             |
|     `n().set()`     | Index und Akkord    | n("0 1").set(chord("C").voicing()) | Spielt Töne aus einem Akkord                                                                    |
### b.  Mini-Notation
Mini-Notation kann verwendet werden, um **zeitliche Abfolgen** von musikalischen Events anzugeben, egal ob Töne, Drums, Effekt-Parameter...
Immer in doppelten Anführungszeichen verwenden!
Beispiel: `"0 2 4 <[6,8] [7,9]>"`

| Zeichen | Funktion          | Beispiel       | Beschreibung                                                                 |
| :-----: | ----------------- | -------------- | ---------------------------------------------------------------------------- |
|  `< >`  | Sequenz           | `<a b c>`      | Spielt `a`, dann `b`, dann `c`<br>Entspricht `[a,b,c]/3`                     |
|  `[ ]`  | Sub-sequenz       | `[bd [hh hh]]` | Unterteilt Zeit rekursiv in `0,5 bd, 0,25 hh, 0,25 hh`                       |
|   `*`   | Wiederholung      | `sd*3`         | Spielt Snaredrum 3-mal in der Zeit von 1-Mal,<br>entspricht `[sd sd sd]`     |
|   `/`   | Division          | `bd/2`         | Spielt Basedrum 1-mal in der Zeit von 2-Mal,<br>entspricht `bd*0.5`          |
|   `!`   | Wiederholung      | `sd!3`         | Spielt Snaredrum 3-mal in gleicher Geschwindigkeit,<br>entspricht `sd sd sd` |
|   `~`   | Pause / Stille    | `<a ~ b>`      | Spielt `a`, dann Pause, dann `b`                                             |
|   `,`   | Parallel          | `a, b`         | Spielt a und b gleichzeitig                                                  |
|  `\|`   | Zufällige Wahl    | `<a\|b\|c>`    | Wählt zufällig eins von `a`, `b` oder `c`                                    |
|   `?`   | Boolescher Zufall | `sd?`          | zu 50 % sd spielen (`sd?0.1` für 10%)                                        |
|   `@`   | Verlängerung      | `bd@2`         | verdoppelt zeitliche Länge von bd                                            |
|  `()`   | Euklidisch        | `bd(3,8)`      | Verteilt 3 Schläge gleichmäßig über 8                                        |

---

## 2. Samples und Instrumente

Strudel hat eine Reihe von integrierten Samples und kann auch externe Pakete laden.
Auf diese kann mit `s("*Soundname*")` zugegriffen werden.
Samples können im Seitenmenü gefunden werden.
### Drum-Komponenten

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
### Drumsets
**Drum-Samples** können mit `.bank(...)` gewählt werden:
- `AkaiLinn`
- `RhythmAce`
- `RolandTR808`
- `RolandTR909`
- `RolandTR707`
- `ViscoSpaceDrum`
### andere Samples
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

Effekte werden mit der Punkt-Syntax an ein Pattern angehängt. Achtung, diese sind zum Teil global ohne Nutzung von `orbit()`. Siehe 

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
- Stoppen mit `STRG + PUNKT`
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
Versucht `.pianoroll()` oder `.punchcard()`, um euch die Events graphisch anzeigen zu lassen.
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
**Workshop-Template**
- [advanced template](https://strudel.cc/#CgoKY29uc3QgY2hvcmRzID0gY2hvcmQoIjxDXjcgQTcgRG03IEc3PiIpLmRpY3QoJ2lyZWFsJykKLy9jb25zdCBjaG9yZHMgPSBjaG9yZCgiPEVtNyBBNyMxMSBEbTkgQ203PiIpLmRpY3QoJ2lyZWFsJykKLy9jb25zdCBjaG9yZHMgPSBjaG9yZCgiPEJtNyo0IEU3KjQgRyo0IEYjbTcqND4iKS5kaWN0KCdpcmVhbCcpLm1hc2soIlswIDAgMSAxXSIpCmNvbnN0IGJhc3MxID0gIjxbMCo2XSEyIFswKjRdIFsxIDIqNF0%2BIgpjb25zdCBiYXNzMiA9ICI8MCEzIFsxIDIqNF0%2BIgoKc2FtcGxlcygnZ2l0aHViOmVkZHlmbHV4L2NyYXRlJykKCnNldGNwbSgxNDAvNCkKLy8gLS0tLS0tLS0tLS0tLSBEcnVtcyAtLS0tLS0tLS0tLS0tLS0tLS0tLS0KJDogcygiW2JkIGJkIFtiZCB8IFtiZCBiZF1dIGJkXSIpLmJhbmsoJ2NyYXRlJykKICAuZHVja29yYml0KCJbMl0gMCEzIikubGF0ZSgiWzAgLjAwXSoyIikuY29tcHJlc3NvcigiLTIwOjIwOjEwOi4wMDI6LjAyIikuZ2FpbiguNSkKJDogcygiPFt%2BIHNkIH4gc2RdW34gc2QgfiBbc2Qgc2RdXT4iKS5iYW5rKCdjcmF0ZScpLmR1Y2tvcmJpdCgyKS5yb29tKCJbWzAgfCAwLjVdIFsuMiAwXV0vMiIpLmdhaW4oMSkuY29tcHJlc3NvcigiLTIwOjIwOjEwOi4wMDI6LjAyIikucm9vbWxwKDEwMDApLmxwZigiNDAwMCA3MDAwIikKJDogcygiW1tbfiB%2BIGNwIH5dXSBbfiB%2BIH4gY3BdIH4gfl0qMC41IikuYmFuaygnUm9sYW5kVFI4MDgnKS5kdWNrb3JiaXQoMyw0KS5yb29tKCJbWzAgfCAwLjVdIFsuMiAwXV0vMiIpLmdhaW4oMC42KS5yb29tbHAoMTAwMCkubHBmKDUwMDApCiQ6IHMoIltoaCBoaCBoaCBvaF1baGggaGggb2ggaGhdIikuYmFuaygnY3JhdGUnKS5nYWluKC4zKQoKLy8gLS0tLS0tLS0tLS0tLSBTb21lIHdlaXJkIElkZWFzIC0tLS0tLS0tLS0tLS0tLS0tLS0tLQpfJDogcygiaGgqMzIiKQogIC5zZWcoIjAgMjAwIikKICAuc3BlZWQoc2F3LnJhbmdlKC0yLDIpKS5ocGYoMTAwMCkubHBmKHNsaWRlcig1ODcwLDAsMTAwMDApKS5jcnVzaCgiPDggMTY%2BIikucm9vbWxwKDUwMCkucm9vbSgxMCkucG9zdGdhaW4oIlswIC4yXSo0IikucGFuKCJbLTEwIDEwXS8yIikub3JiaXQoNCkuZHVja2RlcHRoKDEpCgovLyAtLS0tLS0tLS0tLS0tIEhhcm1vbmllcyAtLS0tLS0tLS0tLS0tLS0tLS0tLS0KJDogY2hvcmRzLm9mZnNldCgiPC0xIDAgMSAwPi80Iikudm9pY2luZygpLnMoImdtX2VwaWFubzE6MSIpLnJvb20oLjUpLmxwZigiPDMwMCAxMDAwIDIwMDAgMTUwMD4vMyIpLm9yYml0KDMpCiAgLmR1Y2tkZXB0aCgiPDMgLjI%2BIikKICAuZGlzdG9ydHR5cGUoJ2Rpb2RlJykuZGlzdG9ydCgiPDA6MSAxOi41IDI6LjM%2BIikuZ2FpbigwLjUpCgovLyAtLS0tLS0tLS0tLS0tIEJhc3MgLS0tLS0tLS0tLS0tLS0tLS0tLS0tCiQ6IG4oYmFzczEpLnNldChjaG9yZHMpLm1vZGUoInJvb3Q6ZzIiKS52b2ljaW5nKCkucygiZ21fYWNvdXN0aWNfYmFzcyIpLmdhaW4oMSkucGVudigxKQogIC5kaXN0b3J0KCI8MCAyOi4zIDI6MC4yIDA%2BIikub3JiaXQoMikKCi8vIC0tLS0tLS0tLS0tLS0gTWVsb2RpZXMgLS0tLS0tLS0tLS0tLS0tLS0tLS0tCl8kOiBjaG9yZHMudm9pY2luZygpLmFycCgiWzAgMSAyIDFdIikucygic2F3dG9vdGgiKS50cmVtc3luYyg0KS50cmVtb2xvcGhhc2UoLjI1KS5jcnVzaCgiPDUgND4iKS5scGYoc2xpZGVyKDE5MjQsMCw0MDAwKSkKXyQ6IG4oIjwwIDEgMCBbMiAwKjRdPiIpLnNldChjaG9yZHMpLm1vZGUoInJvb3Q6ZzYiKS52b2ljaW5nKCkucygiZ21fcGlhbm8iKS5nYWluKDEpLnBlbnYoLTUpLnJvb20oMSkuZ2FpbiguNSkuZGlzdG9ydCgiMjouNSIpCl8kOiBjaG9yZHMudm9pY2luZygpLmFycCgiWzEgMiA2XS8yKjMiKS5zKCJzcXVhcmUiKS50cmVtc3luYyg0KS50cmVtb2xvcGhhc2UoMCkudHJlbXNoYXBlKCd0cmknKQogIC5kaXN0b3J0KDEpLnBvc3RnYWluKC4zKQogIC8vLmxwZihzbGlkZXIoMTY5NiwwLDQwMDApKS5waGFzZXIoOCk%3D)
- [short template](https://strudel.cc/#CgoKCmNvbnN0IGNob3JkcyA9IGNob3JkKCI8Q143IEE3IERtNyBHNz4iKQovL2NvbnN0IGNob3JkcyA9IGNob3JkKCI8RW03IEE3IzExIERtOSBDbTc%2BIikKLy9jb25zdCBjaG9yZHMgPSBjaG9yZCgiPEJtNyo0IEQ3KjQgRyo0IEYjbTcqND4iKS5tYXNrKCJbMSAwIDEgMV0iKQoKc2FtcGxlcygnZ2l0aHViOmVkZHlmbHV4L2NyYXRlJykKCnNldGNwbSgxNDAvNCkKLy8gLS0tLS0tLS0tLS0tLSBEcnVtcyAtLS0tLS0tLS0tLS0tLS0tLS0tLS0KJDogcyhgPFtiZCBiZCBiZCBiZF0gICAgIFtiZCBiZCBiZCBiZF0%2BLAogICAgICA8W34gc2QgfiBbc2Qgc2RdXSAgW34gc2QgfiBzZF0%2BLAogICAgICA8ICAgIH4gICAgICAgICAgICAgW34gY3AgfiBbfiBjcF1dPmApLmJhbmsoImNyYXRlIikKCi8vIC0tLS0tLS0tLS0tLS0gSGFybW9uaWVzIC0tLS0tLS0tLS0tLS0tLS0tLS0tLQokOiBjaG9yZHMub2Zmc2V0KCI8LTEgMCAxIDA%2BLzQiKS52b2ljaW5nKCkucygiZ21fZXBpYW5vMToxIikKCi8vIC0tLS0tLS0tLS0tLS0gQmFzcyAtLS0tLS0tLS0tLS0tLS0tLS0tLS0KJDogbigiPDAhMyBbMSAyKjRdPiIpLnNldChjaG9yZHMpLm1vZGUoInJvb3Q6ZzIiKS52b2ljaW5nKCkucygiZ21fYWNvdXN0aWNfYmFzcyIpLnBlbnYoMSkKCi8vIC0tLS0tLS0tLS0tLS0gTWVsb2RpZXMgLS0tLS0tLS0tLS0tLS0tLS0tLS0tCl8kOiBjaG9yZHMudm9pY2luZygpLmFycCgiWzAgMSAyIDFdIikucygic2F3dG9vdGgiKQokOiBuKCI8MCAxIDAgWzIgMCo0XT4iKS5zZXQoY2hvcmRzKS5tb2RlKCJyb290Omc2Iikudm9pY2luZygpLnMoImdtX3BpYW5vIikKXyQ6IGNob3Jkcy52b2ljaW5nKCkuYXJwKCJbMCAxIDBdKjQiKS5zKCJzdGVpbndheSIp)

%% rooms
orbits for nerds
ducking
chords?

oft aktualisieren!!!
repl settings %%
