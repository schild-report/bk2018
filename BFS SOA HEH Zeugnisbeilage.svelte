{#each schueler as s}
  <div class="page grid" orientation="portrait" size="A4">
    <Pageheader art="daten/zeugnis.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
    <div class="main">
      Seite 3/3 des Abschlusszeugnisses für {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}
      <Voffset v="2"/>
      <table class="table table-bordered" style="width: 100%">
        <tr>
          <th width="35%">Bereichsspezifische Fächer</th>
          <th>Zugeordnete Lernfelder</th>
        </tr>
        <tr>
          <td>Heilerziehung und Soziales</td>
          <td>
            <ul class="dashes">
              <li>Pädagogische Handlungsprozesse planen, umsetzen, dokumentieren und evaluieren</li>
              <li>Mit Beteiligten am beruflichen Handlungsprozess kommunizieren und kooperieren</li>
              <li>Berufliche Identität entwickeln und Lebenswelten kennen</li>
              <li>Assistierende Beziehungsgestaltung aufbauen</li>
              <li>Mit Konzepten in Einrichtungen der Behindertenhilfe arbeiten</li>
              <li>Kompetenzen, Ressourcen und Potentiale erkennen und entsprechende Situationen gestalten</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>Gesundheitsförderung und Pflege</td>
          <td>
            <ul class="dashes">
              <li>Pflegerische Handlungsprozesse planen, umsetzen, dokumentieren und evaluieren</li>
              <li>Mit Pflege- und Handlungskonzepten in pflegerischen Einrichtungen arbeiten</li>
              <li>Gesundheitserhaltung definieren, berücksichtigen und adressatengerecht anwenden</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>Arbeitsorganisation und Recht</td>
          <td>
            <ul class="dashes">
              <li>Rechtliche und verwaltungstechnische Rahmenbedingungen in der Heilerziehungshilfe berücksichtigen</li>
              <li>Die eigene Person und Dienstleistungen vermarkten</li>
            </ul>
          </td>
        </tr>
      </table>
    </div>
    <div class="footer klein schulnummer">
      Schulnummer: {schule.SchulNr}
    </div>
  </div>
{/each}

<script>
  export const kommentar = `
[Lehrplan, S. 24](https://www.berufsbildung.nrw.de/cms/upload/_lehrplaene/b/gesundheit_erziehung_soziales/bfsb_ges-erz-soz_heilerziehung.pdf)
  `
  export let schueler, schule, privat
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
</script>

<style>
  @import 'css/main.css';
  .table-bordered {
    border-collapse: collapse;
  }
  .table-bordered tr {
    border-bottom: thin solid #333;
  }
  .schulnummer {
    text-align: right;
  }
</style>
