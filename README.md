# Agent Builder

En meta-agent för Claude Code som skapar specialiserade agenter från PDF-dokumentation.

## Vad den gör

```
PDF-filer → Sidor → Markdown → Konsolidering → Agent + Skill
```

Agent Builder tar PDF-dokument och transformerar dem till:
1. LLM-optimerade markdown-filer
2. En specialistagent med systempromppt
3. En skill med snabbreferens
4. Relevanta templates

## Installation

Kopiera till ditt projekt:

```bash
cp -r agent-builder/.claude /ditt/projekt/
```

Eller klona:

```bash
cd /ditt/projekt
git clone https://github.com/[USER]/agent-builder.git temp
cp -r temp/.claude .
rm -rf temp
```

## Struktur

```
.claude/
├── agents/
│   └── agent-builder.md            # Agentdefinition
└── skills/
    └── agent-builder/
        ├── SKILL.md                 # Snabbreferens
        └── templates/
            ├── agent-template.md    # Mall för nya agenter
            ├── skill-template.md    # Mall för nya skills
            └── pdf-to-agent-workflow.md  # Detaljerat arbetsflöde
```

## Användning

Placera PDF-filer i en mapp och kör:

```
Skapa en agent från PDF-filerna i /path/to/pdfs/
```

Agenten kommer:
1. Inventera alla PDF-filer
2. Splitta till enskilda sidor
3. Extrahera och optimera text till markdown
4. Konsolidera efter logisk struktur (artiklar, kapitel, etc.)
5. Skapa agent-fil med system prompt
6. Skapa skill med snabbreferens
7. Generera relevanta templates

## Arbetsflöde

### Steg 1: PDF-splitting
```bash
pdftk input.pdf burst output page_%03d.pdf
```

### Steg 2: Text-extraktion
```bash
pdftotext -layout page_001.pdf page_001.txt
```

### Steg 3: Markdown-optimering

**Ta bort (fluff):**
- Sidhuvuden/sidfötter
- Sidnummer
- Upprepade disclaimers

**Behåll (substans):**
- Alla definitioner och krav
- Alla siffror och gränsvärden
- Tabeller och exempel

### Steg 4: Konsolidering

| Dokumenttyp | Konsolidera per |
|-------------|-----------------|
| EU-direktiv | Artikel |
| ISO-standard | Kontrollområde |
| Teknisk spec | Funktion/API |
| Policy | Ämnesområde |

### Steg 5: Agent-skapande

Resultat:
```
.claude/
├── agents/
│   └── [ny-agent].md
└── skills/
    └── [ny-agent]/
        ├── SKILL.md
        └── templates/
            └── [relevanta-mallar].md

markdown/
├── [DOKUMENT]_COMPLETE.md
└── [dokument]_by_section/
```

## Medföljande templates

| Mall | Syfte |
|------|-------|
| `agent-template.md` | Komplett agentstruktur med variabler |
| `skill-template.md` | Skill med snabbreferens och checklistor |
| `pdf-to-agent-workflow.md` | Steg-för-steg med kodexempel |

## Dokumenttyper som stöds

- EU-lagstiftning (direktiv, förordningar)
- ISO-standarder
- Tekniska specifikationer
- Policies och riktlinjer
- API-dokumentation
- Manualer

## Krav

- Claude Code CLI
- PDF-verktyg (valfritt):
  - `pdftk` för PDF-splitting
  - `pdftotext` (poppler) för textextraktion
  - Eller Python: `PyPDF2`, `pdfplumber`

## Exempelprojekt

Se [nis2-compliance-agent](../nis2-compliance-agent) för ett komplett exempel skapat med denna agent.

## Licens

MIT

## Bidrag

Pull requests välkomna! Särskilt:
- Stöd för fler dokumenttyper
- Förbättrad textextraktion
- Fler agentmallar
