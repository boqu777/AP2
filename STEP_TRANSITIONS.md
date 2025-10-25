# STEP_TRANSITIONS — AP2 Sortieranlage

Kurztabelle mit Schrittbezeichnungen, Zweck und priorisierten Transitionen.
Referenz: FB500 (Automatik-Ablauf). Symboltabelle: `symbole.csv`.

| Schritt | Kurzname                        | Zweck (Kurz)                                 | Entry / Actions (Kurz)                      | Priorisierte Transitions (Kurz) |
|--------:|---------------------------------|-----------------------------------------------|---------------------------------------------|---------------------------------|
| 500     | Grundstellung                    | Rücksetzen / Ausgangs-Schritt                 | Resetize interne Flags / Timer              | Default / Startbedingungen      |
| 503     | Schlitten zur Soll-Position      | X-Achse fährt zur Zielposition                | Anfahren Schlitten                           | bei Position erreicht -> nächster Schritt |
| 506     | Teile sortieren / Auswahl Pfad   | Bestimmen Ablageort des aktuellen Teils       | TIMER_S507c reset                            | 1) Teil_Kunststoff -> 507<br>2) Teil_Metall & C2=Pos3 -> 509<br>3) C2=Pos2 -> 5071<br>4) else -> 500 |
| 507     | Wippe/Ablage Kunststoff          | Ablage- oder Wippen-Sequenz für Kunststoff    | Aktoren für Wippe setzen                     | bei Bestätigung / Zeit -> nächster Schritt |
| 5071    | Warten auf Entnahme (Metall)     | Warten auf Taster SJ12 (Entnahme durch Bediener)| TIMER_S507c reset; SJ12 flank detektion    | 1) SJ12_POS_FL -> 509<br>2) NOT Automatikbetrieb -> 500 |
| 509     | Rechtsfahrt / Weitertransport    | Weitertransport / nächste Position            | Motor Rechtsfahrt setzen                     | ...                             |
| 599     | Fehler / Timeout                 | Fehlerbehandlung bei Timeouts / Ausnahmen     | Alarm / Safe-State setzen                    | Manuelle Rücksetzung erforderlich|

Hinweise:
- Tabelle ist bewusst kompakt; für vollständige Beschreibung jeden Schritt mit dem
  Standard-Template in FB500 dokumentieren (siehe Kopf der FB500).
- Bei Änderungen an Symbolnamen oder DB-Feldern (`DB1.Positionen.PosX`) Tabelle aktualisieren.
- Empfohlen: Export als CSV für Simulation/Testwerkzeuge.
