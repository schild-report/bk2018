{#each chunk(schueler, anzahl) as slice, i}
  <div class="page grid" orientation="landscape" size="A3">
    <div class="header">
      <pre>
        Konferenzliste APA BK Abitur <b>{klasse.Klasse || schueler[0].Klasse}</b> &Tab; {aktAbschnitt().klassenlehrer_in}: <b>{`${aktAbschnitt().lehrer?.Vorname} ${aktAbschnitt().lehrer?.Nachname} (${aktAbschnitt().lehrer?.Kuerzel})`}</b> &Tab; Stand: {heute()}
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
                    <td class:red={parseInt(cell) < 5 && parseInt(cell) >= 0}>{cell}</td>
                  {/each}
                </tr>
              {/each}
              <tr><td>✎ → </td>
              {#each s.abi_abschluss_faecher as i}
              <td></td>
              {/each}
              </tr>
            </table>
          </div>
          <div class="bemerkungen">
            Bermerkungen:
            {#each '12345' as i}
              <hr>
              <Voffset v='1.5' />
            {/each}
            Zugelassen: {s.abi_abschluss.Zugelassen === '+' ? "Ja":"Nein"}
          </div>
        </div>
        <Voffset v=1 />
      {/each}
    </div>
    <div class="footer">Seite {i+1}/{Math.ceil(schueler.length/anzahl)}</div>
  </div>
{/each}

<script>
  import { chunk }  from './helfer'
  import Voffset from './partials/Voffset.svelte'
  export let schueler, klasse

  const anzahl = 5
  const heute = () => new Date().toLocaleDateString('de', {day: '2-digit', month: '2-digit', year: 'numeric'})
  const aktAbschnitt = () => schueler[0].abschnitte.slice(-1)[0]
  function faecher (s) {
    return s.abi_abschluss_faecher.sort((a,b) => a.FSortierung - b.FSortierung)
  }
  function zeilen (faecher) {
    const arr = [[''], ['12.1'], ['12.2'], ['13.1'], ['13.2']]
    faecher.forEach(f => {
      arr[0].push(f.FachKrz)
      arr[1].push(f.P12_1 || '')
      arr[2].push(f.P12_2 || '')
      arr[3].push(f.P13_1 || '')
      arr[4].push(f.P13_2 || '')
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
  .red { background-color: tomato; }
</style>
