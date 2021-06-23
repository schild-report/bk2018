{#each chunk(schueler, anzahl) as slice, i}
  <div class="page grid" orientation="landscape" size="A3">
    <div class="header">
      <pre>
        Konferenzliste APA BK Berufsabschluss <b>{klasse.Klasse || schueler[0].Klasse}</b> &Tab; {aktAbschnitt().klassenlehrer_in}: <b>{`${aktAbschnitt().lehrer?.Vorname} ${aktAbschnitt().lehrer?.Nachname} (${aktAbschnitt().lehrer.Kuerzel})`}</b> &Tab; Stand: {heute()}
      </pre>
    </div>
    <div class="main">
      {#each slice as s, ii}
        <b>{(i*anzahl)+ii+1}. {s.Name}, {s.Vorname}</b>
        <div class="flex-grid">
          <div class="tabelle">
            <table>
              {#each zeilen(faecher(s)) as row}
                <tr>
                  {#each row as cell}
                    <td>{cell}</td>
                  {/each}
                </tr>
              {/each}
            </table>
          </div>
          <div class="bemerkungen">
            Bermerkungen:
            {#each '12345' as i}
              <hr>
              <Voffset v=2 />
            {/each}
          </div>
        </div>
        <Voffset v=1 />
      {/each}
    </div>
    <div class="footer">Seite {i+1}/{Math.ceil(schueler.length/anzahl)}</div>
  </div>
{/each}

<script>
  import { chunk, punkte2note }  from './helfer'
  import Voffset from './partials/Voffset.svelte'
  export let schueler, klasse

  const anzahl = 5
  const heute = () => new Date().toLocaleDateString('de', {day: '2-digit', month: '2-digit', year: 'numeric'})
  const aktAbschnitt = () => schueler[0].abschnitte.slice(-1)[0]
  function faecher (s) {
    return s.abi_abschluss_faecher.filter(f => f.fach.Fachgruppe_ID !== 30).sort((a,b) => a.FSortierung - b.FSortierung)
  }
  function zeilen (faecher) {
    const arr = [[''], ['13.1'], ['13.2'], ['VN'], ['SP'], ['BA']]
    faecher.forEach(f => {
      const anzahlNoten = [f.P13_1, f.P13_2].filter(n => parseInt(n) > 0).length
      const vornote = Math.round((parseFloat(f.P13_1 || 0.0) + parseFloat(f.P13_2 || 0.0)) / anzahlNoten)
      arr[0].push(f.FachKrz)
      arr[1].push(punkte2note(f.P13_1) || '')
      arr[2].push(punkte2note(f.P13_2) || '')
      arr[3].push(parseInt(punkte2note(vornote)) || '–')
      arr[4].push('123'.includes(f.AbiFach) ? (punkte2note(parseInt(f.AbiPruefErgebnis)) || '') : '')
      if ('123'.includes(f.AbiFach) && !f.AbiPruefErgebnis) arr[5].push('…')
      else arr[5].push(parseInt(punkte2note('123'.includes(f.AbiFach) ? (Math.round((vornote + parseInt(f.AbiPruefErgebnis))/2)) : vornote)))
    })
    return arr
  }
</script>

<style>
  @import 'css/main.css';
  table {
    width: 98%;
    border-collapse: collapse;
  }
  table td {
    border: thin solid #ddd;
    white-space:nowrap;
    text-align: center;
  }
  .flex-grid {
    display: flex;
  }
  .tabelle {
    flex: 65%;
  }
  .bemerkungen {
    flex: 35%;
  }
</style>
