# PackMixer

Fabric-Mod für **Minecraft Java 26.2**. Du siehst die Texturen aller Resource Packs, die du installiert hast,
wählst pro Textur, aus welchem Pack sie kommen soll, und exportierst daraus ein **neues Resource Pack**.
Alles im Spiel, ohne ZIP-Dateien zu entpacken.

Die Mod bringt selbst **keine Texturen** mit. Sie arbeitet nur mit den Packs in deinem `resourcepacks`-Ordner.

## Benutzen

1. Lege die Packs, die du mischen willst, in den Ordner `resourcepacks` (ZIP-Dateien oder entpackte Ordner).
2. Öffne PackMixer mit der Taste **J** (änderbar unter Steuerung) oder über den Knopf **PackMixer**
   oben links im Resource-Pack-Menü.
3. Links stehen alle Texturen. Mit dem Suchfeld und dem Knopf **Kategorie** (block, item, entity, gui ...) filterst du.
4. Klicke eine Textur an. Rechts siehst du sie aus jedem Pack, das sie enthält. Klicke das Pack an, das du willst.
5. Schneller: unten **Pack:** anklicken (wechselt durch die Packs) und dann **Auf Liste anwenden**.
   Das setzt alle Texturen in der aktuellen Liste auf dieses Pack, soweit es sie hat.
   **Liste leeren** nimmt die Auswahl für die Liste wieder zurück.
6. **Basis:** ein Pack, das komplett als Grundlage dient (auch Modelle, Sounds, Sprachdateien ...).
   Deine ausgewählten Texturen überschreiben dann dessen Texturen. Ohne Basis enthält das neue Pack nur deine Auswahl.
7. Namen eintippen, **Exportieren** klicken. Das neue Pack liegt danach in `resourcepacks` und wird im normalen
   Resource-Pack-Menü aktiviert. Ein bestehendes Pack wird nie überschrieben, es wird durchnummeriert (`Name_2.zip`).

## Bauen (die Mod ist als Quellcode, noch nicht als fertige .jar)

Du brauchst **Java 25 (JDK)**, zum Beispiel von https://adoptium.net . Das Java im Minecraft-Launcher reicht zum Bauen nicht.

```
Windows:      gradlew build
Mac / Linux:  ./gradlew build
```

(Mac/Linux: falls "Permission denied", einmal `chmod +x gradlew`.)

Beim ersten Mal lädt Gradle Minecraft und Fabric herunter, das dauert einige Minuten.
Die fertige Datei liegt danach in `build/libs/` als `packmixer-1.0.0.jar`.
Nimm **nicht** die Datei mit `-sources` im Namen.

### Alternative ohne Java auf dem Computer: GitHub baut die Mod

1. Neues Repository auf github.com anlegen.
2. Den **Inhalt** dieses Ordners hochladen (Add file, Upload files; auch der Ordner `.github` muss mit).
3. Oben auf **Actions** gehen. Der Lauf **build** startet von selbst.
4. Wenn er grün ist: den Lauf öffnen und unten bei **Artifacts** die fertige .jar herunterladen.

## Installieren

1. Fabric Loader für 26.2 installieren (https://fabricmc.net).
2. Die passende **Fabric API** für 26.2 in den Ordner `mods` legen.
3. `packmixer-1.0.0.jar` ebenfalls in `mods` legen.

## Wenn das Bauen scheitert

Die Versionsnummern stehen in `gradle.properties` (`fabric_api_version`, `loader_version`, `loom_version`).
Auf https://fabricmc.net/develop findest du die aktuellen Werte für 26.2. Trage sie dort ein und baue neu.
Bei einem Java-Fehler (`cannot find symbol`, `does not override`) schick mir die komplette Fehlermeldung.

## Grenzen

- Die Vorschau zeigt höchstens 16 x 16 Pixel (große Texturen werden verkleinert, bei Animationen das erste Bild).
  Das exportierte Pack enthält die Originaldateien in voller Auflösung.
- Gemischt werden einzelne Texturen (`assets/*/textures/**.png`) samt ihrer `.png.mcmeta`-Animationsdatei.
  Eigene Modelle, Item-Definitionen, Sounds und Texte kommen nur über das **Basis**-Pack mit.
  Eine Textur mit anderem Seitenverhältnis als das Original kann bei fremden 3D-Modellen falsch aussehen.
- Nur Packs im Ordner `resourcepacks` werden gefunden, keine Server-Packs.
- Das Resource-Pack-Format wird aus dem laufenden Spiel gelesen. Klappt das nicht, markiert Minecraft das Pack als
  "inkompatibel", du kannst es aber trotzdem aktivieren.

## Rechtliches

Texturen gehören den Erstellern der Original-Packs. Das Export-Pack enthält eine `CREDITS.txt`, die die Quellen nennt.
Für dich privat ist das unproblematisch. Gib gemischte Packs nur weiter, wenn die Lizenzen der Original-Packs das erlauben.
Der Quellcode von PackMixer steht unter der MIT-Lizenz (siehe `LICENSE`, trag dort deinen Namen ein; ebenso bei `authors` in
`src/main/resources/fabric.mod.json`).

## Aufbau

```
src/client/java/com/packmixer/
  PackMixerClient.java   Einstieg: Taste J und Knopf im Resource-Pack-Menü
  ui/MixerScreen.java    das Menü
  core/                  Logik ohne Minecraft-Code: Packs einlesen, Vorschau, Export
src/main/resources/      fabric.mod.json, Sprachdateien, Icon
```
