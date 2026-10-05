<script>
  export let folded = false;
</script>

<svg viewBox="0 0 400 240" xmlns="http://www.w3.org/2000/svg">
  <rect x="28" y="188" width="330" height="26" rx="10" fill="#e8e4dc" stroke="#2b3a42" stroke-width="3" />

  {#if folded}
    <path d="M40 190 C30 160 60 145 100 140 L230 110 L255 100 L345 96 L352 190 Z" fill="#5b7c8d" stroke="#2b3a42" stroke-width="3" />
    <path d="M255 100 L345 96 L340 142 L262 146 Z" fill="#3f5f6f" stroke="#2b3a42" stroke-width="3" />
    <line x1="257" y1="100" x2="343" y2="96" stroke="#d9c27a" stroke-width="4" stroke-dasharray="3 3" />
  {:else}
    <path d="M40 190 C30 160 60 145 100 140 L230 110 L250 38 L340 28 L352 190 Z" fill="#5b7c8d" stroke="#2b3a42" stroke-width="3" />
    <line x1="236" y1="96" x2="346" y2="90" stroke="#d9c27a" stroke-width="4" stroke-dasharray="3 3" />
  {/if}

</svg>

<style>
  svg {
    width: 100%;
    height: 100%;
    display: block;
  }
</style>