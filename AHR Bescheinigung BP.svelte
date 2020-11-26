{#each schueler as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
  <div class="page grid" orientation="portrait" size="A4">
    <Seitenlogo logo="{privat.traegerLogo}"/>
    <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}"/>
    <div class="main">
      <div class="text-center">
        <h4>Bescheinigung</h4>
        <Voffset v="4"/>
        <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}</b>
        <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
        <br>hat den doppelqualifizierenden Bildungsgang
        <Voffset v="2"/>
        <b>Erzieher{s.Geschlecht===3 ? '':'in'}/AHR</b>
      </div>
      <Voffset v="2"/>
      seit dem {datum(s.BeginnBildungsgang)} besucht. {s.Geschlecht===3 ? 'Er':'Sie'} hat die Berechtigung zum Übergang in das Berufspraktikum erworben.
      <Voffset v="9"/>
      <div class="flex-grid">
        <div class="col">
          {schule.Ort}, den {datum(hj.ZeugnisDatum)}
        </div>
        <div class="col">
          <Voffset v="3"/>
          <div class="text-center klein">Siegel</div>
        </div>
        <div class="col">
          <Voffset v="6"/>
          <hr />
          <div class="text-center klein">
            {schule.SchulleiterVorname} {schule.SchulleiterName} <br />{schule.schulleiter_in}
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
  import { datum }  from './helfer'
  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
</style>


