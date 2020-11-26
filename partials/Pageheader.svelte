<!-- Folgende Attribute werden unterstützt:
art:  z.B. Zeugnis, Abgangszeugnis etc. Sollte auf eine vorhandene Datei zeigen
      wird unten links platziert im Header
logo: z.B Schullogo. Wird oben recht splatziert
untertitel: Wird unterhalb des Logos platziert.
hr:   Wenn eine rote Linie unter das Logo soll. Standard an -->
<div class="header">
    {#if art}
      <img class="art" src="{art}" alt="logo_art"/>
    {/if}
    {#if logo}
      <img class="logo" src="{logo}" alt="logo"/>
    {/if}
    {#if untertitel}
      <img class="untertitel" src="{untertitel}" alt="logo_untertitel"/>
    {/if}
    {#if hr}
      <hr class="hr-rot"/>
    {/if}
</div>
<script>
  export let art, logo, untertitel
  export let hr = true
</script>

<style>
  .header {
    position: relative;
    height: 115px;
  }
  .art {
    position: absolute;
    bottom: 12px;
  }
  .logo {
    position: absolute;
    right: 0px;
  }
  .untertitel {
    position: absolute;
    right: 0px;
    bottom: 12px;
  }
  .hr-rot {
    border-color: #ff2700;
    margin: 8px 0;
    position: absolute;
    bottom: 0px;
    width: -webkit-fill-available;
  }
</style>
