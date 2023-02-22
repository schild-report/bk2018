{#each schueler as s}
  <div class="page grid" orientation="portrait" size="A4">
    <Pageheader art="daten/zeugnis.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
    <div class="main">
      Seite 3/3 des Abschlusszeugnisses für {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}
      <Voffset v="2"/>
      <SoaTabelle></SoaTabelle>
    </div>
    <div class="footer klein schulnummer">
      Schulnummer: {schule.SchulNr}
    </div>
  </div>
{/each}

<script>
  export let schueler, schule, privat
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import SoaTabelle from './partials/SOA-Tabelle.svelte'
</script>

<style>
  @import 'css/main.css';
</style>
