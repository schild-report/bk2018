{#each schueler as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
  <div class="page grid" orientation="portrait" size="A4">
    <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
    <div class="main">
      <h5>Anlage zum Jahreszeugnis</h5>
      <Voffset v="-1.5"/>
      Bescheinigung für Bewerbungszwecke
      <Voffset v="3"/>
      <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}</b>
      <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort}
      <Voffset v="2"/>
      besucht seit dem {datum(s.BeginnBildungsgang)} die Fachschule für Sozialwesen in der
      {bg(s, 'Bereich')} und hat laut Beschluss des allgemeinen
      Prüfungsausschusses den theoretischen Prüfungsteil des Fachschulexamens
      bestanden. {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat damit die Berechtigung zum Übergang
      in das Berufspraktikum erworben.
      <Voffset v="2"/>
      Der allgemeine Prüfungsausschuss stellte in seiner Abschlusskonferenz am {datum(hj.Konferenzdatum)} fest:
      <Voffset v="1"/>
      <b>Theoretischer Prüfungsteil des Fachschulexamens</b>
      <Voffset v="1"/>
      <table class="table-noten">
        {#each s.bk_abschluss_faecher.filter(f => f.fach.FachKrz.startsWith('AS')) as as}
          <tr>
            <td width="22%" style="height:4rem">{as.fach.Zeugnisbez}</td>
            <td class="td-padding-extra">{findeFach(hj, as).Lernentw}</td>
            <td class="td-fach-note"><span>{note(as.NoteAbschluss)}</span></td>
          </tr>
        {/each}
      </table>
      <Voffset v="9"/>
      <div class="flex-grid">
        <div class="col">
          {schule.Ort}, den {datum(hj.Konferenzdatum)}
        </div>
        <div class="col">
          <Voffset v="3"/>
          <div class="text-center klein">Siegel</div>
        </div>
        <div class="col">
          <Voffset v="6"/>
          <hr />
          <div class="text-center klein">
            {schule.SchulleiterVorname} {schule.SchulleiterName}
            <br>Vorsitzende{schule.SchulleiterGeschlecht === 3 ? 'r':''} des allgemeinen Prüfungsausschusses
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
  import { datum, bg, note } from './helfer'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt
  const findeFach = (hj, as) => hj.noten.find(n => n.fach.ID === as.fach.ID) || { Lernentw: 'undefined' }
</script>

<style>
  @import 'css/main.css';
  .td-fach-note {
    padding-bottom: .2rem;
    padding-top: .2rem;
    width: 12rem;
  }
  .td-fach-note span {
    background-color: #dcdcdc !important;
    text-align: center;
    width: 10rem;
    display: block;
    margin: 0 auto;
  }
</style>




