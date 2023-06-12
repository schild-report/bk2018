{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4">
      <Pageheader art="daten/zeugnis.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      <div class="main">
        <div style="font-size: 13.5pt; line-height: 1.5em">
          {bg(s, 'Schulform')}
        </div>
        <div style="font-size: 12pt; line-height: 1.2em">
          {bg(s, 'Bereich')}
          {#if s.ASDSchulform.startsWith('B')}
            <br />
            Abschluss {@html s.Geschlecht === 3 ? bg(s, 'Berufsbezeichnung_m') : bg(s, 'Berufsbezeichnung_w')}
          {/if}
        </div>
        <Voffset v="1"/>
        <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},</b>
        <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},<br />
        {#if s.ASDSchulform.startsWith('E')}
          besuchte das Schuljahr {hj.schuljahr} in der Klasse {hj.Klasse}
        {:else}
          besuchte im {hj.Abschnitt}. Halbjahr des Schuljahres {hj.schuljahr} die Klasse {hj.Klasse}
        {/if}
        <Voffset v="2"/>
        Leistungen laut Konferenzbeschluss vom {datum(hj.Konferenzdatum)}:
        <Noten noten={hj.noten} fachGliederungen={s.fachklasse.fach_gliederungen}></Noten>
        <hr />
        {#if !s.ASDSchulform.startsWith('E')}
          Fehlstunden: {Number(hj.SumFehlStd) || 0}{#if Number(hj.SumFehlStd) > 0}, davon unentschuldigt: {hj.SumFehlStdU || 0}
            <Voffset v="1"/>
          {/if}
        {/if}
        <Voffset v="1"/>
        <b>{versetzungsvermerk(hj)}</b>
        <Voffset v="1"/>
        {#if s.ASDSchulform.startsWith('C') && hj.VersetzungKrz==='V'}
          Der Unterricht in der Klasse 12 kann nur aufgenommen werden, wenn
          zu Beginn des 12. Schuljahres der Nachweis über die erfolgreiche
          Ableistung des Praktikantenjahres gemäß
          Praktikum-Ausbildungsordnung, RdErl. v. 11.12.2006 (BASS 13 – 31
          Nr. 1) vorgelegt wird.
          <Voffset v="1"/>
        {/if}
        {#if s.ASDSchulform.startsWith('B')}
          {s.Geschlecht === 3 ? "Der Schüler":"Die Schülerin"} hat ein bildungsgangbegleitendes Praktikum im Umfang von 10 Wochen absolviert. Dieses wurde mit der Note {hj.noten.find(f=>f.fach.FachKrz==="PRXSOA").NotenKrz} bewertet.
          <Voffset v="1"/>
        {/if}
        <b>Bemerkungen</b><br />{@html bemerkungen(hj)}
      </div>
      <div class="footer">
        <div class="flex-grid">
          <div class="col-2">
            {schule.Ort}, den {datum(hj.ZeugnisDatum)}
          </div>
          <div class="col-2 text-center klein">
          <Voffset v="3"/>
            <hr />
            {schule.SchulleiterVorname} {schule.SchulleiterName}
            <br />{schule.schulleiter_in}
          </div>
          <div class="col text-center klein">
          <Voffset v="3"/>
            Siegel
          </div>
          <div class="col-2 text-center klein">
          <Voffset v="3"/>
            <hr />
            {hj.lehrer?.Titel ? hj.lehrer.Titel+" " : ''}{hj.lehrer?.Vorname} {hj.lehrer?.Nachname}<br />{hj.klassenlehrer_in}
          </div>
        </div>
        <div class="flex-grid">
          {#if !s.ASDSchulform.startsWith('E')}
            <div class="col-2"></div>
            <div style="flex: 3" class="text-center">
              <Voffset v="1"/>
              Die Kenntnisnahme wird bestätigt:
            </div>
            <div class="col-2">
              <Voffset v="3"/>
              <hr />
              <div class="klein text-center">
                {volljaehrigBei(s, hj.ZeugnisDatum) ? s.schueler_in : 'Erziehungsberechtigte'}
              </div>
            </div>
          {/if}
        </div>
        <Fussnote schule={schule}></Fussnote>
      </div>
    </div>
  {/each}
{/each}

<script>
  import { datum, versetzungsvermerk, volljaehrigBei, bemerkungen, bg }  from './helfer'

  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'
  import Fussnote from './partials/Fussnoten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
</style>
