<script>
  import ShoeGraphic from './lib/ShoeGraphic.svelte';

  const SNUG = 70;

  let footIn = false;
  let pressure = 0;
  let zipped = true;
  let folded = false;
  let showInfo = false;
  let message = 'Shoe is empty and deflated.';
  let timer;

  $: lights = Math.round(pressure / 20);

  function run(step, done) {
    clearInterval(timer);
    timer = setInterval(() => {
      if (step()) {
        clearInterval(timer);
        done();
      }
    }, 50);
  }

  function inflate() {
    if (!footIn) {
      message = 'No foot detected. Lining stays deflated.';
      return;
    }
    if (folded || !zipped) {
      message = 'Fold the top up and zip it before tightening.';
      return;
    }
    message = 'Inflating...';
    run(() => (pressure = Math.min(SNUG, pressure + 2)) >= SNUG, () => (message = 'Snug fit reached.'));
  }

  function release() {
    if (pressure === 0) {
      message = 'Lining is already deflated.';
      return;
    }
    message = 'Releasing air...';
    run(() => (pressure = Math.max(0, pressure - 3)) <= 0, () => (message = 'Air released.'));
  }

  function toggleZip() {
    if (folded) {
      message = 'Fold the top up before zipping.';
      return;
    }
    if (zipped && pressure > 0) {
      message = 'Release air before unzipping.';
      return;
    }
    zipped = !zipped;
    message = zipped ? 'Zipped.' : 'Unzipped. The top can fold down.';
  }

  function toggleFold() {
    if (zipped) {
      message = 'Unzip before folding the top.';
      return;
    }
    folded = !folded;
    message = folded ? 'Top folded down.' : 'Top folded up.';
  }

  function toggleFoot() {
    if (footIn && pressure > 0) {
      message = 'Release air before taking your foot out.';
      return;
    }
    footIn = !footIn;
    message = footIn ? 'Foot detected.' : 'Foot removed.';
  }

  function reset() {
    clearInterval(timer);
    footIn = false;
    pressure = 0;
    zipped = true;
    folded = false;
    message = 'Shoe is empty and deflated.';
  }
</script>

<main>
  <section class="device">
    <h2>Shoe</h2>
    <div class="shoe">
      <ShoeGraphic {folded} />

      <button class="hotspot toe" on:click={inflate} title="Inflate">+</button>
      <button class="hotspot heel" on:click={release} title="Release air">−</button>
      <button class="hotspot zip" class:open={!zipped} class:low={folded} on:click={toggleZip} title={zipped ? 'Unzip' : 'Zip'}>
        {zipped ? 'Unzip' : 'Zip'}
      </button>
      <button class="hotspot fold" class:folded on:click={toggleFold} title={folded ? 'Fold up' : 'Fold down'}>
        {folded ? 'Fold up' : 'Fold down'}
      </button>

      <div class="lights">
        {#each [1, 2, 3, 4, 5] as i}
          <span class:lit={i <= lights}></span>
        {/each}
      </div>
    </div>

    <dl class="legend">
      <dt>Toe button</dt><dd>Inflates the air lining until it fits your foot</dd>
      <dt>Heel button</dt><dd>Releases the air</dd>
      <dt>Zipper</dt><dd>Opens around the ankle so the high top can fold down</dd>
      <dt>Lights</dt><dd>Show how full the lining is</dd>
    </dl>
  </section>

  <section class="testing">
    <h1>Smart Shoe</h1>
    <p>Jason Bellerjeau</p>

    <button on:click={() => (showInfo = !showInfo)}>{showInfo ? 'Hide info' : 'Info'}</button>
    {#if showInfo}
      <div class="info">
        <p>Use the buttons below to simulate someone wearing the shoe. The controls on the shoe itself work the same way the real buttons would.</p>
        <p>Put a foot in, then press the toe button to tighten. Press the heel button to loosen. To fold the top down, unzip first.</p>
      </div>
    {/if}

    <h3>Simulate</h3>
    <div class="sim">
      <button on:click={toggleFoot}>{footIn ? 'Take foot out' : 'Put foot in'}</button>
      <button on:click={reset}>Reset</button>
    </div>

    <h3>Status</h3>
    <ul class="status">
      <li>Foot: {footIn ? 'in' : 'out'}</li>
      <li>Lining: {pressure}%</li>
      <li>Zipper: {zipped ? 'closed' : 'open'}</li>
      <li>Top: {folded ? 'folded down' : 'up'}</li>
    </ul>
    <p class="message">{message}</p>
  </section>
</main>

<style>
  :global(body) {
    margin: 0;
    font-family: 'Trebuchet MS', Verdana, sans-serif;
    background: #eef1f2;
    color: #22303a;
  }

  main {
    display: grid;
    grid-template-columns: 640px 320px;
    gap: 24px;
    padding: 24px;
    width: max-content;
  }

  section {
    background: #fff;
    border: 2px solid #2b3a42;
    padding: 20px;
  }

  h1, h2, h3 {
    margin: 0 0 8px;
  }

  h3 {
    margin-top: 20px;
  }

  .testing p {
    margin: 4px 0;
  }

  .shoe {
    position: relative;
    width: 600px;
    height: 360px;
  }

  .hotspot {
    position: absolute;
    transform: translate(-50%, -50%);
    border: 2px solid #2b3a42;
    background: #d9c27a;
    font-weight: bold;
    cursor: pointer;
  }

  .toe {
    left: 10%;
    top: 71%;
    width: 36px;
    height: 36px;
    border-radius: 50%;
    font-size: 20px;
  }

  .heel {
    left: 86%;
    top: 62%;
    width: 36px;
    height: 36px;
    border-radius: 50%;
    font-size: 20px;
    background: #e9a08f;
  }

  .zip {
    left: 73%;
    top: 39%;
    padding: 4px 8px;
    border-radius: 4px;
  }

  .zip.open {
    background: #fff;
  }

  .zip.low {
    top: 34%;
  }

  .fold {
    left: 74%;
    top: 21%;
    padding: 4px 8px;
    border-radius: 4px;
    background: #c9dde4;
  }

  .fold.folded {
    top: 51%;
  }

  .lights {
    position: absolute;
    left: 30%;
    top: 84%;
    display: flex;
    gap: 6px;
  }

  .lights span {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: #3f5f6f;
    border: 1px solid #2b3a42;
  }

  .lights span.lit {
    background: #7fe0c4;
  }

  .legend {
    display: grid;
    grid-template-columns: max-content 1fr;
    gap: 4px 12px;
    margin: 16px 0 0;
  }

  .legend dt {
    font-weight: bold;
  }

  .legend dd {
    margin: 0;
  }

  .info {
    margin-top: 8px;
    padding: 8px 12px;
    background: #eef1f2;
  }

  .sim {
    display: flex;
    gap: 8px;
  }

  button {
    font: inherit;
  }

  .testing > button, .sim button {
    padding: 6px 12px;
    border: 2px solid #2b3a42;
    background: #fff;
    cursor: pointer;
  }

  .status {
    padding-left: 18px;
    margin: 0;
  }

  .message {
    margin-top: 12px !important;
    padding: 8px;
    border-left: 4px solid #d9c27a;
    background: #faf6e8;
  }
</style>