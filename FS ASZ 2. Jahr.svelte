{#each schueler as s (s.ID)}
  {#each aktHalbjahrFuer(s) as hj (hj.ID)}
    <div class="page grid" orientation="landscape" size="A3" style="font-size: 87%;">
      <Pageheader art="daten/asz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      <div class="main">
        <div class="main-grid">
          <Voffset v="2"/>
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
                noten={hj.noten}
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
              {#each hj.noten.filter(f => f.fach.FachKrz.startsWith('AS')).sort((a,b) => a.FSortierung > b.FSortierung ? 1:-1) as as}
                <tr>
                  <td width="22%">{as.fach.Zeugnisbez}</td>
                  <td class="td-padding-extra">{as.Lernentw}</td>
                  <td class="td-fach-note"><span>{note(as.NotenKrz)}</span></td>
                </tr>
              {/each}
            </table>
            <Voffset v="1"/>
            <table class="table-noten" width="100%">
              {#each hj.noten.filter(f => ['BLgesamt', 'KO', 'BPgesamt'].some(n => n === f.fach.FachKrz)).sort((a, b) => a.FSortierung < b.FSortierung ? -1 : 1) as f}
                <tr>
                  <td class="{f.fach.FachKrz === 'BPgesamt' ? 'fett' : ''}">{f.fach.Zeugnisbez}</td>
                  <td class="td-fach-note"><span>{note(f.NotenKrz)}</span></td>
                </tr>
              {/each}
            </table>
            <Voffset v="4"/>
            {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} ist berechtigt, die Berufsbezeichnung
            <h5 class="text-center">{bg(s, (s.Geschlecht === 3 ? 'Berufsbezeichnung_m' : 'Berufsbezeichnung_w'))}
            <br><div style="font-size: 80%">(Bachelor Professional im Sozialwesen)</div></h5>
            <Voffset v="-2"/>
            zu führen.
            <Voffset v="1"/>
            Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen dem Niveau {s.fachklasse.DQR_Niveau} zugeordnet.
            <Voffset v="5"/>
            {schule.Ort}, den {datum(hj.ZeugnisDatum)}
            <Voffset v="5"/>
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
        <div class="schulnummer">
          Schulnummer: {schule.SchulNr}
        </div>
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
            ist beim {schule.Bezeichnung1}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
            schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
            das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dies
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
  import { datum, bemerkungen, bg, note }  from './helfer'

  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahrFuer = (s) => s.abschnitte.filter(a => a.Jahr === jahr && a.Abschnitt === abschnitt)
  const findeFach = (hj, as) => hj.noten.find(n => n.fach.ID === as.fach.ID) || { Lernentw: 'undefined' }
</script>

<style>
  @import 'css/main.css';
  @import 'css/a3-landscape.css';
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
