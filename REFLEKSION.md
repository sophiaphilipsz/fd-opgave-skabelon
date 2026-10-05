# Refleksion – Figma til kode

**Gruppemedlemmer:** Sophia Philipsz

---

## Eksempel 1: FAQ med animation

### Hvor og hvorfor?

Hvor i løsningen bruger I teknikken, og hvilket konkret problem løser den? Henvis gerne til en fil, fx `src/components/MinKomponent.astro`.

Jeg bruger details og summary til FAQ så spørgsmålene kan åbnes og lukkes. Med @supports tjekker jeg om browseren understøtter ''interpolate-size'' Hvis den gør, så får åbning og lukningen en animation. Hvis ikke browseren understøtter ''interpolate-size'' så fungerer FAQ boksen stadig, men bare uden en animation.

Findes i: `src/pages/about.astro`

### Relevant kode

```astro
<details class="faq-item">
  <summary>
    <span>{item.question}</span>
    <span class="faq-icon" aria-hidden="true"></span>
  </summary>

  <div class="faq-answer">
    <div class="faq-answer-inner">
      {item.answer.split("\n\n").map((paragraph) => (
        <p>{paragraph}</p>
      ))}
    </div>
  </div>
</details>
```

```css
@supports (interpolate-size: allow-keywords) {
  :global(html) {
    interpolate-size: allow-keywords;
  }

  .faq-answer {
    height: 0;
    overflow: hidden;
    padding-top: 0;
    padding-bottom: 0;

    transition:
      height 250ms ease,
      padding 250ms ease;
  }

  .faq-item[open] .faq-answer {
    height: auto;
    padding: 0 35px 15px 0;
  }
}
```

### Afprøvning og ændringer

- **Vi testede:** Jeg testede FAQ'en ved at åbne og lukke spørgsmålene for at se, om de åbnede og om animationen virkede.
- **Vi observerede:** Selve spørgsmålene i FAQ'en åbnede og lukkede, men animationen virker ikke på det første spørgsmål. De andre spørgsmål åbner dog og lukker med animationen.
- **Vi ændrede eller mangler:** Selve spørgsmålet åbnes stadig, bare uden animationen. Men jeg mangler at finde ud af, hvorfor animationen ikke virker på det første spørgsmål.

## Eksempel 2: Container queries

### Hvor og hvorfor?

Jeg har brugt container queries på kortene i “Our core values”sektionen. Det gør, at kortenes layout tilpasser sig efter, hvor meget plads der er i containeren, i stedet forat tage udgangspunkt i hele skærmens bredde. Når der bliver mindre plads, får kortene færre koloner.

### Relevant kode

```css
.value-cards-wrapper {
  container-type: inline-size;
  container-name: values;
}

@container values (max-width: 720px) {
  .value-cards {
    grid-template-columns: repeat(2, 1fr);
  }
}

@container values (max-width: 460px) {
  .value-cards {
    grid-template-columns: 1fr;
  }
}
```

### Afprøvning og ændringer

- **Vi testede:** Jeg gjorde browseren smallere og skalerede siden ned for at se, om kortene tilpassede sig, når der var mindre plads.
- **Vi observerede:** Kortene gik fra tre kolonner til to kolonner og derefter én kolonne.
- **Vi ændrede eller mangler:** Her lavede jeg ikke rigtig nogle ændringer

## Eksempel 3: Login med Popover

### Hvor og hvorfor?

Jeg har brugt ''Popover API'' til login, så loginboksen kan åbnes og lukkes direkte fra login knappen uden JavaScript. Jeg bruger ''Anchor Positioning'' til at placere loginboksen lige under login knappen

Findes i: `src/components/Header.astro`

### Relevant kode

```css
@supports (position-anchor: --login-button) {
  .login-panel:popover-open {
    position: fixed;
    position-anchor: --login-button;

    top: anchor(bottom);
    right: anchor(right);
    bottom: auto;
    left: auto;

    margin-top: 12px;
    transform: none;
  }
}
```

### Afprøvning og ændringer

- **Vi testede:** Jeg testede login knappen og tjekkede om loginboksen åbnede det rigtige sted i forhold til knappen.
- **Vi observerede:** I starten åbnede login boksen øverst midt på skærmen i stedet for under login knappen.
- **Vi ændrede eller mangler:** Jeg fik tilføjet ''Anchor Positioning'' så loginboksen blev placeret under login knappen i stedet for midt øverst på skærmen. login boksen åbner nu hvor den skal.

## Fallback og robusthed

Dette må gerne indgå i de tre eksempler ovenfor. Hvis det allerede er dækket dér, kan I slette dette afsnit.

- **Fallback/progressive enhancement:** I FAQ'en har jeg brugt `@supports` til at tjekke, om browseren understøtter `interpolate-size`. Hvis den gør, får FAQ'en en animation, når man åbner og lukker spørgsmålene. Hvis browseren ikke understøtter det, virker FAQ'en stadig, men bare uden animationen. Man kan altså stadig åbne og læse spørgsmålene, selvom animationen ikke virker. Jeg har testet FAQ'en i både Chrome,Safari og Firefox.

- **Defensive CSS:** Jeg har brugt container queries på kortene i “Our core values”. Når der bliver mindre plads, så går kortene først fra fire, til tre også til to kolonner og tilsidst til én kolonne. På den måde bliver kortene ikke presset sammen.

- **Global CSS og komponent-CSS:** Jeg har brugt tokens.css til fælles værdier som spacing, typografi og farver. CSS styling der kun hører til en bestemt side eller komponent, har jeg primært skrevet direkte i de enkelte Astro filer. Jeg er stadig ved at lære Astro at kende og skulle lige finde ud af, hvordan det hele var bygget op. Jeg opdagede først ret sent i opgaven, at jeg nok kunne have brugt komponent-CSS mere. Jeg valgte dog at beholde den struktur jeg havde, da jeg ikke turde ødelægge eller lave rod i koden.

## Brug af AI

Hvis I har brugt AI til en væsentlig del af løsningen, så beskriv kort:

Jeg har brugt AI som hjælp undervejs, hvis jeg har været i tvivl om kode eller haft fejl, jeg ikke selv kunne forstå eller løse. Jeg brugte også AI til at forstå Astro bedre, da jeg ikke var med til introduktionen og ikke havde arbejdet med det før. AI foreslog blandt andet, at jeg delte forsiden op i flere komponenter. Jeg er dog i tvivl om, hvor nødvendigt det var at dele den op i så mange komponenter og om der nok var en anden måde det skulle sættes op på?. Hvis jeg har brugt AI, har jeg brug foreslagene som en hjælp og selv justeret og rettet dem så de passer til, og tjekket efter at det fungere optimalt.

Særligt cirkl diagrammerne var en stor udfodring for mig, jeg kunne ikke få dem til at fungere, prikkerne endte først med at være helt ude fra cirklen, så der måtte jeg have hjælp fra AI til at løse den, og fik til sidst den rimelig tæt på figma designet.

Jeg brugte også AI til loginboksen, fordi jeg havde problemer med at få den placeret rigtigt under login knappen. AI hjalp mig med at ''få Anchor Positioning'' til at fungere.
