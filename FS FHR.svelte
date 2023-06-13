{#each schueler.filter(s => s.DurchschnittsnoteFHR) as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4">
      <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      <div class="main">
        <h5>Zeugnis der Fachhochschulreife</h5>
        Der allgemeine Prüfungsausschuss stellte in seiner Abschlusskonferenz am {datum(hj.Konferenzdatum)} folgende Leistungen fest<sup>1</sup>:
        <Voffset v="1"/>
        Mathematisch-naturwissenschaftlicher-technischer Bereich: {note(s.bk_abschluss_faecher.find(f => f.fach.FachKrz === 'MFHR').NoteAbschluss)}
        <Voffset v="1"/>
        <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}</b> hat die Fachhochschulreifeprüfung im Bildungsgang
          {bg(s, 'Zeugniskopf')} am {datum(hj.Konferenzdatum)} bestanden.
          {s.anrede}{s.Geschlecht === 3 ? 'n':''} {s.Vorname} {s.Zusatz || ''} {s.Name} wird die
        <Voffset v="1"/>
          <div class="text-center">
            <h5>Fachhochschulreife</h5>
          </div>
        <Voffset v="-1.5"/>
          zuerkannt.
          <Voffset v="1"/>
          Entsprechend der Vereinbarung über den Erwerb einer Fachhochschulreife
          in beruflichen Bildungsgängen – Beschluss der Kultusministerkonferenz vom
          5. Juni 1998 in der jeweils geltenden Fassung – berechtigt dieses Zeugnis in allen
          Ländern der Bundesrepublik Deutschland zum Studium an Fachhochschulen und
          entsprechender Studiengänge an Universitäten.
          <Voffset v="1"/>
        <table class="table-noten">
          <tr>
                <td>Durchschnittsnote:</td><td class="td-fach-note"><span>{s.DurchschnittsnoteFHR}</span></td>
                <td>in&nbsp;Worten:</td><td class="td-fach-note"><span>{@html noteInWorten(s.DurchschnittsnoteFHR)}</span></td>
          </tr>
        </table>
        <Voffset v="5"/>
        {schule.Ort}, den {datum(hj.ZeugnisDatum)}
        <Voffset v="4"/>
        <div class="flex-grid">
          <div class="col">
            <hr />
            <div class="text-center klein">
              {schule.SchulleiterVorname} {schule.SchulleiterName}
              <br>Vorsitzende{schule.SchulleiterGeschlecht === 3 ? 'r':''} des allgemeinen Prüfungsausschusses
            </div>
          </div>
          <div class="col klein">
            <div class="text-center klein">
              Siegel
            </div>
          </div>
          <div class="col klein">
            <hr />
            <div class="text-center">
              {schule.SchulleiterVorname} {schule.SchulleiterName} <br />{schule.schulleiter_in}
            </div>
          </div>
        </div>
        <Voffset v="2"/>
      </div>
      <div class="footer klein">
        <div class="klein eng">
          <Voffset v="2"/>
          <span style="float: right">Seite 3/3</span>
          <span style='float left'>Schulnummer: {schule.SchulNr}</span>
          <hr class="hr-grau" />
          <div class="grau">
            <sup>1</sup>Die Fächer
            {s.bk_abschluss_faecher.filter(f => f.fach.GewichtungFHR > 0)
              .sort((a,b) => a.FSortierung > b.FSortierung ? 1 : -1)
              .map(f => f.fach.Zeugnisbez)
              .join(', ')
              .replace(/, \ sowie/g, ' sowie')}
            gehen in die Berechnung der Durchnittsnote für die Fachhochschulreife ein.
            <Voffset v=".5"/>
            Rechtsbehelfsbelehrung: Gegen dieses Zeugnis kann innerhalb eines Monats
            nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
            ist beim {schule.Bezeichnung1}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
            schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
            das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dieses
            Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
          </div>
        </div>
      </div>
    </div>
  {/each}
{/each}

<script>
  export const kommentar = `
[E7 html](https://bass.schul-welt.de/3129.htm#pgfId-2875772)
[VV zu §16](https://bass.schul-welt.de/3129.htm#13-33nr1.1p16_AnlageE)
`
  export let schueler, schule, privat, jahr, abschnitt
  import { datum, bg, note, noteInWorten } from './helfer'
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




