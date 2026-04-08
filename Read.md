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
