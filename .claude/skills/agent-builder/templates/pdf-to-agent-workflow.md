# PDF till Agent: Arbetsflöde

## Översikt

```
PDF-filer → Sidor → Markdown → Konsolidering → Agent + Skill
```

## Steg-för-steg

### 1. Förberedelse

```bash
# Skapa mappstruktur
mkdir -p [PROJEKT]/pdf_source
mkdir -p [PROJEKT]/pdf_pages
mkdir -p [PROJEKT]/markdown
mkdir -p [PROJEKT]/.claude/agents
mkdir -p [PROJEKT]/.claude/skills/[AGENT_NAMN]/templates
```

Placera alla PDF-filer i `pdf_source/`.

### 2. Inventering

Lista och analysera PDF-filerna:

```bash
ls -la pdf_source/*.pdf
```

Dokumentera:
| Fil | Sidor | Ämne | Språk |
|-----|-------|------|-------|
| doc1.pdf | 60 | NIS2 Directive | EN |
| doc2.pdf | 12 | Guidelines | EN |

### 3. PDF-splitting

**Med pdftk:**
```bash
for pdf in pdf_source/*.pdf; do
    name=$(basename "$pdf" .pdf)
    mkdir -p "pdf_pages/$name"
    pdftk "$pdf" burst output "pdf_pages/$name/page_%03d.pdf"
done
```

**Med Python:**
```python
import os
from PyPDF2 import PdfReader, PdfWriter

for pdf_file in os.listdir("pdf_source"):
    if pdf_file.endswith(".pdf"):
        name = pdf_file[:-4]
        os.makedirs(f"pdf_pages/{name}", exist_ok=True)

        reader = PdfReader(f"pdf_source/{pdf_file}")
        for i, page in enumerate(reader.pages):
            writer = PdfWriter()
            writer.add_page(page)
            with open(f"pdf_pages/{name}/page_{i+1:03d}.pdf", "wb") as f:
                writer.write(f)
```

### 4. Text-extraktion

**Med pdftotext:**
```bash
for dir in pdf_pages/*/; do
    for pdf in "$dir"*.pdf; do
        txt="${pdf%.pdf}.txt"
        pdftotext -layout "$pdf" "$txt"
    done
done
```

**Med Python (pdfplumber):**
```python
import pdfplumber
import os

for doc_dir in os.listdir("pdf_pages"):
    for pdf_file in sorted(os.listdir(f"pdf_pages/{doc_dir}")):
        if pdf_file.endswith(".pdf"):
            with pdfplumber.open(f"pdf_pages/{doc_dir}/{pdf_file}") as pdf:
                text = pdf.pages[0].extract_text()
                # Spara som .txt
```

### 5. Markdown-konvertering

För varje textfil, skapa optimerad markdown:

**Rensningsprinciper:**
- Ta bort: sidhuvuden, sidfötter, sidnummer, "Official Journal" etc.
- Behåll: ALL substans, definitioner, krav, siffror
- Strukturera: rubriker, listor, tabeller

**Exempelskript:**
```python
def clean_page(text, page_num):
    """Rensa och strukturera en sida."""
    lines = text.split('\n')

    # Ta bort header/footer
    lines = [l for l in lines if not is_header_footer(l)]

    # Identifiera rubriker
    # Konvertera till markdown
    # ...

    return markdown_text
```

### 6. Kvalitetskontroll

Jämför stickprov PDF vs markdown:
- [ ] Alla artiklar/sektioner med?
- [ ] Definitioner korrekta?
- [ ] Siffror och gränsvärden rätt?
- [ ] Tabeller bevarade?

### 7. Konsolidering

**Identifiera struktur:**
- Artiklar: `Article 1`, `Artikel 1`
- Kapitel: `Chapter`, `Kapitel`
- Sektioner: `Section`, `§`

**Slå ihop logiskt:**
```python
# Exempel: Konsolidera per artikel
articles = {}
for md_file in sorted(markdown_files):
    content = read_file(md_file)
    article_num = extract_article_number(content)
    if article_num:
        if article_num not in articles:
            articles[article_num] = []
        articles[article_num].append(content)

# Skriv konsoliderade filer
for article_num, contents in articles.items():
    write_file(f"art{article_num}.md", merge_contents(contents))
```

### 8. Skapa Agent

Använd `agent-template.md` och fyll i:
- Namn baserat på dokumentämne
- Verktyg baserat på användningsfall
- System prompt med expertroll
- Referens till kunskapsfiler

### 9. Skapa Skill

Använd `skill-template.md` och fyll i:
- Snabbreferens med viktigaste punkterna
- Checklistor för verifiering
- Länkar till fullständiga dokument

### 10. Skapa Templates

Identifiera vanliga outputs och skapa mallar:
- Checklistor
- Rapporter
- Procedurer

### 11. Test

Testa agenten med verkliga frågor:
```
"Granska detta projekt mot [dokumentets] krav"
"Vilka krav ställs på [specifik fråga]?"
"Generera en checklista för [område]"
```

## Tidsuppskattning

| Steg | Tid (ca) |
|------|----------|
| Förberedelse | 5 min |
| PDF-splitting | 2 min per dokument |
| Text-extraktion | 2 min per dokument |
| Markdown-konvertering | 5-10 min per 10 sidor |
| Kvalitetskontroll | 10 min |
| Konsolidering | 15-30 min |
| Agent + Skill | 20 min |
| Test | 10 min |

**Total för ~60 sidor:** ca 1-2 timmar

## Vanliga problem

| Problem | Lösning |
|---------|---------|
| Tabeller blir röriga | Använd pdfplumber med `extract_tables()` |
| Kolumntext blandat | Använd `pdftotext -layout` |
| Rubriker mitt i mening | Manuell justering eller regex-regler |
| Saknade tecken | Kontrollera PDF-encoding |
