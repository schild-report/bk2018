<div class="page grid" orientation="portrait" size="A4">
  <div class="header">
    <pre>
      Durchschnittspunkte Zulassung der Abiturfächer <b>{klasse.Klasse || schueler[0].Klasse}</b> &Tab; Stand: {heute()}
    </pre>
  </div>
  <div class="main">
    <Voffset v=1/>
    <table class="table-bordered" width="100%">
      <tr>
        <th></th><th>1. LK</th><th>2. LK</th><th colspan="3">3. Abiturfach</th>
      </tr>
      <tr>
        <th>Name</th><th>{eins()}</th><th>{zwei()}</th>{#each drei() as fach}<th>{fach}</th>{/each}
      </tr>
      {#each schueler as s}
        <tr>
          <td>{s.Name}, {s.Vorname[0]}.</td>
          <td class="text-center">{fixed(s.abi_abschluss_faecher.find(f => f.AbiFach === '1').Durchschnitt)}</td>
          <td class="text-center">{fixed(s.abi_abschluss_faecher.find(f => f.AbiFach === '2').Durchschnitt)}</td>
          {#each drei() as fach}
            {#each s.abi_abschluss_faecher.filter(f => f.AbiFach === '3') as s_fach}
              <td class="text-center">{s_fach.FachKrz === fach ? fixed(s_fach.Durchschnitt) : ''}</td>
            {/each}
          {/each}
        </tr>
      {/each}
    </table>
  </div>
</div>

<script>
  export let schueler, klasse
  import Voffset from './partials/Voffset.svelte'
  const heute = () => new Date().toLocaleDateString('de', {day: '2-digit', month: '2-digit', year: 'numeric'})
  let eins, zwei, drei
  $: {
    eins = () => schueler[0].abi_abschluss_faecher.find(f => f.AbiFach === '1').FachKrz
    zwei = () => schueler[0].abi_abschluss_faecher.find(f => f.AbiFach === '2').FachKrz
    drei = () => [...new Set(schueler.map(s => s.abi_abschluss_faecher.find(f => f.AbiFach === '3').FachKrz))]
  }
  const fixed = n => n ? n.toFixed(2) : null
</script>
<style>
  @import 'css/main.css';

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
  }
</style>
