# Terrarium Pro Steuerung v7.3

Diese Home Assistant Automatisierung steuert die Beleuchtung eines Terrariums basierend auf monatlichen Sonnenstands-Simulationen.

### Features:
- **Saisonaler Rhythmus:** Automatische Anpassung der Lichtzeiten je Monat.
- **Sicherheit:** Notaus-Funktion bei Überhitzung (> 50°C).
- **Hardwareschutz:** 15-Minuten-Sperre für HDI-Lampen gegen Heißzünden.
- **Monitoring:** Benachrichtigung bei Defekt der HDI-Lampe (Leistungsüberwachung).

### Benötigte Entitäten:
- `sensor.terrarium_temp` (Temperatursensor)
- `input_datetime.hdi_last_off` (Helfer für die Sperrzeit)
- `sensor.hdi_lampe_leistung` (Strommessung)
- `switch.hdi_lampe`, `switch.uv_spot_waermelampe`, `switch.grundbeleuchtung`

## Funktionsweise
Die Steuerung simuliert den natürlichen Jahresverlauf, indem sie die Ein- und Ausschaltzeiten monatlich anpasst. 

### Licht-Phasen:
1. **Morgendämmerung:** Grundbeleuchtung startet.
2. **Aufwärmphase:** UV-Spots schalten sich dazu.
3. **Mittagsspitze:** Die HDI-Lampe sorgt für maximale Helligkeit.
4. **Abendphase:** Schrittweises Abschalten zur Simulation des Sonnenuntergangs.

### Sicherheitsfeatures:
- **Heißzündschutz:** HDI-Lampen benötigen Zeit zum Abkühlen. Die Automatisierung verhindert einen Start innerhalb von 15 Minuten nach dem letzten Ausschalten.
- **Überhitzungsschutz:** Ein Temperatursensor überwacht das Terrarium und schaltet bei über 50°C sofort alle Hitzequellen ab.

- ## Installation & Einrichtung

1. **Helfer erstellen:** Gehe in Home Assistant zu *Einstellungen > Geräte & Dienste > Helfer*. Erstelle einen neuen Helfer vom Typ **Datum und/oder Uhrzeit** (nur Zeit reicht nicht, nimm beides) und nenne ihn `hdi_last_off`.
2. **Automatisierung anlegen:** Erstelle eine neue leere Automatisierung, klicke oben rechts auf die drei Punkte (...) und wähle **"Als YAML bearbeiten"**.
3. **Code kopieren:** Kopiere den Inhalt der `automation.yaml` aus diesem Repository hinein.
4. **Entitäten anpassen:** Falls deine Sensoren oder Schalter anders heißen (z. B. `switch.terrarium_licht` statt `switch.grundbeleuchtung`), nutze die "Suchen und Ersetzen"-Funktion (Strg+F), um die Namen global im Code anzupassen.

## Anpassung der Lichtzeiten
Die Automatisierung nutzt zwei Listen (Arrays) für die 12 Monate des Jahres (Januar bis Dezember). Du findest sie im Abschnitt `variables`:

- `on_times`: Die Uhrzeit, zu der die morgendliche Phase beginnt.
- `off_times`: Die Uhrzeit, zu der die abendliche Phase endet.

**Beispiel:** Wenn dein Tier im Sommer (Juli = 7. Stelle in der Liste) bereits um 07:00 Uhr Licht braucht, ändere den 7. Wert in der Liste `on_times` auf `"07:00"`.

## Fehlerbehebung (Troubleshooting)
- **HDI-Lampe geht nicht an:** Prüfe den Helfer `input_datetime.hdi_last_off`. Wenn die Lampe erst vor kurzem aus war, blockiert der Heißzündschutz den Start für 15 Minuten.
- **Benachrichtigungen:** Die Automatisierung sendet "Persistent Notifications". Diese erscheinen direkt in deiner Home Assistant Oberfläche (unten links bei der Glocke).

- ## Mitwirken & Feedback
Dieses Projekt wurde entwickelt, um eine zuverlässige und tiergerechte Steuerung zu ermöglichen. Wenn du Verbesserungsvorschläge hast oder einen Fehler findest, öffne bitte ein "Issue" oder erstelle einen "Pull Request".

**Hinweis:** Die Temperatur-Schwellenwerte und Zeit-Intervalle (z. B. 15 Min. HDI-Schutz) sind auf gängige Terrarien-Setups optimiert. Bitte prüfe vorab, ob diese Werte für deine spezifischen Tiere und Leuchtmittel geeignet sind.
