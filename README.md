# Opgaveskabelon til "Figma til kode"

Se opgavebeskrivelsen på Fronter.

## Medfølgende Data

Der medfølger indholdsdata i form af lokale JSON-filer, som du kan bruge til din opgave. Det er ikke et krav til opgaven, men det kan gøre det nemmere og hurtigere at få tekst og billeder ind i dit projekt.

> [!NOTE]
> Bemærk, at CaseStudy-siden allerede inkluderer data fra en lokal JSON-fil.
> Bemærk også, at ikke alle billeder fra Figma-filen er i det lokale indholdsdata.

Dokumentationen til anvendelsen af dataene finder du på: [https://frontend-design-theme.netlify.app/](https://frontend-design-theme.netlify.app/).

Her er et eksempel på, hvordan du kan bruge dataene i dine Astro-komponenter:

```astro
import employees from "@data/employees.json";

console.log(employees);
```

## Brug af hjælpekomponenter

### DynamicImage.astro (`@helpers/DynamicImage.astro`)

Brug denne komponent til at vise billeder dynamisk fra lokale datafiler. Komponenten slår billedet op i `src/data/images/` ud fra den sti, du sender ind via `src`.

`DynamicImage` forventer mindst:

- `src`: stien til billedet fra dine data
- `alt`: alt-tekst til billedet

Den forwarder desuden almindelige `<img>`-attributter som fx `class`, `style`, `loading` og `sizes`, samt udvalgte Astro `<Image>`-options som `width`, `height`, `format`, `quality`, `priority` og `layout`.

Eksempel med data:

```astro
{employees.map((employee) => (
  <DynamicImage
    src={employee.img}
    alt={employee.name}
    width={200}
    height={200}
    class="employee-image"
  />
))}
```

Eksempel på styling:

```astro
<DynamicImage src={employee.img} alt={employee.name} class="employee-image" />

<style>
  .employee-image {
    max-width: 300px;
    border-radius: 1rem;
  }
</style>
```

### DynamicIcon.astro (`@helpers/DynamicIcon.astro`)

`DynamicIcon` bruges til at vise SVG-ikoner dynamisk baseret på et navn fra dine data. `name` skal matche filnavnet på et ikon i `src/icons/`.

Komponenten forwarder øvrige props direkte til SVG-komponenten, så du fx kan sende `class`, `width`, `height` og lignende med.

Eksempel med data:

```astro
{employee.social_links.map((link) => (
  <DynamicIcon name={link.icon} width={24} height={24} class="social-icon" />
))}
```

Hvis ikonet ikke findes, vises der ikke noget output, og komponenten logger en advarsel i konsollen.

---

## Import af SVG-ikoner direkte

Du kan importere SVG-ikoner direkte i dine komponenter ved at importere dem:

```astro
import Checkmark from "@icons/checkmark.svg";

<Checkmark width={32} height={32} class="my-icon" />
```

Se evt. `src/pages/svgs.astro` for flere eksempler på direkte import og brug af SVG-ikoner.
