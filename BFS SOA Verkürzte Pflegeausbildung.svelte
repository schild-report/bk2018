{#each schueler as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
  <div class="page grid" orientation="portrait" size="A4">
    <Seitenlogo logo="{privat.traegerLogo}"/>
    <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}"/>
    <div class="main">
      <div class="text-center">
        <h5>Bescheinigung für den Antrag zur Verkürzung der <br>Altenpflegeausbildung gemäß § 7 Absatz 2 AltPflG</h5>
      </div>
      <Voffset v="4"/>
      {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},
      <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
      <Voffset v="2"/>
      hat im Schuljahr {hj.Jahr}/{hj.Jahr-1999} die Ausbildung
      {s.Geschlecht === 4 ? "zur" : "zum"} <b>staatlich geprüften
      Sozialassistent{s.Geschlecht === 4 ? 'i' : 'e'}n</b> absolviert.
      <Voffset v="2"/>
      Für den Antrag auf Verkürzung der Altenpflegeausbildung wird folgende
      maßgebliche Gesamtleistung gemäß RdErl. d. Ministeriums für Gesundheit,
      Emanzipation, Pflege und Alter (MGE-PA) v. 19.02.2015 (AZ 402-0422)
      bescheinigt: <b>NOTE</b>
      <Voffset v="2"/>
      Die Bescheinigung gilt nur in Verbindung mit dem Berufsabschlusszeugnis
      vom {datum(hj.ZeugnisDatum)}.
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
            {hj.lehrer.Vorname} {hj.lehrer.Nachname}<br />{hj.klassenlehrer_in}
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
export const kommentar = `
Mindestens gut oder sehr gut, weitere Bestimmungen siehe
[VV](https://bass.schul-welt.de/3129.htm#pgfId-3908297)

[Zeugnis nach Anlage C 10](https://bass.schul-welt.de/3129.htm#AnlageC10)

__Achtung, das Ändern der Praxisnachweise gilt für alle Schüler im Dokument.__
`
  export let schueler, schule, privat, jahr, abschnitt
  import { datum }  from './helfer'
  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
</style>

