{#each schueler as s}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4">
      <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      <div class="main">
        <h5>Abschlusszeugnis</h5>
        <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}</b>
        <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
        <br>war vom {datum(s.BeginnBildungsgang)} bis zur Aushändigung des Zeugnisses
        {s.studierende_r} im Bildungsgang
          <Voffset v="1"/>
          <b>{bg(s, 'Zeugniskopf')}</b>
        <Voffset v="1.5"/>
        Der allgemeine Prüfungsausschuss stellte in seiner Abschlusskonferenz am {datum(hj.Konferenzdatum)} fest:
        <Voffset v="1"/>
        {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat das staatliche
        <h5 class="text-center">{bg(s, 'Examen')}</h5>
        <Voffset v="-2"/>
        am {hj.noten.map(n => n.fach.FachKrz === 'KO' ? n.Lernentw : null).join('')} bestanden.<sup>1</sup>
        <Voffset v="2"/>
        <table class="table-noten" width="100%">
          {#each s.bk_abschluss_faecher.filter(f => ['BLgesamt', 'KO', 'BPgesamt'].some(n => n === f.fach.FachKrz)) as f}
          <tr>
            <td class="{f.fach.FachKrz === 'BPgesamt' ? 'fett' : ''}">{f.fach.Zeugnisbez}</td>
            <td class="td-fach-note"><span>{note(f.NoteAbschluss)}</span></td>
          </tr>
          {/each}
        </table>
        <Voffset v="1"/>
        {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} ist berechtigt, die Berufsbezeichnung
        <h5 class="text-center">{bg(s, (s.Geschlecht === 3 ? 'Berufsbezeichnung_m' : 'Berufsbezeichnung_w'))}
        <br><div style="font-size: 80%">(Bachelor Professional im Sozialwesen)</div></h5>
        <Voffset v="-1.5"/>
        zu führen.
        <Voffset v="1"/>
        Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen dem Niveau {s.fachklasse.DQR_Niveau} zugeordnet.
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
          <div class="schulnummer">
            Schulnummer: {schule.SchulNr}
          </div>
          <hr class="hr-grau" />
          <div class="grau">
            Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend,
            4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
            <Voffset v=".5"/>
            <sup>1</sup>Gilt nur in Verbindung mit der Bescheinigung über die
            erfolgreich abgelegte fachtheoretischen Prüfung (Teil I) und dem
            Versetzungszeugnis, welches die Berechtigung zum Übergang in das
            Berufspraktikum ausweist, ausgestellt am {s.vermerke.find(v => v.Vermerkart_ID === 8).Bemerkung}
            vom {s.vermerke.find(v => v.Vermerkart_ID === 9).Bemerkung}
            <Voffset v=".5"/>
            Dem Zeugnis liegen zugrunde: Die Verordnung über die Ausbildung und Prüfung
            in den Bildungsgängen des Berufskollegs (Ausbildungs- und Prüfungsordnung
            Berufskolleg, APO-BK) vom 26. Mai 1999 (SGV. NRW. 223/BASS 13 – 33 Nr. 1.1).
            <br>Die Vereinbarung über Fachschulen (Beschluss der
            Kultusministerkonferenz vom 07. November 2002 in der jeweils geltenden
            Fassung).
            <br>Abschluss der Fachschule entspricht der Rahmenvereinbarung über
            Fachschulen (Beschluss der Kultusministerkonferenz vom 07.11.2002 in
            der jeweils geltenden Fassung) und wird von allen Ländern in der
            Bundesrepublik Deutschland anerkannt.
            <Voffset v=".5"/>
            Rechtsbehelfsbelehrung: Gegen dieses Zeugnis kann innerhalb eines Monats
            nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
            ist beim {schule.Bezeichnung2}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
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
[E5 PDF](https://bass.schul-welt.de/anlagen/3129-60.pdf)
[E5 html]()
`
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




