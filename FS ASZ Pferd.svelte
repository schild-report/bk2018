{#each schueler as s (s.ID)}
  {#each aktHalbjahrFuer(s) as hj (hj.ID)}
    <div class="page grid" orientation="landscape" size="A3" style="font-size: 1.1rem">
      <div class="header">
        <Seitenlogo logo="{privat.traegerLogo}"/>
        <Pageheader art="daten/asz.svg" logo="{privat.logo}" untertitel="{privat.untertitel}"/>
      </div>
      <div class="main-grid">
        <div class="main-left">
          <Voffset v="2"/>
          <b>{s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name},</b>
          <br />geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
          <br />war vom {datum(s.Aufnahmedatum)} bis zur Aushändigung des Zeugnisses {s.studierende_r} des Bildungsgangs
          <br /><b>{@html bg(s, 'Zeugniskopf')}</b>
          <br>in Kooperation mit dem Deutschen Kuratorium für Therapeutisches Reiten e.V. (DKThR).
          <Voffset v="1"/>
          Der allgemeine Prüfungsausschuss stellte in seiner Abschlusskonferenz am {datum(hj.Konferenzdatum)} fest:
          <Voffset v=".5"/>
          <b>Leistungen</b>
          <Noten
            noten={hj.noten}
            faechergruppenIds={[10, 20, 30]}
            fachGliederungen={s.fachklasse.fach_gliederungen}
            fachklasse={s.fachklasse.Kennung}
          ></Noten>
          <hr />
          <b>Bemerkungen</b>
          <br />{@html bemerkungen(hj)}
        </div>
        <div class="main-right">
          <Voffset v="2"/>
          {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} erbrachte in der Abschlussprüfung folgende Leistung:
          <Voffset v="2"/>
          <table class="table-noten" width="100%">
            {#each hj.noten.filter(f => f.fach.FachKrz === "PROJ-PF") as f}
              <tr>
                <td width="22%">{f.fach.Zeugnisbez}</td>
                <td class="td-padding-extra">{f.Lernentw}</td>
                <td class="td-fach-note"><span>{note(f.NotenKrz)}</span></td>
              </tr>
            {/each}
          </table>
          <Voffset v="1"/>
          <Voffset v="4"/>
          {s.anrede} {s.Vorname} {s.Zusatz || ''} {s.Name} ist berechtigt, die Berufsbezeichnung
          <h4 class="text-center">{@html s.berufsbezeichnung_mw}
          <br><div style="font-size: 80%">(Bachelor Professional im Sozialwesen)</div></h4>
          zu führen.
          <Voffset v="1"/>
          Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen dem Niveau {s.fachklasse.DQR_Niveau} zugeordnet.
          <Voffset v="9"/>
          <div class="flex-grid">
            <Voffset v="6"/>
            <div class="col">
              {schule.Ort}, den {datum(hj.ZeugnisDatum)}
              <Voffset v="6"/>
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
                {schule.SchulleiterVorname} {schule.SchulleiterName} <br />{schule.schulleiter_in}
              </div>
              <Voffset v="6"/>
              <hr />
              <div class="text-center klein">
                Weiterbildung DKThR
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="footer-grid klein eng">
        <div class="header">
          Schulnummer: {schule.SchulNr}
          <hr />
        </div>
        <div class="footer-left">
          Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend, 4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
          <p></p>
          Dem Zeugnis liegen zugrunde:
          <ul class="list-unstyled dashes">
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
        </div>
        <div class="footer-right">
          Rechtsbehelfsbelehrung:<br>
          Gegen dieses Zeugnis kann innerhalb eines Monats
          nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
          ist beim {schule.Bezeichnung2}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
          schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
          das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dies
          Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
        </div>
      </div>
    </div>
  {/each}
{/each}

<script>
export const kommentar = `
[E5 PDF](https://bass.schul-welt.de/anlagen/3129-60.pdf)
[E5 html]()
`
  import { datum, bemerkungen, bg, note }  from './helfer'

  import Seitenlogo from './partials/Seitenlogo.svelte'
  import Pageheader from './partials/Pageheader.svelte'
  import Voffset from './partials/Voffset.svelte'
  import Noten from './partials/Noten.svelte'

  export let schueler, schule, privat, jahr, abschnitt

  const aktHalbjahrFuer = (s) => s.abschnitte.filter(a => a.Jahr === jahr && a.Abschnitt === abschnitt)
</script>

<style>
  @import 'css/main.css';
  @import 'css/a3-landscape.css';
  .table-noten {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 1rem;
  }
  .td-fach-note {
    padding-bottom: .2rem;
    padding-top: .2rem;
    width: 12rem;
  }
  .td-fach-note span {
    background-color: #dcdcdc;
    text-align: center;
    width: 10rem;
    display: block;
    margin: 0 auto;
  }
</style>

