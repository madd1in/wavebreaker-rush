# Wavebreaker Rush

Arcade-Jetski-Rennen in der Korallenbucht. Es fängt als goldene Stunde an und
endet als Gewitter. Three.js, WebGL, eine einzige HTML-Datei ohne Build-Schritt.

**Spielen:** https://madd1in.github.io/wavebreaker-rush/

![Three.js](https://img.shields.io/badge/three.js-r128-000?logo=three.js&logoColor=white)
![Kein Build](https://img.shields.io/badge/build-keiner-4ee7ff)
![Eine Datei](https://img.shields.io/badge/dateien-1-ff9a3c)

## Worum es geht

Drei Runden gegen drei KI-Gegner. Der Ozean ist keine texturierte Ebene, sondern eine
Wellenfunktion aus sechs Sinustermen — und der Jetski reitet genau diese Wellen:
Auftrieb, Nick- und Rollwinkel kommen aus der echten Wellennormale. Eine steil
angefahrene Flanke wird zur Sprungrampe.

In der Bucht ist außerdem was los.

## Das Wetter dreht mitten im Rennen

Vier Stufen, gekoppelt an den Rennfortschritt statt an die Uhr: **Goldene Stunde →
Böen ziehen auf → Regen → Gewitter**. Jede Stufe hebt den Seegang (bis auf das
1,8-fache), verdunkelt Himmel und Wasser, zieht die Sicht zu, dreht den Seitenwind auf
und schaltet die Musik hoch. Im Gewitter zucken Blitze mit nachlaufendem Donner, der
Regen fällt schräg, Gischt weht von den Kämmen und die See wirft dich von allein in
die Luft. Der Rumpf verliert bei schwerer See spürbar Grip — im Sturm driftet es sich
leichter, aber auch unfreiwilliger.

Blitze sind gezeichnete Bahnen mit Verästelungen, nicht nur ein Aufblitzen. Manche
schlagen neben der Strecke ein: Wassersäule, Druckwelle, und wer zu nah steht, wird
weggeschoben. Dazu kann eine **Wasserhose** über die Bahn wandern, die alles in ihrer
Nähe ansaugt. Wer bei schwerer See punktet, bekommt einen **Sturmbonus** bis ×1,6.

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
- **Raketenstart** — bei GRÜN sofort Gas.
- **Grogfässer**, **Turbo-Ringe** und **Schubfelder** liegen auf und neben der Ideallinie.

## Wer sonst noch in der Bucht ist

- **Blauwal** — taucht alle paar Runden auf und legt sich quer über die Ideallinie.
  Sein Rücken ist die höchste Rampe im ganzen Rennen. Wer über den Kopf abspringt,
  fliegt weiter als über jede Holzrampe.
- **Riesenkrake** — holt mit einem Arm aus und fegt über die Bahn. Das Aufrichten
  ist die Vorwarnung: springen oder außen vorbei.
- **Geisterschiff** — eine Galeone mit brennenden Hecklaternen. Kommt man in
  Reichweite, feuert sie. Die Einschläge treffen selten, aber sie kosten Tempo.
- **Delfine** — eskortieren den Führenden und springen mit.
- **Fliegende Fische** — schrecken vor dem Bug auf, wenn du schnell genug bist.
- **Papageien** — kreisen über der Bucht und tragen nichts zum Rennen bei.

## Actionpunkte

- **Schiffswrack** — liegt quer im Wasser, die Bruchstelle in der Mitte ist die
  Durchfahrt. Spantenbogen über dem Kopf, Fetzen im Wind, links und rechts Rumpf.
- **Felsentor** — dasselbe Prinzip aus Stein, enger.
- **Holzrampen** auf Fasspontons an vier Stellen der Strecke.

Beide Durchfahrten sind reine Lückenkollision: zwei Rundkollider links und rechts,
dazwischen Platz. Kommt die KI in die Nähe eines Tors, peilt sie gezielt die Mitte an —
sonst würden sich ihre beiden Ausweichimpulse gegenseitig aufheben.

## Technik

Alles steckt in `index.html`, ~116 KB. Externe Abhängigkeiten: three.js r128 von cdnjs
und zwei Google Fonts.

- **Wasser** — Sinussumme im Vertex-Shader, Fresnel plus Sonnenglanz im Fragment-Shader,
  Feinkräusel nur für die Beleuchtung. Die Ebene folgt der Kamera, die Wellen bleiben
  weltfest — dadurch endloses Meer ohne sichtbare Kante.
- **Sprünge über ein Höhenfeld** — Rampen und Walrücken liefern eine Zusatzhöhe über der
  Wasserlinie. Bricht sie an einer Kante ab, hebt der Ski ab; wie weit, ergibt sich aus
  Fallhöhe und Auffahrtsrate. Rampe und Wal teilen sich denselben Mechanismus.
- **Post-Effekt** — vier Durchgänge: Szene ins Render-Target, Helligkeitsauszug mit
  radialem Zug aus der Sonnenposition (Lichtstrahlen), zwei getrennte Unschärfen auf
  Viertelauflösung, dann das Endbild mit Bloom, Tempostreifen, chromatischer Aberration
  bei Einschlägen, Regen auf der Linse, Vignette und Farbgradation.
- **Rundenzählung** — nicht über Trigger-Volumen, sondern über den nächstgelegenen
  Stützpunkt auf der Kurslinie plus ein Halbzeit-Flag. Abkürzen quer über die Bucht
  bringt deshalb nichts.
- **Ton** — komplett synthetisch über WebAudio, Musik und Effekte auf getrennten Bussen.
  Der Soundtrack ist keine Schleife über vier Takte mehr, sondern ein Stück: acht Takte
  Harmonie (Am–Dm–E–Am–F–G–Am–E), zwei Steel-Drum-Themen, die sich alle acht Takte
  ablösen, Gegenstimme im B-Teil, laufender Bass mit Quinten und Oktaven, Drum-Fill vor
  jedem Durchgang. Drei Intensitätsstufen, gesteuert vom Wetter — im Gewitter kommen
  Toms, Bordunfläche und eine Oktavverdopplung dazu. Große Aktionen setzen einen
  musikalischen Akzent, für den die Musik kurz wegduckt.

### Achtung beim Ändern der Wellen

Die Wellenformel steht bewusst zweimal in der Datei: als GLSL-String `WAVE_GLSL` für die
Wasseroberfläche und als JS-Funktion `waveAt()` für Auftrieb, Neigung, Sprungrampen,
Bojen und Kameraminimum. Wird eine Fassung geändert, muss die andere mit — sonst schwimmt
der Jetski sichtbar neben dem Wasser. Der Feinkräusel im Fragment-Shader ist bewusst
ausgenommen: er stört nur die Normale, nie die Höhe.

## Lokal starten

Keine Toolchain nötig, `index.html` im Browser öffnen reicht.
