{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4" style="font-size: 1.0rem">
      <div class="header">
        <Seitenlogo logo="{privat.traegerLogo}"/>
        <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}"/>
      </div>
      <div class="main">
        <div style="font-size: 12pt; line-height: 1.5em">Bescheinigung über die Schullaufbahn gemäß §13 APO-BK, Anlage D</div>
        <div style="font-size: 11pt; line-height: 1.2em">Berufliches Gymnasium, Bereich Gesundheit und Soziales</div>
        <Voffset v="0.5"/>
        {s.anrede} <b>{s.Vorname} {s.Zusatz || ''} {s.Name}</b>, geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
        <br />besuchte im Schuljahr {hj.schuljahr} die Jahrgangsstufe {parseInt(hj.ASDJahrgang)+10}.{hj.Abschnitt} im Bildungsgang
        <br /><b>{bg(s, 'Bereich')}</b>.
        <Voffset v="0.5"/>
        In der Konferenz am {datum(hj.Konferenzdatum)} sind folgende Leistungen festgestellt worden:
        <table class="table table-bordered table-eng">
          <tr>
            <td rowspan="2">Fach<sup>1</sup></td>
            <td rowspan="2" width="20px">Aufg.- feld<sup>2</sup></td>
            <td rowspan="2" width="20px">Abitur- fach<sup>3</sup></td>
            {#if s.ASDSchulform === 'D01'}
              <td rowspan="2" width="20px">Fach der Berufs- abschlussprüfung</td>
            {/if}
            <td colspan="4">Leistungen im Halbjahr<sup>4, 5</sup></td>
          </tr>
          <tr>
            <td class="text-center">12.1</td>
            <td class="text-center">12.2</td>
            <td class="text-center">13.1</td>
            <td class="text-center">13.2</td>
          </tr>
          {#each s.abi_abschluss_faecher.sort((a, b) => a.FSortierung < b.FSortierung ? -1 : 1) as f}
            <tr>
              <td>{f.fach.Zeugnisbez}</td>
              <td class="text-center">{'I'.repeat(f.fach.Aufgabenfeld)}</td>
              <td class="text-center">{f.AbiFach || ''}</td>
              {#if s.ASDSchulform === 'D01'}
                <td class="text-center">{(parseInt(f.AbiFach) > 0 && parseInt(f.AbiFach) < 4) || f.FachKrz === 'DM' ? '+' : ''}</td>
              {/if}
              <td class="text-center">{f.P12_1 || '–'}</td>
              <td class="text-center">{f.P12_2 || '–'}</td>
              <td class="text-center">{f.P13_1 || '–'}</td>
              <td class="text-center">{f.P13_2 || '–'}</td>
            </tr>
          {/each}
        </table>
        Versäumte Stunden: {parseInt(hj.SumFehlStd) || 0}, davon unentschuldigt: {parseInt(hj.SumFehlStdU) || 0}
        <Voffset v="0.5"/>
        Kurse unter 5 Punkten in den Fächern und Jahrgangsstufen:
        <table>
          <tr>
            <td width="7%"><b>12.1:</b></td>
            <td width="40%">{s.abi_abschluss_faecher.filter(f => parseInt(f.P12_1) < 5 && parseInt(f.P12_1) > 0).map(f => f.fach.Bezeichnung).join(', ') || '- keine -'}</td>
            {#if hj.ASDJahrgang === "03" || hj.ASDJahrgang === "02" && hj.Abschnitt === 2}
              <td width="7%"><b>12.2:</b></td>
              <td width="40%">{s.abi_abschluss_faecher.filter(f => parseInt(f.P12_2) < 5 && parseInt(f.P12_2) > 0).map(f => f.fach.Bezeichnung).join(', ') || '- keine -'}</td>
            {/if}
          </tr>
          {#if hj.ASDJahrgang === "03"}
            <tr>
              <td><b>13.1:</b></td>
              <td width="40%">{s.abi_abschluss_faecher.filter(f => parseInt(f.P13_1) < 5 && parseInt(f.P13_1) > 0).map(f => f.fach.Bezeichnung).join(', ') || '- keine -'}</td>
              {#if hj.ASDJahrgang === "03" && hj.Abschnitt === 2}
                <td width="7%"><b>13.2:</b></td>
                <td width="40%">{s.abi_abschluss_faecher.filter(f => parseInt(f.P13_2) < 5 && parseInt(f.P13_2) > 0).map(f => f.fach.Bezeichnung).join(', ') || '- keine -'}</td>
              {/if}
            </tr>
          {/if}
        </table>
        <Voffset v="0.5"/>
        {#if s.abi_abschluss_faecher.some(f => [f.P12_1, f.P12_2, f.P13_1, f.P13_2].some(ff => parseInt(ff) < 5 && parseInt(ff) > 0))}
          Ihre Schullaufbahn weist Defizite auf, durch die die Zulassung zur
          Abiturprüfung
          {#if s.ASDSchulform === 'D01'}
            und zur Berufsabschlussprüfung
          {/if}
          gefährdet wird.
        {/if}
        <Voffset v="0.5"/>
        <b>Bemerkungen:</b> {@html bemerkungen(hj)}
      </div>
      <div class="footer">
        <div class="flex-grid">
          <div class="col-2">
            {schule.Ort}, den {datum(hj.ZeugnisDatum)}
          </div>
          <div class="col text-center klein">
            <Voffset v="3"/>
            <hr />
            {schule.SchulleiterVorname} {schule.SchulleiterName}
            <br />{schule.schulleiter_in}
          </div>
          <div class="col text-center klein">
            <Voffset v="3"/>
            Siegel
          </div>
          <div class="col text-center klein">
            <Voffset v="3"/>
            <hr />
            {hj.lehrer?.Vorname} {hj.lehrer?.Nachname}<br />{hj.klassenlehrer_in}
          </div>
        </div>
        <div class="flex-grid">
          <div class="col-2"></div>
          <div class="col-2">
            <Voffset v="1"/>
            Die Kenntnisnahme wird bestätigt:
          </div>
          <div class="col">
            <Voffset v="3"/>
            <hr />
            <div class="klein text-center">
              {volljaehrigBei(s, hj.ZeugnisDatum) ? s.schueler_in : 'Erziehungsberechtigte'}
            </div>
          </div>
        </div>
        <div class="klein eng">
          Schulnummer: {schule.SchulNr}
          <hr />
          <table>
            <tr>
              <td><sup>1</sup> </td>
              <td colspan="5">In der Reihenfolge der erlassenen Stundentafel</td>
            </tr>
            <tr>
              <td rowspan="4" valign="top"><sup>2</sup> </td>
              <td>I</td>
              <td width="70%">= sprachlich literarisches Aufgabenfeld</td>
              <td align="right" rowspan="4" valign="top"><sup>3</sup> </td>
              <td>1</td>
              <td>= erstes Leistungsfach</td>
            </tr>
            <tr>
              <td>II</td>
              <td>= gesellschaftswissenschaftliches Aufgabenfeld</td>
              <td>2</td>
              <td>= zweites Leistungsfach</td>
            </tr>
            <tr>
              <td>III</td>
              <td>= mathematisch-naturwissenschaftlich-technisches Aufgabenfeld</td>
              <td>3</td>
              <td>= drittes Abiturfach</td>
            </tr>
            <tr>
              <td colspan="2"></td>
              <td>4</td>
              <td>= viertes Abiturfach</td>
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
              <td width="1%"><sup>5</sup> </td>
              <td width="3%">E1</td>
              <td width="40%">= Mit besonderem Erfolg teilgenommen</td>
              <td width="3%">E2</td>
              <td width="30%">= Mit Erfolg teilgenommen</td>
              <td width="3%">E3</td>
              <td width="15%">= Teilgenommen</td>
            </tr>
          </table>
        </div>
      </div>
    </div>
    <div class="page grid" orientation="portrait" size="A4">
      <div class="footer grau">
        <Fussnote {schule}></Fussnote>
      </div>
    </div>
  {/each}
{/each}

<script>
  export const kommentar = `
[Anlage D 33a](https://bass.schul-welt.de/3129.htm#AnlageD33a)
[VV](https://bass.schul-welt.de/3129.htm#pgfId-3910124)
  `
  import { datum, volljaehrigBei, bemerkungen, bg }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Fussnote from './partials/Fussnoten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahr = a => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
  .table-bordered {
    border-collapse: collapse; }
  .table-bordered td {
    border: thin solid #ddd;
    line-height: 1.3rem }
  .table-eng {
    margin-bottom: 0;
  }
</style>
