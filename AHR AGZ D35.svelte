{#each schueler as s (s.ID)}
  {#each aktHalbjahrFuer(s) as hj (hj.ID)}
    <div class="page grid" orientation="landscape" size="A3">
      <div class="header">
        <Pageheader art="daten/agz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      </div>
      <div class="main-grid">
        <div class="main-left">
          <div style="font-size: 12pt; line-height: 1.5em">Abgangszeugnis gemäß §13 APO-BK, Anlage D</div>
          <div style="font-size: 11pt; line-height: 1.2em">Berufliches Gymnasium, {bg(s, 'Bereich')}</div>
          <Voffset v="0.5"/>
          {s.anrede} <b>{s.Vorname} {s.Zusatz || ''} {s.Name}</b>, geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
          <br />hat vom {datum(s.Aufnahmedatum)} bis zum {datum(s.Entlassdatum)} den <b>Bildungsgang {bg(s, 'Zeugniskopf')}</b> besucht.
          <br>{s.Geschlecht === 3 ? 'Er':'Sie'} war zuletzt {s.schueler_in} im {hj.Abschnitt}. Halbjahr der Jahrgangstufe {hj.Jahrgang+10}.
          <Voffset v="0.5"/>
          In der Konferenz am {datum(hj.Konferenzdatum)} sind folgende Leistungen festgestellt worden:
          <table class="table table-bordered ">
            <tr>
              <td>Fach</td>
              <td>Aufg.-<br>feld<sup>2</sup></td>
              <td>Kurs-<br>Art<sup>3</sup></td>
              <td colspan="2" class="text-center">12.1<sup>4</sup></td>
              <td colspan="2" class="text-center">12.2<sup>4</sup></td>
              <td colspan="2" class="text-center">13.1<sup>4</sup></td>
              <td colspan="2" class="text-center">13.2<sup>4</sup></td>
            </tr>
            <tr>
              <td colspan="3"></td>
              <td class="text-center">Note</td>
              <td class="text-center">Punkte</td>
              <td class="text-center">Note</td>
              <td class="text-center">Punkte</td>
              <td class="text-center">Note</td>
              <td class="text-center">Punkte</td>
              <td class="text-center">Note</td>
              <td class="text-center">Punkte</td>
            </tr>
            {#each s.abi_abschluss_faecher.sort((a, b) => a.FSortierung < b.FSortierung ? -1 : 1) as f}
              <tr>
                <td>{f.fach.Bezeichnung}
                  {#if f.fach.IstSprache === '+'}{fremdsprache(f, s.fachklasse.Kennung)}<sup>1</sup>{/if}
                </td>
                <td class="text-center">{'I'.repeat(f.fach.Aufgabenfeld)}</td>
                <td class="text-center">{f.KursartAllg || 'GK'}</td>
                <td class="text-center">{punkte2note(f.P12_1) || '–'}</td>
                <td class="text-center">{fhrFach(s, f, '2_1') || '–'}</td>
                <td class="text-center">{punkte2note(f.P12_2) || '–'}</td>
                <td class="text-center">{fhrFach(s, f, '2_2') || '–'}</td>
                <td class="text-center">{punkte2note(f.P13_1) || '–'}</td>
                <td class="text-center">{fhrFach(s, f, '3_1') || '–'}</td>
                <td class="text-center">{punkte2note(f.P13_2) || '–'}</td>
                <td class="text-center">{fhrFach(s, f, '3_2') || '–'}</td>
              </tr>
            {/each}
          </table>
          <Voffset v="0.5"/>
          <b>Bemerkungen:
          <br></b> {@html bemerkungen(hj)}
        </div>
        <div class="main-right">
          <Voffset v="2"/>
          Die Kurse des letzten Halbjahres gelten als {zeige_Kurse_abgeschlossen ? '':'nicht'} abgeschlossen.
          <button class="vbutton no-print" on:click="{() => zeige_Kurse_abgeschlossen = !zeige_Kurse_abgeschlossen}">
            Kurse abgeschlossen: {zeige_Kurse_abgeschlossen ? 'Ja' : 'Nein'}
          </button>
          {#if zeige_FHR}
            <Voffset v="2"/>
            Dieses Zeugnis gilt in Verbindung mit dem Nachweis über eine
            abgeschlossene Berufsausbildung oder ein einjähriges gelenktes
            Praktikum gemäß der Praktikum - Ausbildungsordnung vom 11.12.2006
            (BASS 13 -31 Nr.1) als Nachweis der Fachhochschulreife. Es
            berechtigt zum Studium an einer Fachhochschule des Landes
            Nordrhein-Westfalen. Dieses Zeugnis gilt auch als Nachweis des
            schulischen Teils der Fachhochschulreife in den Ländern
            Baden-Württemberg, Berlin, Brandenburg, Bremen, Hamburg, Hessen,
            Mecklenburg-Vorpommern, Niedersachsen, Rheinland-Pfalz, Saarland,
            Sachsen-Anhalt, Schleswig-Holstein und Thüringen.
            <Voffset v="2"/>
            <table class="table-noten">
              <tr>
                <td>Durchschnittsnote:</td>
                <td class="td-fach-note"><span>{s.fhr_abschluss && s.fhr_abschluss.Note}</span></td>
                <td>in&nbsp;Worten:</td>
                <td class="td-fach-note"><span>{@html s.fhr_abschluss && s.fhr_abschluss.Note && noteInWorten(s.fhr_abschluss.Note)}</span></td>
              </tr>
            </table>
          {/if}
          <button class="vbutton no-print" on:click="{() => zeige_FHR = !zeige_FHR}">
            FHR abgeschlossen: {zeige_FHR ? 'Ja' : 'Nein'}
          </button>
        </div>
        <div class="footer-right">
          <div class="flex-grid">
            <div class="col-2">
              {schule.Ort}, den {datum(hj.ZeugnisDatum)}
            </div>
            <div class="col text-center klein">
              <Voffset v="2"/>
              <hr />
              {schule.SchulleiterVorname} {schule.SchulleiterName}
              <br />{schule.schulleiter_in}
            </div>
            <div class="col text-center klein">
              <Voffset v="2"/>
              Siegel
            </div>
            <div class="col text-center klein">
              <Voffset v="2"/>
              <hr />
              {hj.lehrer?.Titel ? hj.lehrer.Titel+" " : ''}{hj.lehrer?.Vorname} {hj.lehrer?.Nachname}<br />{hj.klassenlehrer_in}
            </div>
          </div>
          <Voffset v="0"/>
        </div>
      </div>
      <div class="footer-grid klein eng">
        <div class="header">
          Schulnummer: {schule.SchulNr}
          <hr />
          <Voffset v="1"/>
        </div>
        <div class="footer-left">
          <sup>1</sup>Der Unterricht in den modernen Fremdsprachen hat auf der nach dem Fach in
          Klammern angegebenen Niveaustufe des „Europäischen Referenzrahmens für
          Sprachen: Lernen, Lehren, Beurteilen“ stattgefunden. Sind zwei
          Referenzniveaus ausgewiesen, ist das niedrigere in vollem Umfang, das höhere
          in Anteilen erreicht. Bei mindestens ausreichenden Leistungen (5 Punkte)
          wird der sprachliche Kompetenzerwerb auf diesem Niveau bescheinigt.
          <Voffset v=".5"/>
          <table>
            <tr>
              <td rowspan="4" valign="top" width="5px" ><sup>2</sup> </td>
              <td width="10px">I</td>
              <td>= sprachlich literarisches Aufgabenfeld</td>
              <td rowspan="4" valign="top" width="5px" ><sup>3</sup> </td>
              <td width="10px">LK</td>
              <td>= Leistungskurs</td>
            </tr>
            <tr>
              <td>II</td>
              <td>= gesellschaftswissenschaftliches Aufgabenfeld</td>
              <td width="10px">GK</td>
              <td>= Grundkurs</td>
            </tr>
            <tr>
              <td>III</td>
              <td>= mathematisch-naturwissenschaftlich-technisches Aufgabenfeld</td>
            </tr>
          </table>
          <sup>4</sup> Für die Umsetzung von Noten in Punkte gilt:
          <table class="table table-bordered table-eng" width="80%">
            <tr>
              <td width="5%"></td>
              {#each ['sehr gut', 'gut', 'befriedigend', 'ausreichend', 'mangelhaft', 'ungenügend'] as n}
                <td class="text-center" width="10%">{n}</td>
              {/each}
            </tr>
            <tr>
              <td>Noten</td>
              {#each [1,2,3,4,5] as i}
                <td class="text-center">
                  <div class="flex-grid">
                    <div class="col">+</div>
                    <div class="col">{i}</div>
                    <div class="col">-</div>
                  </div>
                </td>
              {/each}
              <td class="text-center">6</td>
            </tr>
            <tr>
              <td>Punkte</td>
              {#each [[15,14,13],[12,11,10],['09','08','07'],['06','05','04'],['03','02','01']] as ii}
                <td class="text-center">
                  <div class="flex-grid">
                    {#each ii as i}
                      <div class="col">{i}</div>
                    {/each}
                  </div>
                </td>
              {/each}
              <td class="text-center">00</td>
            </tr>
          </table>
          <Voffset v=".5"/>
          <table>
            <tr>
              <td width="3%">E1</td>
              <td width="40%">= Mit besonderem Erfolg teilgenommen</td>
              <td width="3%">E2</td>
              <td width="30%">= Mit Erfolg teilgenommen</td>
              <td width="3%">E3</td>
              <td width="15%">= Teilgenommen</td>
            </tr>
          </table>
          Die Punkte in Klammern sind nicht in die Durchschnittsnote eingerechnet worden.
        </div>
        <div class="footer-right">
          Dem Zeugnis liegen zugrunde:<br>
          Die Verordnung über die Ausbildung und Prüfung
          in den Bildungsgängen des Berufskollegs (Ausbildungs- und Prüfungsordnung
          Berufskolleg, APO-BK) vom 26. Mai 1999 (SGV. NRW. 223/BASS 13 – 33 Nr. 1.1).
          <Voffset v=".5"/>
          Rechtsbehelfsbelehrung:<br>
          Gegen dieses Zeugnis kann innerhalb eines Monats
          nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
          ist beim {schule.Bezeichnung2}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
          schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
          das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dies
          Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
        </div>
      </div>
    </div>
  {/each}
{/each}

<script>
  export const kommentar = `
  *Achtung* die FHR wird nicht automatisch vergeben, Voraussetzungen beachten
- [§13 zu AGZ 12, 13,](https://bass.schul-welt.de/3129.htm#pgfId-1289625)
- [VV 13.4 zu Absatz 4](https://bass.schul-welt.de/3129.htm#pgfId-3910131)
- [FHR bei Abgängern](https://bass.schul-welt.de/3129.htm#13-33nr1.1p13a_AnlageD)
- [Sprachniveaus](https://bass.schul-welt.de/3129.htm#pgfId-3910826)
- [AGZ D35](https://bass.schul-welt.de/3129.htm#AnlageD35)
- [Kurse abgeschlossen? Bemerkung](https://bass.schul-welt.de/3129.htm#pgfId-3910135)
  `
  import { datum, bemerkungen, bg, punkte2note, noteInWorten, fremdsprache }  from './helfer'

  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  let zeige_FHR, zeige_Kurse_abgeschlossen
  const aktHalbjahrFuer = s => [s.abschnitte.find(a => a.Jahr === jahr && a.Abschnitt === abschnitt)]

  const fhrFach = (s, fach, id) => {
    const pid = 'P1' + id
    if (!fach[pid]) return
    const fach2 = s.fhr_abschluss_faecher.find(f => f.Fach_ID === fach.Fach_ID)
    const rsid = 'RSII_' + id
    return fach2[rsid] === '+' ? fach[pid] : `(${fach[pid]})`
  }
</script>

<style>
  @import 'css/main.css';
  .table-bordered {
    border-collapse: collapse;
  }
  .table-bordered td {
    border: thin solid #ddd;
    line-height: 1.5rem;
  }
  .table-eng {
    margin-bottom: 0;
  }
  .table-noten {
    width: 70%;
    border: none;
    border-collapse: collapse;
    margin-bottom: 1rem;
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
