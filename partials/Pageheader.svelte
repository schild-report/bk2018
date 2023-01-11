<!-- Folgende Attribute werden unterstützt:
art:  z.B. Zeugnis, Abgangszeugnis etc. Sollte auf eine vorhandene Datei zeigen
      wird unten links platziert im Header
logo: z.B Schullogo. Wird oben recht splatziert
untertitel: Wird unterhalb des Logos platziert.
hr:   Wenn eine rote Linie unter das Logo soll. Standard an -->
<div class="header">
    {#if traeger && untertitel}
      <div style="height: 157px">
        <div class="traeger">
          <img class="" style="margin-bottom: 7mm; display: block;" src="{traeger}" alt="logo"/>
          <img class="" style="margin-bottom: 1mm; block;" src="{logo}" alt="logo"/>
          <img class="" style="display: block;" src="{untertitel}" alt="logo_untertitel"/>
        </div>
        {#if art}
          <img class="art" src="{art}" alt="logo_art"/>
        {/if}
      </div>
    {/if}
    {#if logo && !traeger && untertitel}
      <div style="height: 75px">
        <div class="traeger">
          <img class="" style="margin-bottom: 1mm; block;" src="{logo}" alt="logo"/>
          <img class="" style="display: block;" src="{untertitel}" alt="logo_untertitel"/>
        </div>
        {#if art}
          <img class="art" src="{art}" alt="logo_art"/>
        {/if}
      </div>
    {/if}
    {#if logo && !untertitel}
      <div class="traeger">
        <img class="" style="margin-bottom: 1mm; block;" src="{logo}" alt="logo"/>
      </div>
    {/if}
    {#if hr}
      <hr class="hr-rot"/>
    {/if}
</div>
<script>
  export let art = undefined
  export let logo
  export let untertitel
  export let traeger
  export let hr = false
</script>

<style>
  .header {
    position: relative;
    margin-bottom: 1rem;
  }
  .art {
    position: absolute;
    bottom: 0px;
  }
  .traeger {
    position: absolute;
    right: 0px;
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
