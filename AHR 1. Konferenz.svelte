{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4">
      <div class="header">
        <Seitenlogo logo="{privat.traegerLogo}"/>
        <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}"/>
      </div>
      <div class="main">
        <b>Abiturprüfung {hj.Jahr+1}</b><br />Ergebnis der 1. Konferenz des allgemeinen Prüfungsausschusses
        <br />{s.anrede} <b>{s.Vorname} {s.Zusatz || ''} {s.Name}</b>
        <Voffset v="0.5"/>
        <table class="table table-bordered table-eng">
          <tr>
            <td rowspan="3" width="40px">Abitur- fach</td>
            <td rowspan="3">Fach – In der Reihenfolge der erlassenen Stundentafel</td>
            <td colspan="4">Leistungsbewertung in den Halbjahren der Qualifikationsphase</td>
            <td colspan="3">Zur Zulassung angerechnete Punkte</td>
            <td rowspan="3">Durchschnitts- punktzahl</td>
          </tr>
          <tr>
            <td class="text-center" rowspan="2">12.1</td>
            <td class="text-center" rowspan="2">12.2</td>
            <td class="text-center" rowspan="2">13.1</td>
            <td class="text-center" rowspan="2">13.2</td>
            <td class="text-center" rowspan="2">Grund- kurse</td>
            <td class="text-center" colspan="2">Leistungskurse</td>
          </tr>
          <tr>
            <td class="text-center">einfach</td>
            <td class="text-center">zweifach</td>
          </tr>
          {#each s.abi_abschluss_faecher.sort((a,b) => a.FSortierung - b.FSortierung) as f}
            <tr>
              <td class="text-center">{f.AbiFach || ''}</td>
              <td>{f.fach.Bezeichnung}</td>
              <td class="text-center">{f.R12_1 === '-' ? `(${f.P12_1 || '–'})` : f.P12_1}</td>
              <td class="text-center">{f.R12_2 === '-' ? `(${f.P12_2 || '–'})` : f.P12_2}</td>
              <td class="text-center">{f.R13_1 === '-' ? `(${f.P13_1 || '–'})` : f.P13_1}</td>
              <td class="text-center">{f.R13_2 === '-' ? `(${f.P13_2 || '–'})` : f.P13_2}</td>
              <td class="text-center">{f.KursartAllg === "GK" ? f.Zulassung || '**' : '**'}</td>
              <td class="text-center">{f.KursartAllg === "LK" ? f.Zulassung/2 : '**'}</td>
              <td class="text-center">{f.KursartAllg === "LK" ? f.Zulassung : '**'}</td>
              <td class="text-center">{f.Durchschnitt ? f.Durchschnitt.toFixed(2) : '**'}</td>
            </tr>
          {/each}
          <tr>
            <td colspan="2"></td>
            <td colspan="3"><b>Summe der Punkte</b></td>
            <td></td>
            <td class="text-center">{s.abi_abschluss.SummeGK}</td>
            <td></td>
            <td class="text-center">{s.abi_abschluss.SummeLK}</td>
          </tr>
          <tr>
            <td colspan="9"><br /></td>
          </tr>
          <tr>
            <td colspan="2"></td>
            <td colspan="6"><b>Gesamtsumme</b></td>
            <td class="text-center">{s.abi_abschluss.SummeGK + s.abi_abschluss.SummeLK}</td>
          </tr>
          <tr>
            <td colspan="2"></td>
            <td colspan="6"><b>Anzahl der eingebrachten Kurse</b></td>
            <td class="text-center">{s.abi_abschluss.Kurse_I}</td>
          </tr>
        </table>
        Die Punktzahlen in Klammern wurden nicht in die Zulassungsberechnung einbezogen.
        <Voffset v="1"/>
        <div class="flex-grid">
          <div class="col-2">Berechnung der Punktsumme im Block I gemäß § 15 in Verbindung mit § 25 Absatz 3 APO-BK Anlage D:</div>
          <div class="col text-center"><h4>{s.abi_abschluss.Punktsumme_I}</h4></div>
        </div>
        Die Prüfung der Zulassung erfolgte unter Berücksichtigung aller erbrachten Leistungen in der
        Qualifikationsphase mit der Maßgabe der Erreichung einer höchstmöglichen Punktzahl (für den
        Block I) gemäß § 15 Absatz 2 APO-BK Anlage D. Falls der Prüfling beantragen möchte, dass
        Änderungen bezüglich der Einbringung für die Berechnung der Punktsumme im Block I
        vorgenommen werden, so ist dies innerhalb von drei Werktagen nach der ersten Konferenz schriftlich
        gegenüber der Schule zu erklären.
        <Voffset v="0.5"/>
        {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} ist zur Abiturprüfung
        <b>{s.abi_abschluss.Zugelassen === '+' ? '' : 'nicht'} zugelassen.</b>
        {#if s.abi_abschluss.Zugelassen !== '+'}
          <br>Es liegen folgende Verstöße gegen die Zulassungsbedingung(en)
          nach § 15 APO-BK Anlage D vor:
          <ul>
            {#if s.abi_abschluss.Kurse_I < 200}<li>Nichterreichen von mindestens 200 Punkten im Block I</li>{/if}
            {#if s.abi_abschluss.AnzahlKurse_0 > 0}<li>Bewertung eines einzubringenden Kurses mit null Punkten</li>{/if}
            {#if (s.abi_abschluss.Kurse_I === 32 && s.abi_abschluss.Defizite_I > 6)
              || (s.abi_abschluss.Kurse_I < 38   && s.abi_abschluss.Defizite_I > 7)
              || (s.abi_abschluss.Kurse_I < 41   && s.abi_abschluss.Defizite_I > 8)
              && (s.abi_abschluss.LK_Defizite_I < 4)}
              <li>Überschreitung der maximal zulässigen Anzahl einzubringender Kurse mit weniger als fünf Punkten</li>
            {/if}
          </ul>
        {/if}
			</div>
      <div class="footer">
        <div class="flex-grid">
          <div class="col">
            {schule.Ort}, den {datum(hj.Konferenzdatum)}
          </div>
          <div class="col text-center klein">
            <Voffset v="3"/>
            <hr />
            {schule.SchulleiterVorname} {schule.SchulleiterName}
            <br />Vorsitzender des allgemeinen Prüfungsausschusses
          </div>
        </div>
      </div>
    </div>
  {/each}
{/each}
<script>
  export const kommentar = `
[Anlage D 33b](https://bass.schul-welt.de/3129.htm#AnlageD33b)

[Bass § 15](https://bass.schul-welt.de/3129.htm#pgfId-3911082)
  `
  import { datum }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
  .table-bordered {
    border-collapse: collapse; }
  .table-bordered tr td {
    border: thin solid #ddd;
    line-height: 1.3rem }
  .table-eng {
    margin-bottom: 0;
  }
</style>