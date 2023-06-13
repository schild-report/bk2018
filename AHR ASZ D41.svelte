<script>
  export const kommentar = `
[Anlage D 41](https://bass.schul-welt.de/3129.htm#13-33nr11A_D41)
[§24](https://bass.schul-welt.de/3129.htm#pgfId-1290315)
Der Name der Bildungsgangleitung wird momentan über die Eingabe in der
Fachklasse Ebene2 eingegeben.
`;
  import { datum, bemerkungen, bg, noteInWorten } from "./helfer";

  import Pageheader from "./partials/Pageheader.svelte";
  import Voffset from "./partials/Voffset.svelte";

  export let schueler, schule, privat, jahr, abschnitt;
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt;
</script>

{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div class="page grid" orientation="landscape" size="A3">
      <div class="main-grid">
        <div class="main-left grid">
          <div class="main">
            <Voffset v="2" />
            4. Seite des Zeugnisses der Allgemeinen Hochschulreife für {s.Vorname}
            {s.Zusatz || ""}
            {s.Name}
            <Voffset v="2" />
            {#if s.abi_abschluss.KlLatinum === "+" || s.abi_abschluss.Latinum === "+"}
              Dieses Zeugnis schließt das Latinum (Nachweis von
              Lateinkenntnissen gemäß Vereinbarung der Kultusministerkonferenz
              vom 22. September 2005) ein.
              <Voffset v="2" />
            {:else if s.abi_abschluss.Graecum === "+"}
              <Voffset v="2" />
              Dieses Zeugnis schließt das Graecum (Nachweis von Griechischkenntnissen
              gemäß Vereinbarung der Kultusministerkonferenz vom 22. September 2005)
              ein.
            {:else if s.abi_abschluss.Hebraicum === "+"}
              Dieses Zeugnis schließt das Hebraicum ein.
              <Voffset v="2" />
            {/if}
            <b>Bemerkungen</b>
            <br />{@html bemerkungen(hj)}
            <Voffset v="2" />
            {s.anrede}
            {s.Vorname}
            {s.Zusatz || ""}
            {s.Name} hat die Abiturprüfung bestanden und damit die Berechtigung zum
            Studium an einer Hochschule in der Bundesrepublik Deutschland erworben.
            Der Abschluss ist im Deutschen und Europäischen Qualifikationsrahmen
            dem Niveau 4 zugeordnet.
          </div>
          <div class="footer">
            <div class="flex-grid">
              <div class="col-2">
                <Voffset v="1" />
                {schule.Ort}, den {datum(hj.ZeugnisDatum)}
              </div>
              <div class="col" />
              <div class="col-2 text-center klein">
                <Voffset v="3" />
                <hr />
                {s.fachklasse.Ebene2Klartext}
                <br />Bildungsgangleitung
              </div>
            </div>
            <Voffset v="2" />
            <div class="flex-grid">
              <div class="col text-center klein">Siegel</div>
            </div>
            <Voffset v="2" />
            <div class="flex-grid">
              <div class="col-2 text-center klein">
                <Voffset v="3" />
                <hr />
                {schule.SchulleiterVorname} {schule.SchulleiterName}
                <br>Vorsitzende{schule.SchulleiterGeschlecht === 3 ? 'r':''} des allgemeinen Prüfungsausschusses
              </div>
              <div class="col" />
              <div class="col-2 text-center klein">
                <Voffset v="3" />
                <hr />
                {schule.SchulleiterVorname}
                {schule.SchulleiterName} <br />{schule.schulleiter_in}
              </div>
            </div>
            <Voffset v="2" />
            <div class="klein eng">
              <b>Rechtsbehelfsbelehrung:</b>
              <Voffset v="0.5" />
              Gegen die Kursabschlussnoten der Jahrgangsstufe 13.2 und die Leistungen
              in der Abiturprüfung sowie die Berechnung der Gesamtqualifikation einschließlich
              der Durchschnittsnote kann innerhalb eines Monats nach Bekanntgabe
              des Zeugnisses Widerspruch erhoben werden. Der Widerspruch ist beim
              {schule.Bezeichnung1}, {schule.Strasse}, {schule.PLZ}
              {schule.Ort}, schriftlich oder zur Niederschrift zu erheben. Falls
              die Frist durch das Verschulden einer/eines Bevollmächtigten
              versäumt wird, wird dieses Verschulden der
              Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
              <Voffset v="2" />
              <div class="schulnummer">
                Schulnummer: {schule.SchulNr}
              </div>
            </div>
          </div>
        </div>
        <div class="main-right grid">
          <div class="header">
            <Pageheader logo="{privat.logo}" untertitel="{privat.untertitel}" traeger="{privat.traeger}"/>
          </div>
          <div class="main">
            <Voffset v="3" />
            <h4 class="text-center">
              Zeugnis<br />der Allgemeinen Hochschulreife
            </h4>
            <Voffset v="10" />
            <div class="text-center" style="font-size: 1.3rem">
              <b>{s.anrede} {s.Vorname} {s.Zusatz || ""} {s.Name},</b>
              <div class="voffset-2" />
              <div>
                geboren am {datum(s.Geburtsdatum)} in {s.Geburtsort},
              </div>
              <div>
                wohnhaft in {s.OrtAbk}, {s.KonfDruck === '+' ? `${s.ReligionAbk},`:''}
              </div>
              <div>hat sich der Abiturprüfung im Bildungsgang</div>
              <div>{bg(s, "Bereich")}</div>
              <div>unterzogen.</div>
            </div>
          </div>
          <div class="footer klein eng">
            Dem Zeugnis liegen zugrunde:
            <ul class="list-unstyled dashes">
              <li>
                die Vereinbarung zur Gestaltung der gymnasialen Oberstufe und
                der Abiturprüfung (Beschluss der Kultusministerkonferenz vom 7.
                Juli 1972 in der jeweils geltenden Fassung)
              </li>
              <li>
                die Bildungsstandards für die Allgemeine Hochschulreife oder die
                Vereinbarung über die einheitlichen Prüfungsanforderungen in der
                Abiturprüfung (Beschlüsse der Kultusministerkonferenz in der
                jeweils geltenden Fassung)
              </li>
              <li>
                die Verordnung über die Ausbildung und Prüfung in den
                Bildungsgängen des Berufskollegs (Ausbildungs- und
                Prüfungsordnung Berufskolleg – APO-BK) vom 26. Mai 1999 (SGV.
                NRW. 223/BASS 13–33 Nr. 1.1)
              </li>
              <li>
                Schulversuch des Landes Nordrhein-Westfalen gemäß der
                Vereinbarung zur Durchführung von Schulversuchen und der
                gegenseitigen Anerkennung der entsprechenden Abschlüsse
                (Beschluss der Kultusministerkonferenz vom 16. Februar 1990 in
                der jeweils gültigen Fassung)
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <!-- Innenteil -->
    <div class="page grid" orientation="landscape" size="A3">
      <div class="main-grid">
        <div class="main-left grid">
          <div class="main">
            <Voffset v="2" />
            2. Seite des Zeugnisses der Allgemeinen Hochschulreife für {s.Vorname}
            {s.Zusatz || ""}
            {s.Name}
            <Voffset v="2" />
            <b>Block I: Ergebnisse in der Qualifikationsphase</b>
            <div class="klein">
              (Die Bewertungen von Halbjahresergebnissen, die nicht in die
              Gesamtqualifikation eingehen, sind in Klammern gesetzt. Es müssen
              32 bis 40 Halbjahresergebnisse eingebracht werden.)
            </div>
            <table class="table table-bordered" width="100%">
              <tr>
                <td>Fach<sup>1</sup></td>
                <td colspan="4"
                  >Bewertung<sup>2</sup>
                  <br />Halbjahresergebnisse in einfacher Wertung</td
                >
              </tr>
              <tr>
                <td colspan="1" />
                <td>1. Halbjahr</td>
                <td>2. Halbjahr</td>
                <td>3. Halbjahr</td>
                <td>4. Halbjahr</td>
              </tr>
              {#each ["Sprachlich-literarisch-künstlerisches Aufgabenfeld", "Gesellschaftswissenschaftliches Aufgabenfeld", "Mathematisch-naturwissenschaftlich-technisches Aufgabenfeld"] as aufgabenfeld, i}
                <tr>
                  <td colspan="5">{aufgabenfeld}</td>
                </tr>
                {#each s.abi_abschluss_faecher.filter((f) => f.fach.Aufgabenfeld === (i + 1).toString()) as f}
                  <tr>
                    <td
                      >{f.fach.Bezeichnung}
                      {["1", "2"].includes(f.AbiFach) ? "(eA)" : ""}</td
                    >
                    <td class="text-center"
                      >{f.R12_1 === "-" ? `(${f.P12_1 || "–"})` : f.P12_1}</td
                    >
                    <td class="text-center"
                      >{f.R12_2 === "-" ? `(${f.P12_2 || "–"})` : f.P12_2}</td
                    >
                    <td class="text-center"
                      >{f.R13_1 === "-" ? `(${f.P13_1 || "–"})` : f.P13_1}</td
                    >
                    <td class="text-center"
                      >{f.R13_2 === "-" ? `(${f.P13_2 || "–"})` : f.P13_2}</td
                    >
                  </tr>
                {/each}
              {/each}
              <tr>
                <td colspan="5">&nbsp;</td>
              </tr>
              {#each ["RE", "SP"] as fach, i}
                {#each s.abi_abschluss_faecher.filter((f) => f.FachKrz === fach) as f}
                  <tr>
                    <td>{f.fach.Bezeichnung}</td>
                    <td class="text-center"
                      >{f.R12_1 === "-" ? `(${f.P12_1 || "–"})` : f.P12_1}</td
                    >
                    <td class="text-center"
                      >{f.R12_2 === "-" ? `(${f.P12_2 || "–"})` : f.P12_2}</td
                    >
                    <td class="text-center"
                      >{f.R13_1 === "-" ? `(${f.P13_1 || "–"})` : f.P13_1}</td
                    >
                    <td class="text-center"
                      >{f.R13_2 === "-" ? `(${f.P13_2 || "–"})` : f.P13_2}</td
                    >
                  </tr>
                {/each}
              {/each}
            </table>
          </div>
          <div class="footer">
            <div class="klein eng">
              Schulnummer: {schule.SchulNr}
              <hr />
              <sup>1</sup>Leistungskursfächer werden mit dem Klammerzusatz „eA“
              (erhöhtes Anforderungsniveau) gekennzeichnet. Grundkurse bleiben
              ohne besondere Kennzeichnung.
              <Voffset v="0.5" />
              <sup>2</sup> Für die Umsetzung der Noten in Punkte gilt:
              <table class="table table-bordered table-eng" width="100%">
                <tr>
                  <td width="5%" />
                  {#each ["sehr gut", "gut", "befriedigend", "ausreichend", "mangelhaft", "ungenügend"] as n}
                    <td class="text-center" width="10%">{n}</td>
                  {/each}
                </tr>
                <tr>
                  <td>Noten</td>
                  {#each [1, 2, 3, 4, 5] as i}
                    <td class="text-center">
                      <div class="flex-grid">
                        <div class="col">+</div>
                        <div class="col">{i}</div>
                        <div class="col">-</div>
                      </div>
                    </td>
                  {/each}
                  <td class="text-center">6</td>
                </tr>
                <tr>
                  <td>Punkte</td>
                  {#each [[15, 14, 13], [12, 11, 10], ["09", "08", "07"], ["06", "05", "04"], ["03", "02", "01"]] as ii}
                    <td class="text-center">
                      <div class="flex-grid">
                        {#each ii as i}
                          <div class="col">{i}</div>
                        {/each}
                      </div>
                    </td>
                  {/each}
                  <td class="text-center">00</td>
                </tr>
              </table>
            </div>
          </div>
        </div>
        <div class="main-right grid">
          <div class="main">
            <Voffset v="2" />
            3. Seite des Zeugnisses der Allgemeinen Hochschulreife für {s.Vorname}
            {s.Zusatz || ""}
            {s.Name}
            <Voffset v="2" />
            <b>Block II: Ergebnisse in der Abiturprüfung</b>
            <Voffset v="0.5" />
            <table class="table table-bordered" width="100%">
              <tr>
                <td rowspan="2">Prüfungsfach<sup>1</sup></td>
                <td colspan="2">Ergebnisse<br />in einfacher Wertung</td>
                <td class="text-center" rowspan="2"
                  >Gesamtergebnis<sup>2</sup></td
                >
              </tr>
              <tr>
                <td class="text-center">schriftlich</td>
                <td class="text-center">mündlich</td>
              </tr>
              {#each s.abi_abschluss_faecher
                .filter((f) => ["1", "2", "3"].includes(f.AbiFach))
                .sort((a, b) => a.AbiFach - b.AbiFach) as f}
                <tr>
                  <td
                    >{f.fach.Bezeichnung}
                    {["1", "2"].includes(f.AbiFach) ? "(eA)" : ""}</td
                  >
                  <td class="text-center">{f.AbiPruefErgebnis || "–"}</td>
                  <td class="text-center">{f.MdlPruefErgebnis || "–"}</td>
                  <td class="text-center">{f.AbiErgebnis}</td>
                </tr>
              {/each}
              {#each s.abi_abschluss_faecher.filter((f) => f.AbiFach === "4") as f}
                <tr>
                  <td>{f.fach.Bezeichnung}</td>
                  <td class="text-center">–</td>
                  <td class="text-center">{f.AbiPruefErgebnis}</td>
                  <td class="text-center">{f.AbiErgebnis}</td>
                </tr>
              {/each}
            </table>
            <Voffset v="2" />
            <b>Berechnung der Gesamtqualifikation und der Durchschnittsnote</b>
            <Voffset v="1" />
            Block I:
            <div class="flex-grid">
              <div class="col-2">
                Punktsumme aus den Halbjahresergebnissen<sup>3</sup>
                <br />Berechnung gemäß Formel
                <Voffset v="1" />
                <div class="text-center">
                  <img
                    src="daten/abiturformel.png"
                    width="60"
                    alt="Abiformel"
                  />
                </div>
              </div>
              <div class="col text-center">
                <h5>{s.abi_abschluss.Punktsumme_I}</h5>
              </div>
              <div class="col">mindestens 200,<br />höchstens 600 Punkte</div>
            </div>
            <Voffset v="1" />
            <div class="klein">
              P = Erzielte Punkte in den eingebrachten Fächern<sup>4</sup>
              <br />S = Anzahl der Schulhalbjahresergebnisse. Doppelt gewichtete
              Fächer zählen auch hier doppelt
            </div>
            <Voffset v="1" />
            Block II:
            <div class="flex-grid">
              <div class="col-2">
                Punktsumme aus den Gesamtergebnissen in den Prüfungsfächern in
                5-facher Wertung
              </div>
              <div class="col text-center">
                <h5>{s.abi_abschluss.Punktsumme_II}</h5>
              </div>
              <div class="col">mindestens 100,<br />höchstens 300 Punkte</div>
            </div>
            <Voffset v="1" />
            <div class="flex-grid">
              <div class="col-2">Gesamtpunktzahl:</div>
              <div class="col text-center">
                <h5>{s.abi_abschluss.GesamtPunktzahl}</h5>
              </div>
              <div class="col">
                mindestens 300,<br />höchstens 900 Punkte
              </div>
            </div>
            <Voffset v="1" />
            <div class="flex-grid">
              <div class="col-2">Durchschnittsnote:</div>
              <div class="col text-center"><h5>{s.abi_abschluss.Note}{s.abi_abschluss.Note?.includes(',') ? '':',0'}</h5></div>
              <div class="col" style="padding-top: .7em">
                In Worten: {@html noteInWorten(s.abi_abschluss.Note)}{@html s.abi_abschluss.Note?.includes(',') ? '':' / n&#8291;ull'}
              </div>
            </div>
            <Voffset v="1" />
            <b>Fremdsprachen</b>
            <Voffset v="1" />
            <table class="table table-bordered" width="100%">
              <tr
                ><td>Fach</td><td>Jahrgangsstufen</td><td
                  >Niveau gemäß GER<sup>5</sup></td
                ></tr
              >
              {#each s.sprachenfolgen as sprache}
                <tr>
                  <td>{sprache.fach.Bezeichnung}</td>
                  <td>von {sprache.JAHRGANGVON} bis {sprache.JAHRGANGBIS}</td>
                  <td>{sprache.Referenzniveau || "–"}</td>
                </tr>
              {/each}
            </table>
          </div>
          <div class="footer klein eng">
            <hr />
            <sup>1</sup>Leistungskursfächer werden mit dem Klammerzusatz „eA“
            (erhöhtes Anforderungsniveau) gekennzeichnet.
            <Voffset v="0.5" />
            <sup>2</sup> Die Ergebnisse der schriftlichen und der mündlichen
            Prüfung sind hierbei im Verhältnis 2:1 gewichtet. Das Gesamtergebnis
            ist in 5-facher Wertung angegeben; es wurde auf eine ganzzahlige
            Punktzahl gerundet; dabei wurde ab der Dezimalen 5 aufgerundet.
            <Voffset v="0.5" />
            <sup>3</sup> Es müssen mindestens 32 Halbjahresergebnisse
            eingebracht werden. Die Punktsumme (E) wurde auf eine ganzzahlige
            Punktzahl gerundet; dabei wurde ab der Dezimalen 5 aufgerundet.
            <Voffset v="0.5" />
            <sup>4</sup> Die Ergebnisse in den Leistungskursfächern werden
            doppelt gewichtet.
            <Voffset v="0.5" />
            <sup>5</sup> Für die modernen Fremdsprachen schließt das Zeugnis den
            „Europäischen Referenzrahmen für Sprachen: Lernen, Lehren, Beurteilen“
            ein. Sind zwei Referenzniveaus ausgewiesen, ist das niedrigere in vollem
            Umfang, das höhere in Anteilen erreicht. Das ausgewiesene Niveau kann
            auch in einem vorangegangenen Schuljahr erreicht worden sein.
          </div>
        </div>
      </div>
    </div>
  {/each}
{/each}

<style>
  @import "css/main.css";
  .sl {
    position: relative;
    bottom: -126px;
    left: -90px;
  }
  .table-bordered {
    border-collapse: collapse;
  }
  .table-bordered tr td {
    border: thin solid #ddd;
    padding: 8px;
    line-height: 1.3rem;
  }
  .table-eng tr td {
    margin-bottom: 0;
    padding: 1px;
  }
</style>
