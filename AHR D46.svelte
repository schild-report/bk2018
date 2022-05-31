<script>
  export const kommentar = `
[Anlage D 46](https://bass.schul-welt.de/anlagen/3129-51.pdf)
  `;
  import { datum } from "./helfer";
  import Seitenlogo from "./partials/Seitenlogo.svelte";
  import Pageheader from "./partials/Pageheader.svelte";
  import Voffset from "./partials/Voffset.svelte";

  export let schueler, schule, privat, jahr, abschnitt;
  const aktHalbjahr = (a) => a.Jahr === jahr && a.Abschnitt === abschnitt;
  function bkfach(s, a) {
    return s.bk_abschluss_faecher.find((f) => f.Kursart === a);
  }
</script>

{#each schueler as s (s.ID)}
  {#each s.abschnitte.filter(aktHalbjahr) as hj (hj.ID)}
    <div
      class="page grid"
      orientation="portrait"
      size="A4"
      style="font-size: 1.0rem"
    >
      <div class="header">
        <Seitenlogo logo={privat.traegerLogo} />
        <Pageheader logo={privat.logo} untertitel={privat.untertitel} />
      </div>
      <div class="main">
        <div style="font-size: 12pt; line-height: 1.5em">
          Zulassung zur Ersten Teilprüfung der Berufsabschlussprüfung für den
          Bildungsgang Erzieherin und Allgemeine Hochschulreife/ Erzieher und
          Allgemeine Hochschulreife
        </div>
        <br />Bekanntgabe der Vornoten, der bisherigen Prüfungsergebnisse im
        Rahmen der Abiturprüfung, der vorläufigen<sup>1</sup> Abschlussnoten und
        der Zulassung zur Ersten Teilprüfung im Bildungsgang Erzieherin und
        Allgemeine Hochschulreife/ Erzieher und Allgemeine Hochschulreife

        <Voffset v="0.5" /><b>Beschluss des allgemeinen Prüfungsausschusses</b>
        <br />
        Sehr geehrte{s.Geschlecht === 4 ? "" : "r"}
        {s.anrede} <b>{s.Vorname} {s.Zusatz || ""} {s.Name}</b>, in seiner
        Sitzung am {datum(hj.ZeugnisDatum)} hat der allgemeine Prüfungsausschuss
        die Leistungen<sup>1,2</sup>
        in den Fächern gemäß der Nummern 1 bis 4 festgestellt und über die Möglichkeit
        des Bestehens der Ersten Teilprüfung der Berufsabschlussprüfung gemäß Nummer
        5 entschieden.
        <Voffset v="1" />
        1. Fächer, die im Rahmen des schriftlichen Abiturverfahrens geprüft wurden.
        In diesen Fächern ist im Rahmen der Ersten Teilprüfung keine gesonderte mündliche
        Prüfung möglich:<sup>3</sup>
        <Voffset v="0.5" />
        <table class="table table-bordered table-eng">
          <tr>
            <td rowspan="2">Fach</td>
            <td rowspan="2">Vornote</td>
            <td colspan="2" class="text-center">Note der</td>
            <td rowspan="2" class="text-center">vorläufige Abschlussnote</td>
          </tr>
          <tr>
            <td class="text-center">schriftlichen Prüfung</td>
            <td class="text-center">mündlichen Prüfung</td>
          </tr>
          <tr>
            <td>{bkfach(s, "LK1").fach.Zeugnisbez}</td>
            <td class="text-center">{bkfach(s, "LK1").Vornote}</td>
            <td class="text-center">{bkfach(s, "LK1").NoteSchriftlich}</td>
            <td class="text-center">{bkfach(s, "LK1").NoteMuendlich}</td>
            <td class="text-center">{bkfach(s, "LK1").NoteAbschluss}</td>
          </tr>
          <tr>
            <td>{bkfach(s, "LK2").fach.Zeugnisbez}</td>
            <td class="text-center">{bkfach(s, "LK2").Vornote}</td>
            <td class="text-center">{bkfach(s, "LK2").NoteSchriftlich}</td>
            <td class="text-center">{bkfach(s, "LK2").NoteMuendlich}</td>
            <td class="text-center">{bkfach(s, "LK2").NoteAbschluss}</td>
          </tr>
          <tr>
            <td>{bkfach(s, "AB3").fach.Zeugnisbez}</td>
            <td class="text-center">{bkfach(s, "AB3").Vornote}</td>
            <td class="text-center">{bkfach(s, "AB3").NoteSchriftlich}</td>
            <td class="text-center">{bkfach(s, "AB3").NoteMuendlich}</td>
            <td class="text-center">{bkfach(s, "AB3").NoteAbschluss}</td>
          </tr>
        </table>
        <Voffset v="1" />
        2. Fach, das im Rahmen des Abiturverfahrens mündlich geprüft wurde. Dieses
        Fach kann als Fach mit mündlicher Prüfung im Rahmen der Ersten Teilprüfung
        eingebracht werden, wenn dadurch das Bestehen der Ersten Teilprüfung nicht
        gefährdet wird.<sup>4,5</sup> Die Note der mündlichen Prüfung und die
        sich hieraus ergebenden vorläufigen Abschlussnote wird zur Information
        in Klammern angegeben, damit sie bei der späteren Entscheidung für eine
        Einbringung einbezogen werden kann:
        <Voffset v="0.5" />
        <table class="table table-bordered table-eng" style="width: 100%;">
          <tr>
            <td>Fach</td>
            <td class="text-center">Vornote</td>
            <td class="text-center">Note der mündlichen Prüfung</td>
            <td class="text-center">Vorläufige Abschlussnote</td>
          </tr>

          <tr>
            <td>{bkfach(s, "AB4").fach.Zeugnisbez}</td>
            <td class="text-center">{bkfach(s, "AB4").Vornote}</td>
            <td class="text-center">({bkfach(s, "AB4").NoteMuendlich})</td>
            <td class="text-center">({bkfach(s, "AB4").NoteAbschluss})</td>
          </tr>
        </table>
      </div>
      <div class="footer">
        <div class="klein eng">
          Schulnummer: {schule.SchulNr}
          <hr />
          <sup>1</sup> Notenstufen gemäß § 48 Absatz 3 SchulG: sehr gut (1), gut
          (2), befriedigend (3), ausreichend (4), mangelhaft (5), ungenügend (6)
          <br /><sup>2</sup> Die Bewertungen, die im Rahmen der Ersten
          Teilprüfung der Berufsabschlussprüfung nicht berücksichtigt werden,
          sind in Klammern gesetzt.
          <br /><sup>3</sup> Die Note der mündlichen Prüfung und der vorläufige
          Abschlussnote erfolgte durch die Anrechnung der Prüfungsleistung in
          der Abiturprüfung nach § 21 i.V.m. § 29 und §§ 41b, 41c Anlage D
          APO-BK.
          <br /><sup>4</sup>Bei der späteren Einbringung der Note der mündlichen
          Prüfung des Abiturverfahrens ist das vierte Abiturfach das erste Fach
          mit einer münd- lichen Prüfung nach § 41e Anlage D APO-BK Absatz 1
          Satz 2. In diesem Fall kann höchstens noch ein weiteres Fach gemäß
          Nummer 3 oder gegebenenfalls Nummer 4 mit einer mündlichen Prüfung
          benannt werden.
          <br /><sup>5</sup> Wurde die zweite Fremdsprache als viertes Abiturfach
          gewählt, so wird die Vornote nicht in das Zulassungsverfahren nach § 41a
          Anlage D APO-BK einbezogen und in Klammern ausgewiesen.
        </div>
      </div>
    </div>
    <div
      class="page grid"
      orientation="portrait"
      size="A4"
      style="font-size: 1.0rem"
    >
      <div class="main">
        <Voffset v="2" />
        3. Weitere Fächer, die in die Zulassungsentscheidung für die Erste Teilprüfung
        einbezogen und im Rahmen der Ersten Teilprüfung mündlich geprüft werden können:<sup
          >6</sup
        >
        <Voffset v="0.5" />
        <table class="table table-bordered table-eng" style="width: 50%;">
          <tr>
            <td>Fach</td>
            <td class="text-center">Vornote</td>
          </tr>
          {#each s.bk_abschluss_faecher.filter( (f) => f.Kursart.startsWith("GK") ) as f}
            <tr>
              <td>{f.fach.Zeugnisbez}</td>
              <td class="text-center">{f.Vornote}</td>
            </tr>
          {/each}
        </table>
        <Voffset v="1" />
        4. Weitere Fächer, die nicht in die Zulassungsentscheidung für die Erste
        Teilprüfung einbezogen werden. Fächer, die gegebenenfalls im Rahmen der Ersten
        Teilprüfung mündlich geprüft werden können, sind gekennzeichnet<sup
          >2,3</sup
        >:
        <Voffset v="0.5" />
        <table class="table table-bordered table-eng" style="width: 50%;">
          <tr>
            <td>Fach</td>
            <td class="text-center">Vornote</td>
          </tr>
          {#each s.bk_abschluss_faecher.filter((f) => (f.Kursart.startsWith("ZK") && !f.Vornote.startsWith("E")) || (f.Kursart.startsWith("GK") && f.fach.IstSprache === "+")) as f}
            <tr>
              <td>{f.fach.Zeugnisbez}</td>
              <td class="text-center">{f.Vornote}</td>
            </tr>
          {/each}
        </table>
        <Voffset v="1" />
        5. Sie werden nach §§ 41a und 41f Anlage D APO-BK (SGV. NRW. 223/BASS 13-33
        Nr. 1.1) zum 1 Ersten Teil der Berufsabschlussprüfung zugelassen, da die
        Möglichkeit des Bestehens des Ersten Teils der Berufsabschlussprüfung besteht.
        <br />Sie können der Schulleiterin spätestens am zweiten Werktag nach
        dieser Bekanntgabe der Zulassung zum Ersten Teil der
        Berufsabschlussprüfung bis zu zwei Fächer schriftlich benennen, in denen
        sie mündlich geprüft werden möchten. Die Meldung ist möglich
        <br />5.1. für die Einbringung der Note der mündlichen Prüfung in der
        Abiturprüfung im vierten Abiturfach nach Nummer 2 und bis zu einem Fach
        nach Nummer 3 oder gegebenenfalls Nummer 4,
        <br />5.2. für bis zu zwei Fächern nach Nummer 3 oder gegebenenfalls
        Nummer 4.
      </div>
      <div class="footer">
        <div class="flex-grid">
          <div class="col">
            {schule.Ort}, den {datum(hj.Konferenzdatum)}
          </div>
          <div class="col text-center klein">
            <Voffset v="3" />
            <hr />
            {schule.SchulleiterVorname}
            {schule.SchulleiterName}
            <br />Vorsitzende des allgemeinen Prüfungsausschusses
          </div>
        </div>
        <Voffset v="4" />
        <b>Rechtsbehelfsbelehrung:</b><br />
        Gegen dieses Zeugnis kann innerhalb eines Monats nach Bekanntgabe des Zeugnisses
        Widerspruch eingelegt werden. Der Widerspruch ist beim {schule.Bezeichnung2},
        {schule.Strasse}, {schule.PLZ}
        {schule.Ort}, schriftlich oder zur Niederschrift zu erheben. <br />Falls
        die Frist durch das Verschulden einer/eines Bevollmächtigten versäumt
        wird, wird dies Verschulden der Widerspruchsführerin/dem
        Widerspruchsführer zugerechnet.
        <Voffset v="2" />
        <div class="klein eng">
          Schulnummer: {schule.SchulNr}
          <hr />
          <sup>6</sup>Nach § 41e Anlage D APO-BK ist die spätere Meldung von bis
          zu zwei Fächern mit mündlichen Prüfungen möglich. Eine Meldung des
          vierten Abiturfaches nach Nummer 2 als Fach mit mündlicher Prüfung
          wird entsprechend auf die Fächer nach Nummer 3 und gegebenenfalls
          Nummer 4 angerechnet.
          <br /><sup>7</sup>Weiteres mögliches mündliches Prüfungsfach nach §
          41e Anlage D APO-BK
          <br /><sup>8</sup>Die Vornoten sind in Klammern auszuweisen. Soweit
          die Fächer die Anforderungen an Grundkurse erfüllen, können sie als
          Fächer der mündlichen Prüfung benannt werden, auch wenn sie nicht in
          das Zulassungsverfahren nach § 41a Absätze 1 und 2 Anlage D APO-BK
          einbezogen werden. Benennt ein Prüfling diese Fächer für die mündliche
          Prüfung nach § 41e Anlage D APO-BK, ist die vorläufige Abschlussnote
          bei der Feststellung des Prüfungsergebnisses nach § 41f Absatz 2
          Anlage D APO-BK einzubeziehen. Durch diese Benennung darf die
          Zulassung nicht gefährdet werden. Fächer, die nicht mit einer Note
          bewertet wurden, sind nicht ausgewiesen.
        </div>
      </div>
    </div>
  {/each}
{/each}

<style>
  @import "css/main.css";
  .table-bordered {
    border-collapse: collapse;
  }
  .table-bordered td {
    border: thin solid #ddd;
    line-height: 1.3rem;
  }
  .table-eng {
    margin-bottom: 0;
  }
</style>
