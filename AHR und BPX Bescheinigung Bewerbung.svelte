{#each schueler as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
  <div class="page grid" orientation="portrait" size="A4">
    <Pageheader logo={privat.logo} untertitel={privat.untertitel} traeger="{privat.traeger}" />
    <div class="main">
      <h4>Bescheinigung für Bewerbungszwecke</h4>
      <Voffset v="4"/>
      <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}</b>
      <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
      <br>ist seit dem {datum(s.BeginnBildungsgang)} bis zur Aushändigung des Zeugnisses Schüler{s.Geschlecht===3 ? '':'in'} des Bildungsgangs
      <b>{bg(s, 'Zeugniskopf')}</b>
      <Voffset v="2"/>
      Der allgemeine Prüfungsausschuss stellte in seiner Abschlusskonferenz am {hj.noten.find(n=>n.fach.FachKrz==='KO')?.Lernentw} fest:
      <Voffset v="1"/>
      {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat die staatliche <b>Berufsabschlussprüfung</b> am {hj.noten.find(n=>n.fach.FachKrz==='KO')?.Lernentw} bestanden.
      <Voffset v="1"/>
      <table width="80%">
        <tr>
          <td>Berufspraktische Leistungen</td>
          <td class="td-fach-note"><span>{note(hj.noten.find(n => n.fach.FachKrz === 'BLgesamt')?.NotenKrz)}</span></td>
        </tr>
        <tr>
          <td>Kolloquium</td>
          <td class="td-fach-note"><span>{note(hj.noten.find(n => n.fach.FachKrz === 'KO')?.NotenKrz)}</span></td>
        </tr>
        <tr>
          <td><b>Gesamtnote Berufspraxis</b></td>
          <td class="td-fach-note"><b><span>{note(hj.noten.find(n => n.fach.FachKrz === 'BPgesamt')?.NotenKrz)}</span></b></td>
        </tr>
      </table>
      <Voffset v="1"/>
      {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} ist mit Abschluss des Berufspraktikums am {hj.noten.find(n=>n.fach.FachKrz==='KO')?.Lernentw} berechtigt, die Berufsbezeichnung
      <b>{bg(s, (s.Geschlecht === 3 ? 'Berufsbezeichnung_m' : 'Berufsbezeichnung_w'))}</b> zu führen.
      <Voffset v="1"/>
      Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen dem Niveau {s.fachklasse.DQR_Niveau} zugeordnet.
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



