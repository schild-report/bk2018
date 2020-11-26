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
            <br />war vom {datum(s.Aufnahmedatum)} bis zur Aushändigung des Zeugnisses {s.schueler_in} des Bildungsgangs
            <br /><b>{bg(s, 'Zeugniskopf')}.</b>
            <Voffset v=".5"/>
            In der Konferenz am {datum(hj.Konferenzdatum)} sind folgende Leistungen festgestellt worden:
            <Voffset v=".5"/>
            <div style="font-size: 90%">
              <Noten
                noten={s.bk_abschluss_faecher}
                faechergruppenIds={[10, 20, 30]}
                fachGliederungen={s.fachklasse.fach_gliederungen}
                ger
                fachklasse={s.fachklasse.Kennung}
              ></Noten>
            </div>
            <hr />
            <b>Bemerkungen</b>
            <br />{@html bemerkungen(hj)}
          </div>
          <div class="main-right">
            <Voffset v="2"/>
            {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat die
            Fachhochschulreifeprüfung im vorgenannten Bildungsgang der
            Fachoberschule am {datum(hj.Konferenzdatum)} bestanden.
            <Voffset v="2"/>
            Der Nachweis der fachpraktischen Ausbildung wurde durch
            {nachweise[selected]} erbracht.
            <div class="no-print">
              <select bind:value="{selected}" class="no-print">
                {#each nachweise as nachweis,i}
                  <option value="{i}">
                    {nachweis}
                  </option>
                {/each}
              </select></div>
            <Voffset v="1"/>
            {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} wird die
            <h4 class="text-center">Fachhochschulreife</h4>
            zuerkannt.
            <Voffset v="1"/>
            Der Abschluss ist im Deutschen und Europäischen
            Qualifikationsrahmen dem Niveau {s.fachklasse.DQR_Niveau}
            zugeordnet. Entsprechend der Vereinbarung über die
            Fachoberschulen – Beschluss der Kultusministerkonferenz vom 16.
            Dezember 2004 in der jeweils geltenden Fassung – berechtigt
            dieses Zeugnis in allen Ländern der Bundesrepublik Deutschland
            zum Studium an Fachhochschulen.
            <Voffset v="2"/>
            <table width="100%">
              <tr>
                <td>Durchschnittsnote:</td><td class="td-fach-note"><span>{s.DurchschnittsnoteFHR}</span></td>
                <td>in&nbsp;Worten:</td><td class="td-fach-note"><span>{@html noteInWorten(s.DurchschnittsnoteFHR)}</span></td>
               </tr>
            </table>
            <Voffset v="6"/>
            {schule.Ort}, den {datum(hj.ZeugnisDatum)}
              <Voffset v="6"/>
            <div class="flex-grid">
              <Voffset v="6"/>
              <div class="col">
                <hr />
                <div class="text-center klein">
                  {schule.SchulleiterVorname} {schule.SchulleiterName} Vorsitzende{schule.SchulleiterGeschlecht === 3 ? 'r':''} des allgemeinen
                  <br />Prüfungsausschusses
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
          </div>
        </div>
      </div>
      <div class="footer klein">
        <div class="schulnummer">
          Schulnummer: {schule.SchulNr}
        </div>
        <hr class="hr-grau" />
        <div class="footer-grid">
          <div class="footer-left">
            Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend, 4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
            <p></p>
            <sup>1</sup>Der Unterricht in den modernen Fremdsprachen hat auf der
            nach dem Fach in Klammern angegebenen Niveaustufe des
            „Europäischen Referenzrahmens für Sprachen: Lernen, Lehren,
            Beurteilen“ stattgefunden. Sind zwei Referenzniveaus ausgewiesen, ist
            das niedrigere in vollem Umfang, das höhere in Anteilen erreicht. Bei
            mindestens ausreichenden Leistungen wird der sprachliche
            Kompetenzerwerb auf diesem Niveau bescheinigt.
          </div>
          <div class="footer-right">
            Dem Zeugnis liegen zugrunde:
            <ul class="list-unstyled dashes">
              <li>
                Die Verordnung über die Ausbildung und Prüfung in den
                Bildungsgängen des Berufskollegs (Ausbildungs- und
                Prüfungsordnung Berufskolleg, APO-BK) vom 26. Mai 1999 (SGV.
                NRW. 223/BASS 13 – 33 Nr. 1.1).
              </li>
              <li>
                die Rahmenvereinbarung über die Fachoberschulen (Beschluss
                der Kultusministerkonferenz vom 16. Dezember 2004 in der
                jeweils geltenden Fassung).
              </li>
            </ul>
            <br>
            Rechtsbehelfsbelehrung:
            <br>Gegen dieses Zeugnis kann innerhalb eines Monats
            nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
            ist beim {schule.Bezeichnung2}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
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
[Anlage C](https://bass.schul-welt.de/3129.htm#13-33nr1.1p18_AnlageC)

[Zeugnis nach Anlage C 10](https://bass.schul-welt.de/3129.htm#AnlageC10)

__Achtung, das Ändern der Praxisnachweise gilt für alle Schüler im Dokument.__
`
  import { datum, bemerkungen, bg, note, noteInWorten }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  let selected = 0
  let nachweise = [
		'die erfolgreiche Ableistung des Praktikantenjahres im Rahmen der Klasse 11 der Fachoberschule',
		'den erfolgreichen Abschluss einer mindestens zweijährigen Berufsausbildung',
		'den Nachweis einer mindestens vierjährigen einschlägigen Berufstätigkeit als XXX'
	]
  const aktHalbjahrFuer = (s) => s.abschnitte.filter(a => a.Jahr === jahr && a.Abschnitt === abschnitt)
</script>

<style>
  @import 'css/main.css';
  @import 'css/a3-landscape.css';
  .td-fach-note span {
    background-color: #dcdcdc !important;
    text-align: center;
    width: 10rem;
    display: block;
  }
  table {
    border-collapse: collapse;
  }
</style>
