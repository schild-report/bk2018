{#each schueler as s (s.ID)}
  {#each aktHalbjahrFuer(s) as hj (hj.ID)}
    <div class="page grid" orientation="landscape" size="A3" style="font-size: .9rem;">
      <div class="header">
        <Pageheader art="daten/agz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
      </div>
      <div class="main-grid">
        <div class="main-left">
          <Voffset v="2"/>
          <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},</b>
          <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
          <br />war vom {datum(s.Aufnahmedatum)} bis zur Aushändigung des
          Zeugnisses {s.schueler_in} des Bildungsgangs <br>
          <b>Erzieherin/Erzieher mit Allgemeiner Hochschulreife</b> im
          Fachbereich Gesundheit und Soziales mit dem fachlichen Schwerpunkt
          Pädagogik.
          <Voffset v="1"/>
          In der Konferenz am {datum(hj.Konferenzdatum)} sind folgende Leistungen festgestellt worden:
          <Voffset v=".5"/>
          <Noten
            ger
            noten={s.bk_abschluss_faecher}
            faechergruppenIds={[10, 20, 30, 40]}
            fachGliederungen={s.fachklasse.fach_gliederungen}
            fachklasse={s.fachklasse.Kennung}
            gruppenbezeichnungNeu={gruppenbezeichnungNeu}
          ></Noten>
        </div>
        <div class="main-right">
          <Voffset v="2"/>
          {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} hat hat die staatliche Berufsabschlussprüfung für Erzieherinnen/Erzieher bestanden.
          <Voffset v="2"/>
          <b>Bemerkungen</b>
          <br />{@html bemerkungen(hj)}
          <Voffset v="9"/>
          <div class="flex-grid">
            <div class="col">
              <Voffset v="-1"/>
              {schule.Ort}, den {datum(hj.ZeugnisDatum)}
              <Voffset v="7"/>
              <div class="text-center klein">Siegel</div>
            </div>
            <div class="col">
              <hr />
              <div class="text-center klein">
                {schule.SchulleiterVorname} {schule.SchulleiterName}
                <br>Vorsitzende{schule.SchulleiterGeschlecht === 3 ? 'r':''} des allgemeinen Prüfungsausschusses
              </div>
              <Voffset v="6"/>
              <hr />
              <div class="text-center klein">
                {schule.SchulleiterVorname} {schule.SchulleiterName}
                <br />{schule.schulleiter_in}
              </div>
            </div>
            <div class="col"></div>
          </div>
        </div>
      </div>
      <div class="footer-grid klein eng">
        <div class="header">
          Schulnummer: {schule.SchulNr}
          <hr />
          <Voffset v="1"/>
        </div>
        <div class="footer-left">
          Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend, 4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
          <Voffset v=".5"/>
          <sup>1</sup>Der Unterricht in den modernen Fremdsprachen hat auf der nach dem Fach in
          Klammern angegebenen Niveaustufe des „Europäischen Referenzrahmens für
          Sprachen: Lernen, Lehren, Beurteilen“ stattgefunden. Sind zwei
          Referenzniveaus ausgewiesen, ist das niedrigere in vollem Umfang, das höhere
          in Anteilen erreicht. Bei mindestens ausreichenden Leistungen wird der
          sprachliche Kompetenzerwerb auf diesem Niveau bescheinigt.
        </div>
        <Voffset v=".5"/>
        <div class="footer-right">
          Dem Zeugnis liegen zugrunde:
          <ul class="list-unstyled dashes">
            <li>
              Die Vereinbarung zur Gestaltung der gymnasialen Oberstufe und der
              Abiturprüfung (Beschluss der Kultusministerkonferenz vom 7. Juli 1972 in der jeweils geltenden Fassung).
            </li>
            <li>
              Die Verordnung über die Ausbildung und Prüfung in den Bildungsgängen
              des Berufskollegs (Ausbildungs- und Prüfungsordnung Berufskolleg,
              APO-BK) vom 26. Mai 1999 (SGV. NRW. 223/BASS 13 – 33 Nr. 1.1).
            </li>
            <li>
              Die Vereinbarung über Fachschulen (Beschluss der Kultusministerkonferenz
              vom 07. November 2002 in der jeweils geltenden Fassung).
            </li>
          </ul>
          <Voffset v=".5"/>
          Rechtsbehelfsbelehrung:
          <br>Gegen dieses Zeugnis kann innerhalb eines Monats
          nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
          ist beim {schule.Bezeichnung1}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
          schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
          das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dieses
          Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
        </div>
      </div>
    </div>
  {/each}
{/each}

<script>
  export const kommentar = `
[Anlage D 53](https://bass.schul-welt.de/3129.htm#13-33nr11A_D53)
`
  import { datum, bemerkungen }  from './helfer'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'
  export let schueler, schule, privat, jahr, abschnitt
  let gruppenbezeichnungNeu = {1600: 'Fachpraktische Prüfung'}
  const aktHalbjahrFuer = s => s.abschnitte.filter(a => a.Jahr === jahr && a.Abschnitt === abschnitt)
</script>

<style>
  @import 'css/main.css';
  @page {
    margin: 0;
    size: A3 landscape;
  }
</style>