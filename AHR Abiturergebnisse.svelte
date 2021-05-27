{#each chunk(schueler, anzahl) as slice, i}
  <div class="page grid" orientation="landscape" size="A4">
    <div class="header">
      <pre>
        Übersicht der Abiturergebnisse <b>{klasse.Klasse || schueler[0].Klasse}</b> &Tab; Stand: {heute()}
      </pre>
    </div>
    <div class="main">
      <Voffset v=1/>
      <table class="table-bordered" width="100%">
        <tr>
          <th width="30px">Nr</th>
          <th  style="text-align: left;">Name</th>
          <th colspan="2" width="50px">1. Fach (LK1)</th>
          <th colspan="2" width="50px">2. Fach (LK2)</th>
          <th colspan="2" width="50px">3. Fach (GK)</th>
          <th colspan="2" width="50px">4. Fach (mdl)</th>
          <th width="50px">Abitur- note</th>
          <th width="70px">Summe Block I</th>
          <th width="70px">vorl. Gesamt- summe</th>
          <th width="70px">Noten- sprung</th>
          <th width="50px">Diff</th>
          <th width="70px">mdl Best.- Prfg</th>
          <th width="70px">mdl Frw.- Prfg</th>
        </tr>
        {#each slice as s, ii}
          <tr>
            <td><b>{i*anzahl+ii+1}</b></td>
            <td style="text-align: left;"><b>{s.Name}, {s.Vorname}</b></td>
            {#each s.abi_abschluss_faecher.filter(f => ['1', '2', '3', '4'].includes(f.AbiFach)).sort(f => f.AbiFach) as f}
              <td>{f.FachKrz}</td>
              <td>{f.AbiPruefErgebnis || "–"}</td>
            {/each}
              <td>{s.abi_abschluss.Note || "–"}</td>
              <td>{s.abi_abschluss.Punktsumme_I || "–"}</td>
              <td>{s.abi_abschluss.GesamtPunktzahl || "–"}</td>
              <td>{s.abi_abschluss.Notensprung || "–"}</td>
              <td>{s.abi_abschluss.Notensprung && s.abi_abschluss.GesamtPunktzahl ? (s.abi_abschluss.Notensprung-s.abi_abschluss.GesamtPunktzahl) : -''}</td>
              <td>{s.abi_abschluss_faecher.find(f=>f.MdlBestPruefung === '+') ? 'X' : ''}</td>
              <td>{s.abi_abschluss_faecher.find(f=>f.MdlFreiwPruefung === '+') ? 'X' : ''}</td>
          </tr>
        {/each}
      </table>
    </div>
    <div class="footer">Seite {i+1}/{Math.ceil(schueler.length/anzahl)}</div>
  </div>
{/each}

<script>
  import { chunk }  from './helfer'
  import Voffset from './partials/Voffset.svelte'
  export let schueler, klasse
  const heute = () => new Date().toLocaleDateString('de', {day: '2-digit', month: '2-digit', year: 'numeric'})
  const anzahl = 25
</script>

<style>
  @import 'css/main.css';
  @import 'css/a4-landscape.css';

  .table-bordered {
    border-collapse: collapse;
  }
  .table-bordered tr th {
    border: thin solid #ddd;
  }
  .table-bordered tr td {
    border: thin solid #ddd;
    line-height: 1.3rem;
    padding: 3px;
    text-align: center;
  }
</style>
