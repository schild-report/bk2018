{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4" style="font-size: .84rem">
      <Pageheader art="daten/zeugnis.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      <div class="main">
        <span style="font-size: 15pt; line-height: 1.5em">
          {bg(s, 'Schulform')} |
        </span>
        <span style="font-size: 12pt; line-height: 1.2em">
          {bg(s, 'Bereich')}
        </span>
        <Voffset v=".5"/>
        <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},</b>
        <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort}, besuchte im Schuljahr {hj.schuljahr} die Klasse {hj.Klasse}
        <Voffset v="1"/>
        Leistungen laut Konferenzbeschluss vom {datum(hj.Konferenzdatum)}:
        <div class="lf-noten" style="font-size: 0.84rem">
          <Noten
            noten={hj.noten}
            fachGliederungen={s.fachklasse.fach_gliederungen}
            lernfeld
            fachklasse={s.fachklasse.Kennung}
            faechergruppenIds={[10,20,30]}
          ></Noten>
        </div>
        <hr />
        <Voffset v=".5"/>
        <div class="text-center">
          <b>{s.Geschlecht === 3 ? "Der Studierende":"Die Studierende"} hat laut Konferenzbeschluss vom {datum(hj.Konferenzdatum)}
          <br>die Berechtigung zum Übergang in das Berufspraktikum erworben.</b>
        </div>
        <Voffset v=".5"/>
        <b>Bemerkungen</b><br />{@html bemerkungen(hj)}
      </div>
      <div class="footer">
        <div class="flex-grid">
          <div class="col-2">
            {schule.Ort}, den {datum(hj.ZeugnisDatum)}
          </div>
          <div class="col text-center klein">
          <Voffset v="1"/>
            <hr />
            {schule.SchulleiterVorname} {schule.SchulleiterName}
            <br />{schule.schulleiter_in}
          </div>
          <div class="col text-center klein">
          <Voffset v="1"/>
            Siegel
          </div>
          <div class="col text-center klein">
          <Voffset v="1"/>
            <hr />
            {hj.lehrer?.Titel ? hj.lehrer.Titel+" " : ''}{hj.lehrer?.Vorname} {hj.lehrer?.Nachname}<br />{hj.klassenlehrer_in}
          </div>
        </div>
        <div class="klein eng">
          <Voffset v="1"/>
          <div class="schulnummer text-center">
            Schulnummer: {schule.SchulNr}
          </div>
          <hr class="hr-grau" />
          <div class="grau text-center">
              Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend,
              4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
          </div>
        </div>
      </div>
    </div>
    <div class="page grid" orientation="portrait" size="A4">
      <div class="footer grau">
        <Fussnote notenstufen={false} {schule} verordnung={true}></Fussnote>
      </div>
    </div>
  {/each}
{/each}

<script>
  import { datum, bemerkungen, bg }  from './helfer'

  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'
  import Fussnote from './partials/Fussnoten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahr = a => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
  .lf-noten {
    font-size: 0.9rem;
  }
</style>
