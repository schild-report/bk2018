{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4">
      <Seitenlogo logo="{privat.traegerLogo}"/>
      <Pageheader art="daten/agz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}"/>
      <div class="main">
        <div style="font-size: 15pt; line-height: 1.5em">
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
        war vom {datum(s.BeginnBildungsgang)} bis {datum(s.Entlassdatum)} {s.schueler_in}
        des Bildungsgangs {bg(s, 'Zeugniskopf')}.
        <br />{s.Geschlecht === 3 ? 'Er':'Sie'} war zuletzt {s.schueler_in} in der Klasse {s.Klasse}.
        <Voffset v="2"/>
        Leistungen laut Konferenzbeschluss vom {datum(hj.Konferenzdatum)}:
        <Noten
          noten={hj.noten}
          fachGliederungen={s.fachklasse.fach_gliederungen}
          ger
          fachklasse={s.fachklasse.Kennung}
          faechergruppenIds={[10,20,30]}
        ></Noten>
        <hr />
        {#if zeigeVersetzungsvermerk}
          <b>{versetzungsvermerk(hj)}</b>
        {/if}
        <button class="vbutton no-print" on:click="{() => zeigeVersetzungsvermerk = !zeigeVersetzungsvermerk}">
          Versetzungsvermerk {zeigeVersetzungsvermerk ? 'entfernen' : 'anzeigen'}
        </button>
        <Voffset v="1"/>
        <b>Bemerkungen</b><br />{@html bemerkungen(hj)}
      </div>
      <div class="footer">
        <Fussnote {schule} ger rechtsbehelfsbelehrung={false}></Fussnote>
      </div>
    </div>
    <div class="page grid" orientation="portrait" size="A4">
      <div class="main">
        Seite 2/2 des Abgangsszeugnisses für {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}
        <Voffset v="2"/>
        {#if zeige_FHR}
          <Voffset v="2"/>
          {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat nach § 13 a Absatz 1 Anlage D
          APO-BK mit der Versetzung in die Jahrgangsstufe 12 die
          Voraussetzungen für die Zuerkennung des schulischen Teils der
          Fachhochschulreife erfüllt. Der Abschluss ist im Deutschen und
          Europäischen Qualifikationsrahmen dem Niveau 4 zugeordnet.
          <Voffset v="2"/>
          Dieses
          Zeugnis gilt in Verbindung mit dem Nachweis einer mindestens
          zweijährigen abgeschlossenen Berufsausbildung nach Bundes- oder
          Landesrecht als Nachweis der Fachhochschulreife. Es berechtigt
          in den Ländern Nordrhein-Westfalen, Niedersachsen und
          Schleswig-Holstein zum Studium an Fachhochschulen.
          <Voffset v="2"/>
          Durchschnittsnote: {s.DurchschnittsnoteFHR} – in&nbsp;Worten: {s.DSN_FHR_Text}
        {/if}
        <Voffset v="1"/>
        <button class="vbutton no-print" on:click="{() => zeige_FHR = !zeige_FHR}">
          FHR abgeschlossen: {zeige_FHR ? 'Ja' : 'Nein'}
        </button>
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
            {hj.lehrer.Vorname} {hj.lehrer.Nachname}<br />{hj.klassenlehrer_in}
          </div>
        </div>
        <Voffset v="9"/>
        <Fussnote {schule}></Fussnote>
      </div>
    </div>
  {/each}
{/each}

<script>
  import { datum, versetzungsvermerk, bemerkungen, bg }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'
  import Fussnote from './partials/Fussnoten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  let zeigeVersetzungsvermerk = true
  const aktHalbjahr = a => a.Jahr === jahr && a.Abschnitt === abschnitt
  let zeige_FHR
</script>

<style>
  @import 'css/main.css';
  .vbutton {
    background-color: red;
    color: white;
  }
</style>
