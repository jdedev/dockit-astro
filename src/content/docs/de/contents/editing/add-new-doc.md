---
title: Document toevoegen
description: Leer hoe je nieuwe documentatiepagina's maakt en organiseert in je Dockit site.
---

# Nieuwe documentatiepagina's toevoegen

Deze gids leidt je door het proces van het toevoegen van nieuwe documentatiepagina's aan je Dockit site, deze correct organiseren en ervoor zorgen dat ze in de navigatie verschijnen.

## Bestandsstructuur

Alle documentatiebestanden bevinden zich in de `src/content/docs/` directory. De structuur ziet er als volgt uit:

```
src/content/docs/
├── index.mdx                 # Homepage
├── getting-started/          # Aan de slag sectie
│   ├── introduction/
│   ├── global-settings/
│   └── navigation.md
├── guides/                   # Gidsen sectie
│   └── example.md
├── reference/                # Referentie sectie
│   ├── configuration.mdx
│   └── example.mdx
├── resources/                # Bronnen sectie
│   ├── community-content.mdx
│   ├── plugins.mdx
│   ├── showcase.mdx
│   └── themes.mdx
└── contents/                 # Aanvullende inhoud
    ├── components/
    └── editing/
```

## Een nieuwe pagina maken

### Stap 1: Het bestand maken

Maak een nieuw `.md` of `.mdx` bestand in de juiste directory:

**Voor Markdown (.md):**
```bash
touch src/content/docs/guides/mijn-nieuwe-gids.md
```

**Voor MDX (.mdx) - met React componenten:**
```bash
touch src/content/docs/guides/mijn-geavanceerde-gids.mdx
```

### Stap 2: Frontmatter toevoegen

Elke documentatiepagina moet beginnen met YAML frontmatter:

```markdown
---
title: Mijn nieuwe gids
description: Een uitgebreide gids voor het gebruik van geavanceerde functies.
---

# Mijn nieuwe gids

Je content komt hier...
```

### Vereiste frontmatter velden

| Veld | Type | Beschrijving |
|-------|------|-------------|
| `title` | string | Paginatitel (verschijnt in browsertab en navigatie) |
| `description` | string | Paginabeschrijving voor SEO en previews |

### Optionele frontmatter velden

| Veld | Type | Beschrijving |
|-------|------|-------------|
| `sidebar.order` | number | Aangepaste volgorde in zijbalk |
| `sidebar.label` | string | Aangepast label in zijbalk (standaard naar titel) |
| `sidebar.hidden` | boolean | Verberg pagina van zijbalk navigatie |

## Content schrijven

### Markdown basics

Gebruik standaard Markdown syntaxis voor je content:

```markdown
# Hoofdkop
## Tweede kop
### Derde kop

**Vette tekst** en *cursieve tekst*

- Lijst item 1
- Lijst item 2
- Lijst item 3

1. Genummerde lijst item
2. Genummerde lijst item

[Link tekst](https://example.com)

`Inline code`

\`\`\`javascript
// Code blok
function hello() {
  console.log("Hallo wereld!");
}
\`\`\`
```

### MDX functies

Als je een `.mdx` bestand gebruikt, kun je React componenten importeren en gebruiken:

```mdx
import { Card } from '@astrojs/starlight/components';
import Button from '../../../components/user-components/Button.astro';

<Card title="Voorbeeld kaart">
  Dit is content binnen een kaart component.
</Card>

<Button 
  label="Klik hier" 
  link="/andere-pagina" 
  variant="primary" 
/>
```

## Organisatie en navigatie

### Directory structuur

Organiseer je bestanden logisch in mappen:

```
src/content/docs/
├── api/              # API documentatie
│   ├── authentication.md
│   ├── endpoints.md
│   └── examples.md
├── tutorials/        # Stap-voor-stap tutorials
│   ├── beginner/
│   ├── intermediate/
│   └── advanced/
└── troubleshooting/  # Probleemoplossing
    ├── common-issues.md
    └── debugging.md
```

### Zijbalk volgorde

Controle de volgorde van items in de zijbalk met `sidebar.order`:

```markdown
---
title: Geavanceerd onderwerp
description: Een geavanceerd onderwerp voor ervaren gebruikers
sidebar:
  order: 100
---
```

Lagere nummers verschijnen eerst. Bestanden zonder `order` verschijnen alfabetisch na geordende bestanden.

## Beste praktijken

### Bestandsnamen
- Gebruik kebab-case: `mijn-nieuwe-pagina.md`
- Maak namen beschrijvend: `api-authentication.md` ipv `auth.md`
- Vermijd spaties en speciale tekens

### Pagina titels
- Houd titels kort en beschrijvend
- Gebruik title case: "Aan de slag met API's"
- Maak titels uniek binnen je site

### Beschrijvingen
- Schrijf duidelijke, beknopte beschrijvingen
- Gebruik 150-160 tekens voor optimale SEO
- Beschrijf wat gebruikers zullen leren

### Content structuur
- Begin met een korte introductie
- Gebruik koppen om content te organiseren
- Voeg voorbeelden en code snippets toe
- Eindig met volgende stappen of gerelateerde links

## Geavanceerde functies

### Aangepaste styling

Voeg aangepaste CSS klassen toe aan je Markdown elementen:

```markdown
<div class="warning-box">
Dit is een waarschuwing met aangepaste styling.
</div>
```

### Content collections

Dockit gebruikt Astro's content collections. Je kunt schema validatie toevoegen door `src/content/config.ts` te bewerken:

```typescript
import { defineCollection, z } from 'astro:content';

const docsCollection = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    published: z.boolean().default(true),
    featured: z.boolean().default(false),
  }),
});

export const collections = {
  docs: docsCollection,
};
```

## Testen en preview

### Lokale development

Start de development server om je wijzigingen te bekijken:

```bash
npm run dev
```

Je nieuwe pagina wordt automatisch toegevoegd aan de navigatie en is beschikbaar op de URL die overeenkomt met je bestandspad.

### Build testen

Test je wijzigingen met een productie build:

```bash
npm run build
npm run preview
```

Dit helpt bij het opsporen van build-time fouten voordat je deployt.

---

**Volgende stappen**: Leer over [component gebruik](../components/using-components) om je documentatie interactiever te maken.