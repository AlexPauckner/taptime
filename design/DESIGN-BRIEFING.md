# TapTime — Design Briefing

**Stand:** 2026-02-16
**Status:** Entwurf genehmigt, bereit zur Implementation
**Favorit:** Rainbow Neon Runes

---

## Genehmigter Entwurf

**Datei:** `mockup-rainbow-neon-runes.html` (im Browser öffnen)

### Stil: Rainbow Neon Runes

- Dunkler Hintergrund (#0c0c18)
- SVG Line-Art-Icons mit Neon-Glow-Effekt (kein Emoji)
- Intensive, satte Kachelfarben (nicht subtil/transparent)
- Rainbow-Farbverläufe innerhalb der Gruppen
- Glow-Border (1.5px) in der jeweiligen Kachelfarbe
- Radial-Gradient-Overlay auf jeder Kachel für Tiefe
- Running-State: Weisser Border + intensivierter Glow + Puls-Animation
- Gruppen-Separatoren: BERUF / HAUSHALT / PERSÖNLICH (dezent, 10px, uppercase, 30% Opacity)

### Farbschema (Rainbow pro Gruppe)

| Zeile | Gruppe | Links | Mitte | Rechts | Verlauf |
|-------|--------|-------|-------|--------|---------|
| 1 | BERUF | #818cf8 (Indigo) | #a78bfa (Violet) | #6366f1 (Deep Indigo) | Kühles Spektrum |
| 2 | BERUF | #fbbf24 (Amber) | #3b82f6 (Blue) | #c084fc (Purple) | Warm → Cool |
| 3 | HAUSHALT | #f9a8d4 (Pink) | #4ade80 (Green) | #2dd4bf (Teal) | Bunt-Kontrast |
| 4 | HAUSHALT | #fb923c (Orange) | #f87171 (Red) | #34d399 (Emerald) | Warm → Grün |
| 5 | PERSÖNLICH | #fcd34d (Gold) | #2dd4bf (Teal) | #4ade80 (Green) | Gold → Grün |

### Grid-Anordnung (3×5, MIT Gruppen-Separatoren)

| Gruppe | Zeile | Spalte 1 | Spalte 2 | Spalte 3 |
|--------|-------|----------|----------|----------|
| BERUF | 1 | EIINH 🎬 | Bilderbuch 📖 | KI-Ökosystem ⚙️ |
| BERUF | 2 | Kita 🏫 | KI-Fortbildung 🎓 | Brainstorm 💡 |
| HAUSHALT | 3 | Kinder 👶 | Hausarbeit 🧹 | Küche 🍽️ |
| HAUSHALT | 4 | Kochen 🍳 | Einkaufen 🛒 | Buchhaltung 📋 |
| PERSÖNLICH | 5 | Jasmin 💛 | Freizeit 🎮 | Sport 🏃 |

### Alle 15 Kategorien (Mapping zum Code)

| ID | Name | Gruppe | Glow-Farbe |
|----|------|--------|-----------|
| eiinh | EIINH | Beruf | #818cf8 |
| bilderbuch | Bilderbuch | Beruf | #a78bfa |
| ki-oekosystem | KI-Ökosystem | Beruf | #6366f1 |
| kita | Kita | Beruf | #fbbf24 |
| ki-fortbildung | KI-Fortbildung | Beruf | #3b82f6 |
| brainstorm | Brainstorm | Beruf | #c084fc |
| kinder | Kinder | Haushalt | #f9a8d4 |
| hausarbeit | Hausarbeit | Haushalt | #4ade80 |
| kueche | Küche | Haushalt | #2dd4bf |
| kochen | Kochen | Haushalt | #fb923c |
| einkaufen | Einkaufen | Haushalt | #f87171 |
| buchhaltung | Buchhaltung | Haushalt | #34d399 |
| jasmin | Jasmin | Persönlich | #fcd34d |
| freizeit | Freizeit | Persönlich | #2dd4bf |
| sport | Sport | Persönlich | #4ade80 |

---

## CSS-Schlüsselkonzepte für Implementation

### Tile-Styling via CSS Custom Property

```css
.tile {
  --glow: #f87171; /* pro Kachel gesetzt */
  border: 1.5px solid var(--glow);
  background:
    radial-gradient(ellipse at 50% 30%, color-mix(in srgb, var(--glow) 35%, transparent) 0%, transparent 70%),
    linear-gradient(160deg, color-mix(in srgb, var(--glow) 22%, #0c0c18) 0%, #0c0c18 100%);
  box-shadow: 0 0 20px color-mix(in srgb, var(--glow) 15%, transparent);
}
```

### Icon-Glow

```css
.tile .icon {
  color: var(--glow);
  filter: drop-shadow(0 0 6px var(--glow)) drop-shadow(0 0 15px color-mix(in srgb, var(--glow) 50%, transparent));
}
```

### Running-State

```css
.tile.running {
  border-color: #fff;
  box-shadow: 0 0 30px color-mix(in srgb, var(--glow) 40%, transparent);
  animation: pulse 2.5s ease-in-out infinite;
}
```

### Gruppen-Separator

```css
.group-sep {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: rgba(255,255,255,0.3);
  padding: 10px 0 5px 4px;
}
```

---

## Dateien in diesem Ordner

| Datei | Beschreibung |
|-------|-------------|
| `mockup-rainbow-neon-runes.html` | **GENEHMIGTER ENTWURF** — im Browser öffnen (15 Kacheln, 3×5 mit Gruppen) |
| `mockup-neon-runes-v1.html` | Erster Entwurf (verworfen — Farben zu bunt pro Zeile, nur 12 Kacheln) |
| `referenz-krea-neon-runes.png` | Krea-generiertes Referenzbild (Icon-Stil-Inspiration) |
| `TapTime-Design-Mockup.pptx` | Keynote/PPTX-Version zum manuellen Anpassen |
| `build-design-pptx.py` | Python-Script für PPTX-Generierung |

---

## Offene Fragen für Implementation

1. **SVG-Icons:** Mockup hat handgebaute SVGs — sollen die 1:1 übernommen oder durch eine Icon-Library (z.B. Lucide, Phosphor) ersetzt werden?
2. **CAT_COLORS Update:** Die neuen Rainbow-Farben weichen vom aktuellen Code ab — soll das 1:1 übernommen werden oder nochmal angepasst?
3. **Sport:** Im aktuellen Code als Custom-Slot — soll Sport als feste Kategorie in FIXED_CATEGORIES rein?
4. **Jasmin:** Im Code noch "Zeit mit Jasmin" — soll auf "Jasmin" gekürzt werden?
5. **Emoji-Fallback:** Soll es einen Fallback auf Emojis geben wenn SVG-Icons nicht laden?
