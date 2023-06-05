{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4" style="font-size: 85%;">
      <Pageheader art="daten/agz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      <div class="main">
        <span style="font-size: 15pt; line-height: 1.5em">
          {bg(s, 'Schulform')} |
        </span>
        <span style="font-size: 12pt; line-height: 1.2em">
          {bg(s, 'Bereich')}
        </span>
        <Voffset v="1"/>
        <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},</b>
        <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},<br />
        war vom {datum(s.BeginnBildungsgang)} bis {datum(s.Entlassdatum)} {s.studierende_r}
        des Bildungsgangs {bg(s, 'Zeugniskopf')}.
        <br>{s.Geschlecht === 3 ? 'Er':'Sie'} war zuletzt {s.studierende_r} in der Klasse {s.Klasse}.
        <Voffset v="2"/>
        Leistungen laut Konferenzbeschluss vom {datum(hj.Konferenzdatum)}:
        <div class="lf-noten" style="font-size: 90%;">
          <Noten
            noten={hj.noten}
            fachGliederungen={s.fachklasse.fach_gliederungen}
            ger
            lernfeld
            fachklasse={s.fachklasse.Kennung}
            faechergruppenIds={[10,20,30]}
          ></Noten>
        </div>
        <hr />
        <Voffset v="1"/>
        {#if zeigeVersetzungsvermerk}
          <b>{versetzungsvermerk(hj)}</b>
        {:else if zeigeBP}
          <div class="text-center">
            <b>{s.Geschlecht === 3 ? "Der Studierende":"Die Studierende"} hat laut Konferenzbeschluss vom {datum(hj.Konferenzdatum)}
            <br>die Berechtigung zum Übergang in das Berufspraktikum erworben.</b>
          </div>
        {/if}
        <button class="vbutton no-print" on:click="{() => zeigeVersetzungsvermerk = !zeigeVersetzungsvermerk}">
          Versetzungsvermerk {zeigeVersetzungsvermerk ? 'entfernen' : 'anzeigen'}
        </button>
        <button class="vbutton no-print" on:click="{() => zeigeBP = !zeigeBP}">
          Übergang BP {zeigeBP ? 'entfernen' : 'anzeigen'}
        </button>
        <Voffset v="1"/>
        <b>Bemerkungen</b><br />{@html bemerkungen(hj)}
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
            {hj.lehrer?.Titel ? hj.lehrer.Titel+" " : ''}{hj.lehrer?.Vorname} {hj.lehrer?.Nachname}<br />{hj.klassenlehrer_in}
          </div>
        </div>
        <div class="klein eng">
          <Voffset v="2"/>
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
        <Fussnote notenstufen={false} {schule}/>
      </div>
    </div>
  {/each}
{/each}

<script>
  import { datum, versetzungsvermerk, bemerkungen, bg }  from './helfer'

  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'
  import Fussnote from './partials/Fussnoten.svelte'
  export const kommentar = `
[Vorlage PDF](https://bass.schul-welt.de/anlagen/3129-61.pdf)

[VV dazu 15.2](https://bass.schul-welt.de/3129.htm#pgfId-3914969)
  `

  export let schueler, schule, privat, jahr, abschnitt
  let zeigeVersetzungsvermerk = true
  let zeigeBP = false
  const aktHalbjahr = a => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
  .lf-noten {
    font-size: 0.9rem;
  }
  .vbutton {
    background-color: red;
    color: white;
  }
</style>
