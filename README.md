# Opgaveskabelon til "Figma til kode"

Se opgavebeskrivelsen på Fronter.

## Medfølgende Data

Der medfølger indholdsdata i form af lokale JSON-filer, som du kan bruge til din opgave. Det er ikke et krav til opgaven, men det kan gøre det nemmere og hurtigere at få tekst og billeder ind i dit projekt.

[!NOTE]
Bemærk, at CaseStudy-siden allerede inkluderer data fra en lokal JSON-fil.
Bemærk også, at ikke alle billeder fra Figma-filen er i det lokale indholdsdata.

Dokumentationen til anvendelsen af dataene finder du på: [https://frontend-design-theme.netlify.app/](https://frontend-design-theme.netlify.app/).

Her er et eksempel på, hvordan du kan bruge dataene i dine Astro-komponenter:

astro
import employees from "@data/employees.json";

console.log(employees);

## Brug af hjælpekomponenter

### DynamicImage.astro (@helpers/DynamicImage.astro)

Brug denne komponent til at vise billeder dynamisk fra lokale datafiler. Komponenten slår billedet op i src/data/images/ ud fra den sti, du sender ind via src.

DynamicImage forventer mindst:

- src: stien til billedet fra dine data
- alt: alt-tekst til billedet

Den forwarder desuden almindelige <img>`-attributter som fx class`, style, loading og sizes, samt udvalgte Astro <Image>`-options som width`, height, format, quality, priority og layout.

Eksempel med data:

astro
{employees.map((employee) => (
<DynamicImage
    src={employee.img}
    alt={employee.name}
    width={200}
    height={200}
    class="employee-image"
  />
))}

Eksempel på styling:

astro
<DynamicImage src={employee.img} alt={employee.name} class="employee-image" />

<style>
  .employee-image {
    max-width: 300px;
    border-radius: 1rem;
  }
</style>

### DynamicIcon.astro (@helpers/DynamicIcon.astro)

DynamicIcon bruges til at vise SVG-ikoner dynamisk baseret på et navn fra dine data. name skal matche filnavnet på et ikon i src/icons/.

Komponenten forwarder øvrige props direkte til SVG-komponenten, så du fx kan sende class, width, height og lignende med.

Eksempel med data:

astro
{employee.social_links.map((link) => (
<DynamicIcon name={link.icon} width={24} height={24} class="social-icon" />
))}

Hvis ikonet ikke findes, vises der ikke noget output, og komponenten logger en advarsel i konsollen.

---

## Import af SVG-ikoner direkte

Du kan importere SVG-ikoner direkte i dine komponenter ved at importere dem:

astro
import Checkmark from "@icons/checkmark.svg";

<Checkmark width={32} height={32} class="my-icon" />

Se evt. src/pages/svgs.astro for flere eksempler på direkte import og brug af SVG-ikoner.

## Kort refleksion

- Reflekter kort men fagligt over jeres løsning med henblik på udfordringerne og successerne ved opgaven.

Vi havde næsten problemer med vores makro layout gennem hele processen, der opstod hele tiden et nyt problem hvor layoutet drillede. Derudover havde vi også lidt svært ved at push og merge, da det var første gang vi arbejdede i en fork.
Vi har valgt ikke at tage “hensyn til brugerens systempræference omkring reduceret bevægelse (prefers-reduced-motion), og begræns bevægelse i animationer.” Da vi ikke havde nogle elementer eller animationer, der var relavant.

- Fremhæv specifikke kodestumper, der illustrerer brugen af forskellige teknikker og principper fra undervisningen, samt hvorfor de er brugbare.

Det første vi kan tage fat i er vores CSS properties som gjorde det nemt for os at bruge forskellige font-skrifter og størrelser og som gjorde processen nemmere for os:
--font-heading: Cabin;
--font-text: Lato;

--text-p: 1rem;
--text-title: 3.75rem;
--text-h1: 3.125rem;
--text-h2: 2.8125rem;
--text-h3: 2.5rem;

Position relativ og absolute: Dette var relevant at bruge da vi skulle lave vores login element. Her skulle pop up'en hænger sammen med knappen:

position-anchor: --nav-btn;
position-area: bottom span-left;

Fallback: relevant at bruge hvis nogle browser ikke støtter forskellige properties eller elementer. Det kan være brugbart da brugere tager sig af brug af forskellige browsers.
@supports not (padding: 1rlh) {
summary span {
padding-block: 1rem;
}

    .answer {
      padding-bottom: 1rem;
    }

}

- Fremhæv steder, hvor Defensive CSS er tænkt ind.
  img {
  width: 100%;
  aspect-ratio: 9 / 10;
  object-fit: cover;
  display: block;
  border-radius: var(--radius-img);
  }

  Ved diverse billeder har vi taget højde for "object-fit: cover" der beskærer billedet til at fylde heæe arealet uden at strække det

  summary span svg {
  flex-shrink: 0;
  }

flex-shrink forhindrer ikonerne fra at presses fladt langs teksten.

a {
margin-top: auto;
}

Det blev brugt til vores servicecard på forsiden og det skubbe links ned i bunden af cardsne.

- Reflekter over, hvor i løsningen, der er brugt progressive enhancement og gennemgå kort, hvordan det er løst.
  Vi har brugt progressive enhancement ved at sikre at al funktionalitet ligger i HTML, og CSS bruges udelukkende til visuel forbedring — ikke til at gøre tingene funktionelle. Dette kan være den mindre animation vi har lavet ved hjælp af CSS som forbedre oplevelsen for brugeren visuel men som godt kan undlades.

- Forklar, hvordan du har organiseret din CSS; hvornår er det globalt, og hvornår er det komponent-specifikt.

Set i retrospektiv blev vores css alt for langt, til at starte med drillede det en del og alt der havde noget med layout at gøre endte vi med at skrive ind i overrides da det ikke ellers gad fungere. Vi kunne have bedre til at genbruge classes i vores css og bare lave nogen der forholdte sig til layoutet, som vi så kunne genbruge tværs af elementer istedet for at lave classes til hver af vores komponenter og bruge dem enkeltvis inde i layoutet. Alt dette gjorde også at vores css ikke blev så scoped.
