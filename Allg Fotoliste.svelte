{#each chunk(schueler, 25) as slice}
  <div class="page grid" orientation="portrait" size="A4">
    <div class="main">
      <b>{slice[0].Klasse}</b>
      Stand: {datum(new Date())}
      <div class="grid" style="font-size: 0.8rem">
        {#each slice as s}
          <div>
            {#await foto(s) then f}
              <img src="data:image/jpg;base64,{f}" alt="Foto" style="width: 120px">
            {:catch}
              Kein Foto
            {/await}
            <br>
            {s.Name}, {s.Vorname}
          </div>
        {/each}
      </div>
    </div>
  </div>
{/each}

<script>
  import { datum, chunk } from './helfer'
  export let schueler, knexConfig
  const mysql = R('mysql')
  var connection = mysql.createConnection(knexConfig.connection)
  connection.connect()
  async function foto (s) {
    return new Promise((resolve, reject) => {
      connection.query('SELECT Foto FROM `schuelerfotos` WHERE `Schueler_ID` = ?', [s.ID],
        (err, rows) => {
          if (err || rows.length===0) return reject(err);
          resolve(Buffer.from(rows[0].Foto, 'binary').toString('base64'));
      });
    });
  }
</script>

<style>
  @import 'css/main.css';
  .grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 10px;
  }
</style>
