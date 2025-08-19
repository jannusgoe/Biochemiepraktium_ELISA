# ELISA Datenanalyse

Dieses Repository enthält eine reproduzierbare Analyse von ELISA-Daten (Enzyme-Linked Immunosorbent Assay) zur Bestimmung von Antikörper-Bindungsaffinitäten.

## Projektübersicht

Das Projekt analysiert ELISA-Absorptionsmessungen zu verschiedenen Zeitpunkten (5, 12 und 17 Minuten) für zwei Antigene (BSA und HSA) bei verschiedenen Antikörperkonzentrationen. Die Analyse umfasst:

1. **Datenaufbereitung und -visualisierung**: Transformation der Rohdaten in ein analysierbares Format
2. **Sättigungskurven-Fitting**: Nichtlineare Regression zur Bestimmung der apparenten Dissoziationskonstante (KD)
3. **Reproduzierbare Auswertung**: Vollständig dokumentierte Analyse mit R

## Datenschema

### Experimentelles Design
- **Messzeiten**: 5, 12, 17 Minuten
- **Antigene**: BSA (Bovine Serum Albumin), HSA (Human Serum Albumin)  
- **Antikörperverdünnungen**: 1:500 bis 1:64000 (Wells A-H)
- **Stammlösung**: 8 mg/ml Anti-BSA-Antikörper (150 kDa)

### Dateien
```
data/
├── elisa_5min.csv   # Absorptionsmessungen nach 5 Minuten
├── elisa_12min.csv  # Absorptionsmessungen nach 12 Minuten  
└── elisa_17min.csv  # Absorptionsmessungen nach 17 Minuten
```

### Plattenaufteilung
- **Spalten 1-3, 7-9**: BSA-beschichtete Wells (Messungen)
- **Spalten 4-5, 10-11**: HSA-beschichtete Wells (Messungen)
- **Spalten 6, 12**: Kontroll-Wells (unspezifische Bindung)
- **Zeilen A-H**: Verschiedene Antikörperverdünnungen

## Reproduzierbarkeit

### Systemvoraussetzungen
- **R Version**: 4.5.0 oder höher
- **Paketverwaltung**: renv (für reproduzierbare Paketversionen)

### Abhängigkeiten installieren
```r
# renv installieren falls nicht vorhanden
install.packages("renv")

# Projektabhängigkeiten aus renv.lock wiederherstellen
renv::restore()
```

### Kernpakete
- `tidyverse` (Datenmanipulation und Visualisierung)
- `here` (Plattformunabhängige Pfade)
- `ggrepel` (Verbesserte Plot-Labels)

### Analyse ausführen
```r
# Jupyter Notebook öffnen
jupyter notebook auswertung.ipynb

# Oder in R direkt ausführen
rmarkdown::render("auswertung.ipynb")
```

## Ergebnisse

Die Analyse generiert folgende Outputs:

### Abbildungen (`figures/`)
- `plot_rohdaten.png`: Alle Rohdaten nach Zeitpunkten und Replikaten
- `plot_mean_rohdaten.png`: Mittelwerte mit Standardabweichungen  
- `plot_saettigung_kd.png`: Sättigungskurve mit KD-Bestimmung

### Wichtige Ergebnisse
- **Apparente KD**: ~1.46 × 10⁻⁸ M (14.6 nM)
- **Maximale Absorption (A_max)**: ~0.178
- **Optimaler Messzeitpunkt**: 17 Minuten (höchste Signalstärke)

## Methodik

### Datenaufbereitung
1. **Normalisierung**: Subtraktion der unspezifischen Bindung (Kontroll-Wells)
2. **Konzentrationberechnung**: Umrechnung der Verdünnungsfaktoren in molare Konzentrationen
3. **Qualitätskontrolle**: Replikatsvergleich und Ausreißererkennung

### Kinetische Analyse
- **Sättigungsmodell**: A = (A_max × [AK]) / (KD + [AK])
- **Fitting-Methode**: Nichtlineare kleinste Quadrate (NLS)
- **Parameter**: A_max (maximale Bindung), KD (Dissoziationskonstante)

## Dateienstruktur

```
elisa/
├── README.md                 # Diese Datei
├── auswertung.ipynb         # Haupt-Analyseskript (Jupyter Notebook)
├── renv.lock                # Exakte Paketversionen für Reproduzierbarkeit
├── renv/                    # renv Cache-Verzeichnis
├── data/                    # Rohdaten (CSV-Dateien)
├── figures/                 # Generierte Abbildungen
│   ├── plot_rohdaten.png
│   ├── plot_mean_rohdaten.png
│   └── plot_saettigung_kd.png
```

## Qualitätssicherung

### Experimentelle Kontrollen
- **Negativ-Kontrollen**: HSA-Wells zeigen erwartungsgemäß geringe Bindung
- **Zeitverlauf**: Zunehmende Signalstärke über 5-17 Minuten
- **Replikate**: Dreifachbestimmungen für alle Verdünnungen

### Statistische Validierung  
- **Residuen-Analyse**: Prüfung der Modellanpassung
- **Konfidenzintervalle**: Standardfehler für alle Fit-Parameter
- **R²-Werte**: Güte der nichtlinearen Regression

## Lizenz

Dieses Projekt dient wissenschaftlichen und educativen Zwecken. Die Daten und der Code stehen unter MIT-Lizenz zur Verfügung.