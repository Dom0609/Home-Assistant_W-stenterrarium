# 🦎 Terrarium Pro Steuerung v7.3

Diese Home Assistant Automatisierung steuert die Beleuchtung eines Terrariums basierend auf monatlichen Sonnenstands-Simulationen. Sie sorgt für einen natürlichen Rhythmus und bietet gleichzeitig wichtige Sicherheitsfunktionen für deine Tiere und die Hardware.

## ✨ Features

* **Saisonaler Rhythmus:** Automatische Anpassung der Lichtzeiten je Monat (Januar bis Dezember).
* **Sicherheit:** Notaus-Funktion bei Überhitzung (> 50°C).
* **Hardwareschutz:** Intelligente 15-Minuten-Sperre für HDI-Lampen gegen gefährliches Heißzünden.
* **Monitoring:** Automatische Benachrichtigung bei Defekt der HDI-Lampe via Leistungsüberwachung.

---

## 🛠 Benötigte Entitäten

Damit die Automatisierung funktioniert, müssen folgende Entitäten in deinem Home Assistant vorhanden sein:

| Entität | Typ | Beschreibung |
| :--- | :--- | :--- |
| `sensor.terrarium_temp` | Sensor | Überwacht die Gehäusetemperatur. |
| `input_datetime.hdi_last_off` | Helfer | **WICHTIG:** Typ "Datum und Zeit" (für die Sperrzeit). |
| `sensor.hdi_lampe_leistung` | Sensor | Misst die aktuelle Wirkleistung in Watt. |
| `switch.hdi_lampe` | Schalter | Die Haupt-Lichtquelle (z.B. Bright Sun / Solar Raptor). |
| `switch.uv_spot_waermelampe` | Schalter | Zusätzliche UV- oder Wärmequelle. |
| `switch.grundbeleuchtung` | Schalter | Basis-Beleuchtung (LED oder T5 Balken). |

---

## ⏳ Funktionsweise (Die Licht-Kaskade)

Die Steuerung simuliert den natürlichen Tagesverlauf durch gestaffeltes Ein- und Ausschalten:

1.  **Morgendämmerung:** Grundbeleuchtung startet (15 Min. vor der berechneten Zeit).
2.  **Aufwärmphase:** UV-Spots schalten sich ein (punktgenau zum berechneten Start).
3.  **Mittagsspitze:** Die HDI-Lampe zündet (15 Min. nach Start), sobald die Sperrzeit abgelaufen ist.
4.  **Abendphase:** Schrittweises Abschalten zur Simulation des Sonnenuntergangs.

---

## 🚀 Installation & Einrichtung

1.  **Helfer erstellen:** Gehe in Home Assistant zu `Einstellungen > Geräte & Dienste > Helfer`. Erstelle einen neuen Helfer vom Typ **Datum und Uhrzeit** und nenne ihn `hdi_last_off`.
2.  **Automatisierung anlegen:** Erstelle eine neue leere Automatisierung, klicke oben rechts auf die drei Punkte (`⋮`) und wähle **"Als YAML bearbeiten"**.
3.  **Code kopieren:** Kopiere den Inhalt der `automation.yaml` aus diesem Repository hinein.
4.  **Entitäten anpassen:** Falls deine Sensoren anders heißen, nutze die "Suchen und Ersetzen"-Funktion (`Strg + F`), um die Namen global im Code an dein System anzupassen.

### Anpassung der Lichtzeiten
Die Automatisierung nutzt zwei Listen (Arrays) im Abschnitt `variables:`. Diese repräsentieren die 12 Monate (Jan bis Dez):
* `on_times`: Die Uhrzeit, zu der die morgendliche Phase beginnt.
* `off_times`: Die Uhrzeit, zu der die abendliche Phase endet.

---

## ⚠️ Sicherheit & Fehlerbehebung

* **Heißzündschutz:** HDI-Lampen dürfen nach dem Ausschalten nicht sofort wieder gezündet werden. Die Automatisierung verhindert einen Start innerhalb von 15 Minuten automatisch.
* **Überhitzungsschutz:** Bei Überschreiten von 50°C (einstellbar im Trigger `overheat`) werden alle Hitzequellen sofort abgeschaltet und ein Alarm ausgelöst.
* **Defekt-Check:** Wenn die HDI-Steckdose auf "AN" steht, aber weniger als 5W verbraucht wird, sendet das System eine Wartungsmeldung (Leuchtmittel defekt).

---

## 🤝 Mitwirken & Feedback

Dieses Projekt wurde entwickelt, um eine zuverlässige und tiergerechte Steuerung zu ermöglichen. 

* **Feedback:** Wenn du Verbesserungsvorschläge hast oder einen Fehler findest, öffne bitte ein **Issue**.
* **Beitrag:** Optimierungen am Code sind via **Pull Request** herzlich willkommen.

*Hinweis: Bitte prüfe vorab, ob die Temperatur-Schwellenwerte und Intervalle für deine spezifische Tierart und Hardware geeignet sind.*
