<script>
  import { datum, chunk } from "./helfer";

  import Voffset from "./partials/Voffset.svelte";

  export let schueler, jahr, abschnitt, klasse;
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt;
  const heute = () =>
    new Date().toLocaleDateString("de", {
      day: "2-digit",
      month: "2-digit",
      year: "numeric",
    });
  const anzahl = 25;
  const aktAbschnitt = () => schueler[0].abschnitte.slice(-1)[0];
  const faecher = new Set();
  schueler.forEach((s) =>
    s.abschnitte.find(aktHalbjahr).noten.forEach((n) => {
      faecher.add(n.fach.FachKrz);
    })
  );
</script>

{#each chunk(schueler, anzahl) as slice, i}
  <div
    class="page grid"
    orientation="landscape"
    size="A3"
    style="font-size: 1.0rem"
  >
    <div class="header">
      <pre>
        Konferenzliste <b>{klasse.Klasse || schueler[0].Klasse}</b
        > &Tab; {aktAbschnitt().klassenlehrer_in}: <b
          >{`${aktAbschnitt().lehrer?.Vorname} ${
            aktAbschnitt().lehrer?.Nachname
          } (${aktAbschnitt().lehrer?.Kuerzel})`}</b
        > &Tab; Stand: {heute()} &Tab; Konferenzdatum: {datum(
          aktAbschnitt().Konferenzdatum
        )} &Tab; Zeugnisdatum: {datum(aktAbschnitt().ZeugnisDatum)}
      </pre>
    </div>
    <div class="main">
      <Voffset v="1" />
      <table>
        <tr>
          <th><b>Nr</b></th>
          <th><b>Name</b></th>
          <th class="rotate"><div><span><b>FStd</b></span></div></th>
          <th class="rotate"><div><span><b>uFStd</b></span></div></th>
          {#each Array.from(faecher) as f}
            <th class="rotate"><div><span><b>{f}</b></span></div></th>
          {/each}
          <th><b>Bemerkungen</b></th>
        </tr>
        <tr
          ><td colspan="2" style="height: 4rem">Bitte abzeichnen</td
          >{#each Array.from(faecher) as f}<td />{/each}<td /><td /><td /></tr
        >
        {#each slice as s, ii}
          {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
            <tr>
              <td><b>{i * anzahl + ii + 1}</b></td>
              <td><b>{s.Name}, {s.Vorname}</b></td>
              <td>{hj.SumFehlStd || 0}</td>
              <td>{hj.SumFehlStdU || 0}</td>
              {#each Array.from(faecher) as f}
                {#each [hj.noten.find((n) => n.fach.FachKrz === f)] as n}
                  {#if n}
                    <td
                      class="text-center"
                      style={parseInt(n.NotenKrz) > 4
                        ? "background:tomato;"
                        : ""}
                    >
                      <span class:kreis={n.Warnung === "+"}
                        >{n.NotenKrz || "–"}</span
                      >
                    </td>
                  {:else}<td style="background: lightgrey" />
                  {/if}
                {/each}
              {/each}
              <td class="extraklein">{hj.ZeugnisBem || ""}</td>
            </tr>
          {/each}
        {/each}
      </table>
    </div>
    <div class="footer">
      Seite {i + 1}/{Math.ceil(schueler.length / anzahl)}
    </div>
  </div>
{/each}

<style>
  @import "css/main.css";
  table {
    border-collapse: collapse;
    width: auto;
  }
  table td {
    border: thin solid #ddd;
    line-height: 1.3rem;
    padding: 9px;
    white-space: nowrap;
  }
  table td.extraklein {
    font-size: 0.6rem;
    white-space: pre-wrap;
    word-wrap: break-word;
    vertical-align: top;
    padding: 1px;
  }
  table td:last-child {
    width: 100%;
  }
  th.rotate {
    height: 30px;
    white-space: nowrap;
  }
  th.rotate > div {
    transform: translate(20px, -3px) rotate(315deg);
    box-sizing: border-box;
  }
  th.rotate > div > span {
    border-bottom: 1px solid #ccc;
    width: 30px;
    transform-origin: left bottom;
    display: inline-block;
  }
  .kreis {
    border: 0.2rem solid red;
    border-radius: 20%;
    padding: 0.2rem;
  }
</style>
