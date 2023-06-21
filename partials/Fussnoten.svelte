<!-- Allgemeine Fussnoten

Folgende Attribute werden unterstützt:
schule:     Hier stehen die SchulNr etc drin. Muss mitgegeben werden
ger:        Wenn der GER für AGZ oder ASZ verwendet werden soll, mitangeben.
            true|false Standardwert ist false
notenstufen:Wenn die Notenstufen mit angegeben werden sollen.
            true|false Standardwert ist false
verordnung :true|false Standartwert false
bfs:        true|false Standardwert false
rechtsbehlfsbelehrung :true|false Standardwert true

Ein defauld-Slot für weitere Fußnoten ist vorhanden

Beachte, true|false nicht als String setzen, sondern als statement:
notenstufen={false} -->

<div class="klein eng">
  <Voffset v="1"/>
  <div class="schulnummer">
    Schulnummer: {schule.SchulNr}
  </div>
  <hr class="hr-grau" />
  <div class="grau">
    {#if notenstufen}
      Notenstufen gemäß § 48 SchulG: 1 = sehr gut, 2 = gut, 3 = befriedigend,
      4 = ausreichend, 5 = mangelhaft, 6 = ungenügend
      <Voffset v=".5"/>
    {/if}
    {#if ger}
      <sup>1</sup>Der Unterricht in den modernen Fremdsprachen hat auf der nach dem Fach in
      Klammern angegebenen Niveaustufe des „Europäischen Referenzrahmens für
      Sprachen: Lernen, Lehren, Beurteilen“ stattgefunden. Sind zwei
      Referenzniveaus ausgewiesen, ist das niedrigere in vollem Umfang, das höhere
      in Anteilen erreicht. Bei mindestens ausreichenden Leistungen wird der
      sprachliche Kompetenzerwerb auf diesem Niveau bescheinigt.
      <Voffset v=".5"/>
    {/if}
    {#if verordnung}
      Dem Zeugnis liegen zugrunde: Die Verordnung über die Ausbildung und Prüfung
      in den Bildungsgängen des Berufskollegs (Ausbildungs- und Prüfungsordnung
      Berufskolleg, APO-BK) vom 26. Mai 1999 (SGV. NRW. 223/BASS 13 – 33 Nr. 1.1).
    {#if bfs}
      <br>Die Rahmenvereinbarung über die Berufsfachschulen (Beschluss der
      Kultusministerkonferenz vom 17.10.2013) in der jeweils geltenden Fassung.
    {:else if fs}
      <br>Die Vereinbarung über Fachschulen (Beschluss der
      Kultusministerkonferenz vom 07. November 2002 in der jeweils geltenden
      Fassung).
    {:else if fsasz}
      <br>Abschluss der Fachschule entspricht der Rahmenvereinbarung über
      Fachschulen (Beschluss der Kultusministerkonferenz vom 07.11.2002 in
      der jeweils geltenden Fassung) und wird von allen Ländern in der
      Bundesrepublik Deutschland anerkannt.
    {/if}
      <Voffset v=".5"/>
    {/if}
    {#if rechtsbehelfsbelehrung}
      Rechtsbehelfsbelehrung: Gegen dieses Zeugnis kann innerhalb eines Monats
      nach Bekanntgabe des Zeugnisses Widerspruch eingelegt werden. Der Widerspruch
      ist beim {schule.Bezeichnung1}, {schule.Strasse}, {schule.PLZ} {schule.Ort},
      schriftlich oder zur Niederschrift zu erheben. <br />Falls die Frist durch
      das Verschulden einer/eines Bevollmächtigten versäumt wird, wird dieses
      Verschulden der Widerspruchsführerin/dem Widerspruchsführer zugerechnet.
    {/if}
    <slot/>
  </div>
</div>

<script>
  export let schule, notenstufen = undefined, verordnung = undefined, fs = undefined, fsasz = undefined
  export let rechtsbehelfsbelehrung = true
  export let ger = false
  export let bfs = false
  import Voffset from './Voffset.svelte'
</script>
