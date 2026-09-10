# Wavebreaker Rush

Arcade-Jetski-Rennen im Browser. Three.js, WebGL, eine einzige HTML-Datei ohne Build-Schritt.

**Spielen:** https://madd1in.github.io/wavebreaker-rush/

![Three.js](https://img.shields.io/badge/three.js-r128-000?logo=three.js&logoColor=white)
![Kein Build](https://img.shields.io/badge/build-keiner-4ee7ff)
![Eine Datei](https://img.shields.io/badge/dateien-1-ff9a3c)

## Worum es geht

Drei Runden auf offener See gegen drei KI-Gegner. Der Ozean ist kein Bodenplane mit
Textur, sondern eine Wellenfunktion aus sechs Sinustermen — und der Jetski reitet
genau diese Wellen: Auftrieb, Nick- und Rollwinkel kommen aus der echten
Wellennormale. Eine steil angefahrene Flanke wird zur Sprungrampe.

## Steuerung

| Taste | Wirkung |
| --- | --- |
| `W` / `S` | Gas / Bremse |
| `A` / `D` | Lenken — in der Luft: Rolle |
| `Leertaste` | Turbo |
| `Shift` | Hop |
| `F` | Vollbild |
| `M` | Sound an/aus |
| `R` | Neustart |

Am Handy: Daumenpad links, Turbo/Hop/Bremse rechts, Gas läuft automatisch.

## Fahrtechnik

- **Drift-Boost** — Heck kommen lassen und halten. Die sechs Segmente unter der
  Turboleiste laden in drei Stufen (blau, orange, rot). Lenkung lösen gibt den Schub frei.
- **Windschatten** — dicht hinter einem Gegner bleiben. Weniger Widerstand, mehr Vortrieb.
- **Rolle** — eine volle Umdrehung in der Luft gibt 45 % Turbo zurück.
- **Knapp vorbei** — Bojen und Felsen streifen, ohne sie zu treffen, gibt Style-Punkte.
- **Raketenstart** — bei GRÜN sofort Gas.
- **Turbo-Ringe** füllen die Leiste, **Schubfelder** geben Sofortschub.

## Technik

Alles steckt in `index.html`, ~82 KB. Externe Abhängigkeiten: three.js r128 von cdnjs
und zwei Google Fonts.

- **Wasser** — Sinussumme im Vertex-Shader, Fresnel plus Sonnenglanz im Fragment-Shader,
  Feinkräusel nur für die Beleuchtung. Die Ebene folgt der Kamera, die Wellen bleiben
  weltfest — dadurch endloses Meer ohne sichtbare Kante.
- **Post-Effekt** — Render-Target plus Vollbild-Quad: radiale Tempostreifen,
  Chromatische Aberration bei Einschlägen, Vignette, Farbgradation.
- **Rundenzählung** — nicht über Trigger-Volumen, sondern über den nächstgelegenen
  Stützpunkt auf der Kurslinie plus ein Halbzeit-Flag. Abkürzen quer über die Bucht
  bringt deshalb nichts.
- **Ton** — komplett synthetisch über WebAudio: Motor aus zwei Oszillatoren plus
  gefiltertem Rauschen, dazu ein sequenzierter Arcade-Loop, der in der letzten Runde
  auf 148 BPM hochschaltet.

### Achtung beim Ändern der Wellen

Die Wellenformel steht bewusst zweimal in der Datei: als GLSL-String `WAVE_GLSL` für die
Wasseroberfläche und als JS-Funktion `waveAt()` für Auftrieb, Neigung, Sprungrampen,
Bojen und Kameraminimum. Wird eine Fassung geändert, muss die andere mit — sonst schwimmt
der Jetski sichtbar neben dem Wasser.

## Lokal starten

Keine Toolchain nötig, `index.html` im Browser öffnen reicht.
