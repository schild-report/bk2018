{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="portrait" size="A4">
      <Seitenlogo logo="{privat.traegerLogo}"/>
      <Pageheader art="daten/asz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}"/>
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
        <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
        <br />war vom {datum(s.Aufnahmedatum)} bis zur Aushändigung des Zeugnisses {s.schueler_in} des Bildungsgangs
        <br /><b>{bg(s, 'Zeugniskopf')}.</b>
        <Voffset v=".5"/>
        Der allgemeine Prüfungsausschuss stellte in seiner Abschlusskonferenz am {datum(hj.Konferenzdatum)} fest:
        <Voffset v=".5"/>
        <b>Leistungen</b>
        <div style="font-size: 90%">
          <Noten
            noten={hj.noten}
            faechergruppenIds={[10, 20, 30]}
            fachGliederungen={s.fachklasse.fach_gliederungen}
            ger
            fachklasse={s.fachklasse.Kennung}
          ></Noten>
        </div>
        <hr />
        {#if s.fachklasse.Kennung === '93-103-00'}
          <Voffset v="1"/>
          {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat ein
          bildungsgangbegleitendes Praktikum in Einrichtungen für Kinder
          und Jugendliche sowie alte, kranke und behinderte Menschen im
          Umfang von 10 Wochen absolviert.<sup>2</sup>
        {/if}
        <Voffset v="1"/>
          {#if s.Entlassart!=='5A'}
            <div class="text-center">
              <Voffset v="2"/>
              {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat den
              <Voffset v="0.5"/>
              <b>mittleren Schulabschluss (Fachoberschulreife)</b>
              {#if s.Entlassart==='5G'}
                <br><b>mit Berechtigung zum Besuch der gymnasialen Oberstufe</b>
              {/if}
              <Voffset v="0.5"/>
              erworben.
            </div>
          {:else}<Voffset v="3"/>
          {/if}
          <Voffset v="1"/>
          Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen dem Niveau 3 zugeordnet.
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
            {schule.SchulleiterVorname} {schule.SchulleiterName} <br />{schule.schulleiter_in}
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
        <Fussnote {schule} ger rechtsbehelfsbelehrung={false} verordnung={false}>
          {#if s.fachklasse.Kennung === '93-103-00'}
            <Voffset v=".5"/><sup>2</sup>Die Verkürzung des Praktikums von 16 auf 10 Wochen erfolgte im Kontext der Corona-Pandemie (vgl. Schulmail des Ministeriums für Schule und Bildung NRW v. 18.03.2020).
          {/if}
        </Fussnote>
      </div>
    </div>
    <div class="page grid" orientation="portrait" size="A4">
      <div class="main">
        Seite 2/2 des Abgangsszeugnisses für {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name}
        <Voffset v="2"/>
        <SoaTabelle></SoaTabelle>
      </div>
      <div class="footer">
        <Fussnote {schule} bfs notenstufen={false}></Fussnote>
      </div>
    </div>
  {/each}
{/each}


<script>
  import { datum, bemerkungen, bg }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import SoaTabelle from './partials/SOA-Tabelle.svelte'
  import Noten from './partials/Noten.svelte'
  import Fussnote from './partials/Fussnoten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahr = a => a.Jahr === jahr && a.Abschnitt === abschnitt
</script>

<style>
  @import 'css/main.css';
</style>
