{#each schueler as s (s.ID)}
  {#each aktHalbjahrFuer(s) as hj (hj.ID)}
    <div class="page grid" orientation="landscape" size="A3">
      <Seitenlogo logo="{privat.traegerLogo}"/>
      <Pageheader art="daten/asz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}"/>
      <div class="main">
        <div class="main-grid">
          <Voffset v="1"/>
          <div class="main-left">
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
            <b>Bemerkungen</b>
            <br />{@html bemerkungen(hj)}
          </div>
          <div class="main-right">
            <Voffset v="2"/>
            <table class="table-noten">
              {#each s.bk_abschluss_faecher.filter(f => f.fach.FachKrz.startsWith('AS')) as as}
                <tr>
                  <td width="22%">{as.fach.Zeugnisbez}</td>
                  <td class="td-padding-extra">{findeFach(hj, as).Lernentw}</td>
                  <td class="td-fach-note"><span>{note(as.NoteAbschluss)}</span></td>
                </tr>
              {/each}
            </table>
            <Voffset v="4"/>
            {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat die Berufsabschlussprüfung bestanden und ist berechtigt, die Berufsbezeichnung
            <h6 class="text-center">{@html bg(s, (s.Geschlecht === 3 ? 'Berufsbezeichnung_m' : 'Berufsbezeichnung_w'))}</h6>
            zu führen.
            <Voffset v="1"/>
            Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen dem Niveau {s.fachklasse.DQR_Niveau} zugeordnet.
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
            <Voffset v="6"/>
            {schule.Ort}, den {datum(hj.ZeugnisDatum)}
            <Voffset v="6"/>
            <div class="flex-grid">
              <div class="col">
                <hr />
                <div class="text-center klein">
                  {schule.SchulleiterVorname} {schule.SchulleiterName}
                  <br>Vorsitzende{schule.SchulleiterGeschlecht === 3 ? 'r':''} des allgemeinen Prüfungsausschusses
                </div>
              </div>
              <div class="col klein">
                <div class="text-center klein">
                  Siegel
                </div>
              </div>
              <div class="col klein">
                <hr />
                <div class="text-center">
                  {schule.SchulleiterVorname} {schule.SchulleiterName}
                  <br />{schule.schulleiter_in}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="footer klein grau">
        <div class="schulnummer">
          Schulnummer: {schule.SchulNr}
        </div>
        <hr class="hr-grau" />
        <Voffset v="1"/>
        <div class="footer-grid">
          <div class="footer-left">
            Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend, 4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
            <p></p>
            <sup>1</sup>Der Unterricht in den modernen Fremdsprachen hat auf der
            nach dem Fach in Klammern angegebenen Niveaustufe des
            „Europäischen Referenzrahmens für Sprachen: Lernen, Lehren,
            Beurteilen“ stattgefunden. Sind zwei Referenzniveaus ausgewiesen, ist
            das niedrigere in vollem Umfang, das höhere in Anteilen erreicht. Bei
            mindestens ausreichenden Leistungen wird der sprachliche
            Kompetenzerwerb auf diesem Niveau bescheinigt.
            <Voffset v="1"/><sup>2</sup>Die Verkürzung des Praktikums von 16 auf 10 Wochen erfolgte im Kontext der Corona-Pandemie (vgl. Schulmail des Ministeriums für Schule und Bildung NRW v. 18.03.2020).
          </div>
          <div class="footer-right">
            Dem Zeugnis liegen zugrunde:
            <ul class="list-unstyled dashes">
              <li>
                Die Verordnung über die Ausbildung und Prüfung in den Bildungsgängen des Berufskollegs (Ausbildungs- und Prüfungsordnung Berufskolleg, APO-BK) vom 26. Mai 1999 (SGV. NRW. 223/BASS 13 – 33 Nr. 1.1).
              </li>
              <li>
                Die Vereinbarung über Fachschulen (Beschluss der Kultusministerkonferenz vom 07. November 2002 in der jeweils geltenden Fassung).
              </li>
            </ul>
            <br>
            Rechtsbehelfsbelehrung:
            Gegen dieses Zeugnis kann innerhalb eines Monats
            nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
            ist beim {schule.Bezeichnung2}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
            schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
            das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dies
            Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
          </div>
        </div>
      </div>
    </div>
  {/each}
{/each}

<script>
  export const kommentar = `
[Anlage B6](https://bass.schul-welt.de/3129.htm#AnlageB6)
[VV](https://bass.schul-welt.de/3129.htm#pgfId-3908229)
**Praktikumsdauer Corona-bedingt beachten!**
  `
  import { datum, bemerkungen, bg, note }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'

  export let schueler, schule, privat, jahr, abschnitt
  const aktHalbjahrFuer = (s) => s.abschnitte.filter(a => a.Jahr === jahr && a.Abschnitt === abschnitt)
  const findeFach = (hj, as) => hj.noten.find(n => n.fach.ID === as.fach.ID) || { Lernentw: 'undefined' }
</script>

<style>
  @import 'css/main.css';
  .td-padding-extra {
    padding: 2mm 0 2mm 0;
  }
  .td-fach-note {
    padding-bottom: .2rem;
    padding-top: .2rem;
    width: 12rem;
  }
  .td-fach-note span {
    background-color: #dcdcdc !important;
    text-align: center;
    width: 10rem;
    display: block;
    margin: 0 auto;
  }
</style>

