# Skill Template

Använd denna mall för att skapa nya skills.

```yaml
---
name: {{SKILL_NAME}}
description: {{SKILL_DESCRIPTION}}
---

# {{SKILL_TITLE}}

{{INTRO_PARAGRAPH}}

## Kunskapsfiler

Fullständig dokumentation finns i:
- `{{DOC_PATH_1}}` - {{DOC_DESC_1}} ({{SIZE}} KB)
- `{{DOC_PATH_2}}` - {{DOC_DESC_2}} ({{SIZE}} KB)

## Snabbreferens: {{MAIN_TOPIC}}

### {{SUBTOPIC_1}}
**Krav**: {{REQUIREMENT_DESCRIPTION}}
**Kontrollera**:
- [ ] {{CHECK_1}}
- [ ] {{CHECK_2}}
- [ ] {{CHECK_3}}

### {{SUBTOPIC_2}}
**Krav**: {{REQUIREMENT_DESCRIPTION}}
**Kontrollera**:
- [ ] {{CHECK_1}}
- [ ] {{CHECK_2}}

---

## {{REFERENCE_SECTION}}

### {{KEY_CONCEPT_1}}

| {{COLUMN_1}} | {{COLUMN_2}} | {{COLUMN_3}} |
|--------------|--------------|--------------|
| {{VALUE}} | {{VALUE}} | {{VALUE}} |

### {{KEY_CONCEPT_2}}

{{DEFINITION_OR_EXPLANATION}}

---

## {{CLASSIFICATION_SECTION}}

### {{CATEGORY_1}}
- {{CHARACTERISTIC_1}}
- {{CHARACTERISTIC_2}}
- {{IMPLICATION}}

### {{CATEGORY_2}}
- {{CHARACTERISTIC_1}}
- {{CHARACTERISTIC_2}}
- {{IMPLICATION}}

---

## {{SECTORS_OR_SCOPE}}

### {{GROUP_1}}
1. {{ITEM_1}}
2. {{ITEM_2}}
3. {{ITEM_3}}

### {{GROUP_2}}
1. {{ITEM_1}}
2. {{ITEM_2}}
```

## Variabler att ersätta

| Variabel | Beskrivning | Exempel |
|----------|-------------|---------|
| `{{SKILL_NAME}}` | Kort namn (kebab-case) | `nis2-compliance` |
| `{{SKILL_DESCRIPTION}}` | Vad skillen innehåller | `NIS2 directive compliance knowledge` |
| `{{SKILL_TITLE}}` | Rubrik | `NIS2 Compliance Skill` |
| `{{DOC_PATH_X}}` | Sökväg till dokument | `markdown/NIS2_COMPLETE.md` |
| `{{MAIN_TOPIC}}` | Huvudämne | `Artikel 21 - De 10 kraven` |

## Designprinciper

1. **Progressive disclosure** - Viktigast först, detaljer i separata filer
2. **Checklistor** - Gör det enkelt att verifiera krav
3. **Tabeller** - Strukturera komplex information
4. **Max 2-3 skärmhöjder** - Snabbreferens ska vara snabb
5. **Länkar till fullständiga dokument** - För djupdykning
