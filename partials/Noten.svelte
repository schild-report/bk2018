<!--
Allgemeine Notentabelle für Berufskollegs, die z.B. die etwas langtextigen
Lernfeldfächer unterstützt oder verschiedene Abschlussnoten.

Folgende Attribute werden unterstützt:
lernfeld: Sorgt dafür, dass die Klasse `fach-bezeichnung` nicht gesetzt wird und
          Fächer dementsprechend nicht eingerückt werden (nur Lernfeldfächer)
          true|false Standardwert false
noten:    Hier können Abschnittsnoten oder Abschlussnoten gesetzt werden, z.B.
          hj.noten oder s.bk_abschluss_faecher oder s.fhr_abschluss_faecher
          Muss gesetzt werden
fachGliederungen: sollten mitgegeben werden, wenn die für verschiedene Bildungsgänge
          geänderten Werte berücksichtigt werden sollen, z.B. andere Bezeichnungen
          in den verschiedenen Bildungsgängen
ger:      Wenn ein Referenzniveau für die Fremdsprachen angegeben werden soll
          true|false, Standardwert false,
fachklasse: muss mitgegeben werden, wenn aus der Fachklassen.json Datei der Standard-
          Wert gelesen werden soll. Ansonsten über Lernentw festlegen, was eigetragen wird
faechergruppenIds: legt fest, welche Fächergruppen gezogen werden sollen. Standartwert ist
          ein leerer Array, d.h. alle Fächergruppen werden gezogen. Ansonsten können
          nur bestimmte festgelegt werden z.B. [10, 20, 30]
fhrSternchen: Legt fest, ob die für die FHR relevanten Fächer ein Sternchen
          bekommen. Standardwert ist false
gruppenbezeichnungNeu: Legt für eine GruppenID eine neue Gruppenbezeichnung fest
          Es wird ein Objekt erwartet, z.B. {40: 'Fachpraktische Prüfung'}
berufsbezogenZuerst: Diese Einstellung legt fest, dass die gen. Fächer zuerst auf
          dem Zeugnis als Fächergruppe erscheinen wie in der BASS vorgegeben.
-->

<table class="table-noten">
  {#each faechergruppen as [gruppen_id, gruppenFaecher]}
    <tr>
      <td class="fachgruppe-margin">
        <b>{gruppenbezeichnung(gruppen_id)}</b>
      </td>
    </tr>
    {#each lernbereicheUndFaecher(gruppenFaecher) as [lernbereich, faecher]}
      {#if lernbereich != faecher[0].fach.Zeugnisbez}
        <tr><td colspan="2" class="{lernfeld ? '' : 'fach-bezeichnung'}">{lernbereich}</td></tr>
      {/if}
      {#each faecher as f (f.ID)}
        {#if f.fach.AufZeugnis === '+'}
          <tr>
            <td class="{lernfeld ? '' : 'fach-bezeichnung'} {lernbereich != faecher[0].fach.Zeugnisbez ? 'lernfeld-lernbereich' : ''}" style="display: inline-flex">
              {#if f.fach.Zeugnisbez.startsWith("Projekt")}
                <!-- Funktioniert nicht mit Abschlussnoten, da diese keine Spalte `Lernentw` haben -->
                {f.Lernentw}
              {:else}
                <!-- Die Zeugnisbezeichnung hat bei gesetzten fachGliederungen einen anderen Namen... -->
                {f.fach.ZeugnisBez || f.fach.Zeugnisbez}
                <!-- Ist ger gesetzt, unter Lernentw nach Eintrag schauen. Wenn dort nichts steht,
                in der Fachklassen.json nach Einträgen für Fremdsprache und Fach_ID suchen
                Mit der Fussnote 1 muss natürlich auch tatsächlich eine Fussnote zum GER existieren … -->
                {#if ger && f.fach.IstSprache === '+'}
                  {fremdsprache(f, fachklasse)}<sup>1</sup>
                {/if}
                <!-- Die für die FHR Durchschnittsnote einbezogenen Noten bekommen in
                den FS-Zeugnissen ein Sternchen -->
                {#if fhrSternchen && f.fach.GewichtungFHR > 0}
                  <sup>*</sup>
                {/if}
              {/if}
            </td>
            <!-- Entweder Abschlussnote, wenn gesetzt oder Abschnittsnote oder nix -->
            <!-- Wenn als Note eines der 'Teilgenommen' steht, dann ohne graue Hinterlegung.
            Außer es ist eine Sprache, dann wird es wohl ein 'teilgenommen' sein,
            das auf eine Fremdsprachenprüfung hinweist und nicht mit den anderen
            Fächern im Diff-Bereich steht. Optisch besser. -->
            {#if (f.fach.IstSprache==='-') && (f.NoteAbschluss?.toUpperCase().startsWith('E') || f.NotenKrz?.toUpperCase().startsWith('E'))}
              <td class="td-fach-note"><span class="e-note">{note(f.NoteAbschluss) || note(f.NotenKrz)}</span></td>
            {:else}
              <td class="td-fach-note"><span>{note(f.NoteAbschluss) || note(f.NotenKrz) || '–––––'}</span></td>
            {/if}
          </tr>
        {/if}
      {/each}
    {/each}
    <tr>
    </tr>
  {/each}
</table>

<script>
  import { gruppenIds } from './gruppenIDs'
  import { note, fremdsprache, groupBy } from '../helfer'

  export let lernfeld
  export let ger = false
  export let faechergruppenIds = []
  export let noten, fachGliederungen, fachklasse
  export let fhrSternchen, gruppenbezeichnungNeu
  export let berufsbezogenZuerst = true

  let faechergruppen

  // Berechne die Noten neu, wenn sich die Notenquelle ändert
  $: noten && faechergruppen_calc()

  Object.assign(gruppenIds, gruppenbezeichnungNeu)

  const faechergruppen_calc = () => {
    // noten werden im jeweiligen Dokument über das *noten*-Attribut mitgegeben.
    // z.B. die Abschnittsnote oder die Noten aus *s.bk_abschluss_faecher* und co.
    // Nutzer über Konsole auf Änderungen in den Fächern hinweisen
    if (fachGliederungen && fachGliederungen.length > 0) console.log('Es wurden Fachgliederungswerte gesetzt: ', fachGliederungen)
    // suche in den fachGliederungen, ob für die jeweilige Fachklasse was besonderes festgelegt wurde
    // vorausgesetzt, fachGliederungen wurde mitgegeben
    const faecher = fachGliederungen
      ? noten.map(f => {return {...f, fach: Object.assign(f.fach, fachGliederungen.find(fg => fg.Fach_ID === f.Fach_ID))}})
      : noten
    // nun alle Fächer rauswerfen, die nicht in den FaechergruppenIds zugeordnet sind, z.B. Abschlussarbeiten, Projekte etc.
    const faecherBereinigt = faechergruppenIds.length === 0
      ? faecher
      : faecher.filter(f => faechergruppenIds.find(id => f.fach.Fachgruppe_ID === id))
    // erstmal alle Fächer sortieren in der passenden Reihenfolge,
    // dann nach Fachgruppe gruppieren
    const sortiert = faecherBereinigt.sort((a, b) => a.fach.SortierungS2 < b.fach.SortierungS2 ? 1 : -1)
    let gruppiert = groupBy(sortiert, 'fach.Fachgruppe_ID')
    // Die BK-Zeugnisse führen zuerst die berufsbezogenen Fächer auf.
    // D.h. Gruppen_ID 20 muss auf 0 gesetzt werden, damit es an den Anfang kommt
    if (berufsbezogenZuerst && gruppiert.get(20)) {
      gruppiert.set(0, gruppiert.get(20))
      gruppiert.delete(20)
      gruppenIds[0] = gruppenIds[20]
    }
    faechergruppen = [...gruppiert.entries()].sort((a,b) => a[0] - b[0])
  }
  const gruppenbezeichnung = gruppe => gruppenIds[gruppe]
  const lernbereicheUndFaecher = gruppenFaecher => [...groupBy(gruppenFaecher, 'fach.Bezeichnung').entries()].reverse()
</script>

<style>
.table-noten {
  width: 100%;
  border: none;
  border-collapse: collapse;
  margin-bottom: 1rem;
}
.fach-bezeichnung {
  padding-left: 10mm;
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
.td-fach-note span.e-note {
  background-color: white !important;
  width: 17em;
  margin: 0;
  padding: 0;
}
.fachgruppe-margin {
  padding-top: 0.3rem;
}
.lernfeld-lernbereich {
  padding-left: 2rem;
  padding-bottom: .5rem;
}
</style>
