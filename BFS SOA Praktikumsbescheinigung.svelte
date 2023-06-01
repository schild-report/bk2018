{#each schueler as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
  <div class="page grid" orientation="portrait" size="A4">
    <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
    <div class="main">
      <Voffset v="8"/>
      <div class="text-center">
      <h5>Praktikumsbescheinigung</h5>
      </div>
      <Voffset v="4"/>
      <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},</b>
      <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
      <Voffset v="2"/>
      hat im Ausbildungszeitraum vom {datum(s.BeginnBildungsgang)} bis {datum(s.Entlassdatum)} im Rahmen der Ausbildung {bg(s, `Berufsbezeichnung_${s.Geschlecht===3?'m':'w'}`).replace('<br>','')} folgende Praktika absolviert:
      <Voffset v="2"/>
      1. Vier Wochen Praktikum in einer Tageseinrichtung für Kinder
      <br>Beurteilung des Praktikums: {hj.noten.find(f => f.fach.FachKrz === "PRXSOA 1").NotenKrz}
      <Voffset v="2"/>
      2. Sechs Wochen Praktikum in der ambulanten oder stationären Altenhilfe
      <br>Beurteilung des Praktikums: {hj.noten.find(f => f.fach.FachKrz ===  "PRX SOA 2").NotenKrz}
      <Voffset v="2"/>
      3.  Sechs Wochen Praktikum in der Eingliederungshilfe
      <br>Beurteilung des Praktikums: {hj.noten.find(f => f.fach.FachKrz === "PRXSOA 3").NotenKrz}
      <Voffset v="7"/>
      <div class="flex-grid">
        <div class="col">
          {schule.Ort}, den {datum(hj.ZeugnisDatum)}
        </div>
        <div class="col">
          <Voffset v="3"/>
          <div class="text-center klein">Siegel</div>
        </div>
        <div class="col">
          <hr />
          <div class="text-center klein">
            {hj.lehrer?.Titel ? hj.lehrer.Titel+" " : ''}{hj.lehrer?.Vorname} {hj.lehrer?.Nachname}<br />{hj.klassenlehrer_in}
          </div>
          <Voffset v="6"/>
        </div>
      </div>
      <Voffset v="2"/>
    </div>
    <div class="footer klein" style="text-align:right">
      Schulnummer: {schule.SchulNr}
    </div>
  </div>
{/each}
{/each}

<script>
  export let schueler, schule, privat, jahr, abschnitt
  import { datum, bg }  from './helfer'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
  @import 'css/a4-portrait.css';
</style>

