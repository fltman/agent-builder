---
name: agent-builder
description: Kunskap om hur man skapar Claude Code agenter och skills från dokumentation
---

# Agent Builder Skill

## Snabbreferens: Agent-fil format

```yaml
---
name: agent-namn           # Kort, beskrivande (kebab-case)
description: Beskrivning   # När agenten ska användas (för PROAKTIV triggning)
tools: Read, Write, ...    # Kommaseparerad lista
model: inherit             # eller: sonnet, opus, haiku
skills: skill-namn         # Valfritt: länka till skill
---

# System Prompt

[Markdown-formaterad instruktion till agenten]
```

## Tillgängliga verktyg

| Verktyg | Användning |
|---------|------------|
| `Read` | Läsa filer |
| `Write` | Skapa nya filer |
| `Edit` | Redigera existerande filer |
| `Bash` | Köra terminalkommandon |
| `Glob` | Hitta filer med mönster |
| `Grep` | Söka i filinnehåll |
| `Task` | Starta sub-agenter |
| `WebFetch` | Hämta webbsidor |
| `WebSearch` | Söka på webben |

## Snabbreferens: Skill-fil format

```yaml
---
name: skill-namn
description: Kort beskrivning av skill-innehållet
---

# Skill Rubrik

[Snabbreferens med det viktigaste - max 2-3 skärmhöjder]

## Detaljerade filer

Hänvisa till fullständiga dokument:
- `path/to/complete-doc.md` - Beskrivning
```

## Filplacering

```
.claude/
├── agents/
│   └── [namn].md              # Projektspecifik agent
└── skills/
    └── [namn]/
        ├── SKILL.md           # Huvudfil (krävs)
        └── templates/         # Valfritt
            └── [mall].md

~/.claude/
├── agents/                    # Globala agenter (alla projekt)
└── skills/                    # Globala skills
```

## PDF-processering

### Verktyg för PDF-splitting

**pdftk (rekommenderat):**
```bash
pdftk input.pdf burst output page_%03d.pdf
```

**Python (PyPDF2):**
```python
from PyPDF2 import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
for i, page in enumerate(reader.pages):
    writer = PdfWriter()
    writer.add_page(page)
    with open(f"page_{i+1:03d}.pdf", "wb") as f:
        writer.write(f)
```

### Text-extraktion

**pdftotext (poppler):**
```bash
pdftotext -layout page_001.pdf page_001.txt
```

**Python (pdfplumber - bättre för tabeller):**
```python
import pdfplumber

with pdfplumber.open("page.pdf") as pdf:
    text = pdf.pages[0].extract_text()
    tables = pdf.pages[0].extract_tables()
```

## Markdown-optimering

### Ta bort (fluff)
- Sidhuvuden/sidfötter
- Sidnummer
- Upprepade disclaimers
- Tomma rader i överflöd
- "This page intentionally left blank"

### Behåll (substans)
- Alla definitioner
- Alla krav/punkter
- Alla siffror/gränsvärden
- Alla referenser
- Tabelldata
- Exempel

### Strukturera
- `#` för huvudrubriker (dokument/kapitel)
- `##` för sektioner (artiklar/avsnitt)
- `###` för undersektioner
- Tabeller för strukturerad data
- Listor för krav och punkter
- `**fetstil**` för nyckeltermer

## Konsolideringsstrategier

| Dokumenttyp | Identifiera via | Konsolidera per |
|-------------|-----------------|-----------------|
| EU-lagstiftning | "Article", "Artikel" | Artikel |
| ISO-standard | "Control", "Annex" | Kontrollområde |
| RFC | "Section" | Sektion |
| API-dok | "Endpoint", "Method" | Resurs/funktion |
| Policy | Kapitelrubriker | Ämnesområde |

## Checklista: Ny agent

- [ ] PDF-filer inventerade
- [ ] Sidor extraherade
- [ ] Markdown skapad för varje sida
- [ ] Kvalitetskontroll: PDF vs markdown
- [ ] Konsoliderade filer skapade
- [ ] Agent-fil skapad i `.claude/agents/`
- [ ] SKILL.md skapad med snabbreferens
- [ ] Templates skapade (om relevant)
- [ ] Agent testad på exempelfråga

## Vanliga agenttyper

### Compliance-agent
```yaml
tools: Read, Grep, Glob, Write, Edit
```
Templates: Checklist, Gap analysis, Audit report

### Teknisk expert-agent
```yaml
tools: Read, Grep, Glob, Bash, Write
```
Templates: Implementation guide, Troubleshooting

### Dokumentations-agent
```yaml
tools: Read, Write, Edit, Glob
```
Templates: Doc templates, Style guide

### Review-agent
```yaml
tools: Read, Grep, Glob
```
Templates: Review checklist, Finding report
