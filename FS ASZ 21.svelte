{#each schueler as s (s.ID)}
  {#each aktHalbjahrFuer(s) as hj (hj.ID)}
    <div class="page grid" orientation="landscape" size="A3">
      <Seitenlogo logo="{privat.traegerLogo}"/>
      <Pageheader art="daten/asz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}"/>
      <div class="main">
        <div class="main-grid">
          <Voffset v="1"/>
          <div class="main-left">
            <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},</b>
            <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
            <br />war vom {datum(s.Aufnahmedatum)} bis zur Aushändigung des Zeugnisses {s.studierende_r} des Bildungsgangs
            <br /><b>{bg(s, 'Zeugniskopf')}.</b>
            <Voffset v=".5"/>
            Der allgemeine Prüfungsausschuss stellte in seiner Abschlusskonferenz am {datum(hj.Konferenzdatum)} fest:
            <Voffset v=".5"/>
            <b>Leistungen</b>
            <div style="font-size: 90%">
              <Noten
                noten={s.abschnitte[3].noten}
                faechergruppenIds={[10, 20, 30]}
                fachGliederungen={s.fachklasse.fach_gliederungen}
                ger
                lernfeld
                fachklasse={s.fachklasse.Kennung}
                fhrSternchen={s.DurchschnittsnoteFHR ? true: false}
              ></Noten>
            </div>
            <hr />
            <b>Bemerkungen</b>
            <br />{@html bemerkungen(hj)}
          </div>
          <div class="main-right">
            {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat das staatliche
            <h5 class="text-center">{bg(s, 'Examen')}</h5>
            <Voffset v="-2"/>
            am {hj.noten.map(n => n.fach.FachKrz === 'KO' ? n.Lernentw : null).join('')} bestanden.
            <Voffset v="2"/>
            <table class="table-noten">
              {#each s.abschnitte[5].noten.filter(f => f.fach.FachKrz.startsWith('AS')).sort((a,b) => a.fach.Zeugnisbez > b.fach.Zeugnisbez ? 1:-1) as as}
                <tr>
                  <td width="22%">{as.fach.Zeugnisbez}</td>
                  <td class="td-padding-extra">{findeFach(s.abschnitte[3], as).Lernentw}</td>
                  <td class="td-fach-note"><span>{note(as.NotenKrz)}</span></td>
                </tr>
              {/each}
            </table>
            <Voffset v="1"/>
            <table class="table-noten" width="100%">
              {#each s.bk_abschluss_faecher.filter(f => ['BLgesamt', 'KO', 'BPgesamt'].some(n => n === f.fach.FachKrz)).sort((a, b) => a.FSortierung < b.FSortierung ? -1 : 1) as f}
                <tr>
                  <td class="{f.fach.FachKrz === 'BPgesamt' ? 'fett' : ''}">{f.fach.Zeugnisbez}</td>
                  <td class="td-fach-note"><span>{note(f.NoteAbschluss)}</span></td>
                </tr>
              {/each}
            </table>
            <Voffset v="3"/>
            {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} ist berechtigt, die Berufsbezeichnung
            <h5 class="text-center">{bg(s, (s.Geschlecht === 3 ? 'Berufsbezeichnung_m' : 'Berufsbezeichnung_w'))}
            <br><div style="font-size: 80%">(Bachelor Professional im Sozialwesen)</div></h5>
            <Voffset v="-2"/>
            zu führen.
            <Voffset v="1"/>
            Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen dem Niveau {s.fachklasse.DQR_Niveau} zugeordnet.
            <Voffset v="4"/>
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
                  {schule.SchulleiterVorname} {schule.SchulleiterName}
                  <br />{schule.schulleiter_in}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="footer klein grau">
        {#if s.DurchschnittsnoteFHR}
          <span style="float: left; margin-right: 3em;">Seite 1/3</span>  
          <span style="float: right">Seite 2/3</span>
        {/if}
        <span style='float left'>Schulnummer: {schule.SchulNr}</span>
        <hr class="hr-grau" />
        <div class="footer-grid">
          <div class="footer-left">
            Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend, 4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
            <Voffset v="0.5"/>
            <sup>1</sup>Der Unterricht in den modernen Fremdsprachen hat auf der
            nach dem Fach in Klammern angegebenen Niveaustufe des
            „Europäischen Referenzrahmens für Sprachen: Lernen, Lehren,
            Beurteilen“ stattgefunden. Sind zwei Referenzniveaus ausgewiesen, ist
            das niedrigere in vollem Umfang, das höhere in Anteilen erreicht. Bei
            mindestens ausreichenden Leistungen wird der sprachliche
            Kompetenzerwerb auf diesem Niveau bescheinigt.
            <Voffset v="0.5"/>
            {#if s.DurchschnittsnoteFHR}
              <sup>*</sup>Die Noten der gekennzeichneten Fächer gehen in die
              Berechnung der Durchnittsnote für die Fachochschulreife ein.
            {/if}
          </div>
          <div class="footer-right">
            Dem Zeugnis liegen zugrunde:
            <ul class="list-unstyled dashes">
              <li>
                Die Verordnung über die Ausbildung und Prüfung in den Bildungsgängen des Berufskollegs (Ausbildungs- und Prüfungsordnung Berufskolleg, APO-BK) vom 26. Mai 1999 (SGV. NRW. 223/BASS 13 – 33 Nr. 1.1).
              </li>
              <li>
                Die Vereinbarung über Fachschulen (Beschluss der Kultusministerkonferenz vom 07. November 2002 in der jeweils geltenden Fassung).
              </li>
            </ul>
            <br>
            Rechtsbehelfsbelehrung:
            Gegen dieses Zeugnis kann innerhalb eines Monats
            nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
            ist beim {schule.Bezeichnung2}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
            schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
            das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dies
            Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
          </div>
        </div>
      </div>
    </div>
    {#if note(s.bk_abschluss_faecher.find(f => f.fach.FachKrz === 'MFHR'))}
    <div class="page grid" orientation="portrait" size="A4">
      <Seitenlogo logo="{privat.traegerLogo}"/>
      <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}"/>
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
            ist beim {schule.Bezeichnung2}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
            schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
            das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dieses
            Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
          </div>
        </div>
      </div>
    </div>
    {/if}
  {/each}
{/each}

<script>
export const kommentar = `
[E5 PDF](https://bass.schul-welt.de/anlagen/3129-60.pdf)
[E5 html]()
`
  import { datum, bemerkungen, bg, note, noteInWorten }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahrFuer = (s) => s.abschnitte.filter(a => a.Jahr === jahr && a.Abschnitt === abschnitt)
  const findeFach = (hj, as) => hj.noten.find(n => n.fach.ID === as.fach.ID) || { Lernentw: 'undefined' }
</script>

<style>
  @import 'css/main.css';
  .td-padding-extra {
    padding: 2mm 0 2mm 0;
  }
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
