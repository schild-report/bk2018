{#each schueler as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
  <div class="page grid" orientation="portrait" size="A4">
    <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
    <div class="main">
      <Voffset v="8"/>
      <div class="text-center">
        <h5>Zertifikat über die fachliche Qualifikation
        <br>zur Betreuungskraft nach § 43b SGB XI</h5>
      </div>
      <Voffset v="4"/>
      {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},
      <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
      <Voffset v="2"/>
      wird bescheinigt, dass {s.Geschlecht===3?'er':'sie'} im Rahmen der Ausbildung
      {bg(s, `Berufsbezeichnung_${s.Geschlecht===3?'m':'w'}`).replace('<br>','')} die erforderlichen fachlichen Kenntnisse
      zur Betreuungskraft gemäß Richtlinien nach § 43b SGB XI
      (Betreuungskräfte-RL vom 1. Januar 2017 in der jeweils geltenden
      Fassung) erworben hat.
      <Voffset v="1.5"/>
      {#if s.fachklasse.Kennung === "93-103-01"}
        Grundlage für das Zertifikat sind der Bildungsplan (APO-BK Anlage B3)
        und die didaktische Jahresplanung (in der jeweils gültigen Fassung).
      {/if}
      <Voffset v="1.5"/>
      Dieses Zertifikat gilt in Verbindung mit dem Abschlusszeugnis des
      Bildungsganges der Berufs- fachschule {bg(s, `Berufsbezeichnung_${s.Geschlecht===3?'m':'w'}`).replace('<br>','')} vom
      {datum(hj.ZeugnisDatum)}.
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
          <hr />
          <div class="text-center klein">
            {schule.SchulleiterVorname} {schule.SchulleiterName}
            <br />{schule.schulleiter_in}
          </div>
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
</style>

