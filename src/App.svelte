<script>
  import jalaali from "jalaali-js";
  import Field from "./Field.svelte";

  const today = new Date();

  let gregYear = today.getFullYear();
  let gregMonth = formatDayOrMonth(today.getMonth() + 1);
  let gregDay = formatDayOrMonth(today.getDate());

  let afgYear = "2020";
  let afgMonth = "01";
  let afgDay = "01";

  let gregMonthField;
  let gregDayField;

  let afgMonthField;
  let afgDayField;

  function formatDayOrMonth(month) {
    return month.toString().padStart(2, "0");
  }

  function calcAfghan() {
    try {
      const { jy, jm, jd } = jalaali.toJalaali(
        new Date(gregYear, gregMonth - 1, gregDay)
      );
      afgYear = jy.toString().padStart(4, "0");
      afgMonth = formatDayOrMonth(jm);
      afgDay = formatDayOrMonth(jd);
    } catch (e) {}
  }

  function calcGreg() {
    try {
      const { gy, gm, gd } = jalaali.toGregorian(
        parseInt(afgYear),
        parseInt(afgMonth),
        parseInt(afgDay)
      );

      if (new Date(gy, gm, gd) == "Invalid Date") return;
      gregYear = gy.toString().padStart(4, "0");
      gregMonth = gm.toString().padStart(2, "0");
      gregDay = gd.toString().padStart(2, "0");
    } catch (e) {}
  }

  calcAfghan();
</script>

<style>
  :global(body) {
    background: #c2e8ce;
  }

  @font-face {
    font-family: "Janna";
    src: url("/Bahij-Janna-Regular.ttf") format("truetype");
  }
  main {
    display: flex;
    flex-direction: column;
    height: 70vh;
    position: relative;
  }
  h1 {
    text-align: center;
    width: 100%;
    position: absolute;
    top: 1.5rem;
    font-size: 1.5rem;
  }
  h1,
  h2 {
    font-family: "Janna";
    font-weight: 400;
  }
  section {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .dateFields {
    display: flex;
    font-size: 2rem;
    font-family: monospace;
  }
  .gregorian {
    background: white;
    padding-top: 15%;
    height: 65%;
  }
  .afghan {
    background: #c2e8ce;
    height: 35%;
  }

  .convertButton-wrap {
    margin: -30px auto 0;
  }

  .convertButton {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    pointer-events: none;
    border: 3px solid #ffffff;
    background-color: #f3f3f3;
    padding: 0;
  }

  @media screen and (min-width: 769px) {
    main {
      flex-direction: row;
      height: 100vh;
    }
    .afghan,
    .gregorian {
      height: initial;
      padding: 0;
    }
    .convertButton-wrap {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translateY(-50%) translateX(-50%);
      margin: initial;
    }
    .dateFields {
      font-size: 3rem;
    }
  }
</style>

<main>
  <h1>ټول افغان نيټه اړونکی</h1>
  <section class="gregorian">
    <h2>ميلادي نيټه</h2>
    <div class="dateFields">
      <Field
        bind:value={gregYear}
        placeholder="Y"
        max={4}
        on:input={calcAfghan}
        on:next={() => gregMonthField.focus()} />
      <span>-</span>
      <Field
        bind:this={gregMonthField}
        bind:value={gregMonth}
        placeholder="M"
        max={2}
        on:input={calcAfghan}
        on:next={() => gregDayField.focus()} />
      <span>-</span>
      <Field
        bind:this={gregDayField}
        bind:value={gregDay}
        placeholder="D"
        max={2}
        on:input={calcAfghan}
        on:next={e => e.detail.$event.target.blur()} />
    </div>
  </section>

  <div class="convertButton-wrap">
    <button class="convertButton" title="اړول">
      <img src="/circular-arrows.png" width="30" alt="convert button" />
    </button>
  </div>

  <section class="afghan">
    <h2>لمریز هجري نيټه</h2>
    <div class="dateFields">
      <Field
        bind:value={afgYear}
        placeholder="Y"
        max={4}
        on:input={calcGreg}
        on:next={() => afgMonthField.focus()} />
      <span>-</span>
      <Field
        bind:this={afgMonthField}
        bind:value={afgMonth}
        placeholder="M"
        max={2}
        on:input={calcGreg}
        on:next={() => afgDayField.focus()} />
      <span>-</span>
      <Field
        bind:this={afgDayField}
        bind:value={afgDay}
        placeholder="D"
        max={2}
        on:input={calcGreg} />
    </div>
  </section>
</main>
