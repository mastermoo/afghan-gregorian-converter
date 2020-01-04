<script>
  import jalaali from "jalaali-js";
  import Field from "./Field.svelte";

  console.log(
    "%c Hey! You can find the source code here: https://github.com/mastermoo/afghan-gregorian-converter",
    "background: #000; color: #bada55"
  );

  const afghanMonthMapping = {
    1: "وری",
    2: "غويی",
    3: "غبرګولی",
    4: "چنګاښ",
    5: "زمری",
    6: "وږی",
    7: "تله",
    8: "لړم",
    9: "ليندۍ",
    10: "مرغومی",
    11: "سلواغه",
    12: "کب"
  };
  const afghanDigitMapping = {
    0: "۰",
    1: "١",
    2: "٢",
    3: "۳",
    4: "۴",
    5: "۵",
    6: "۶",
    7: "۷",
    8: "۸",
    9: "۹"
  };
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

  $: prettyAfghanDate = `
    ${afgYear
      .split("")
      .map(digit => afghanDigitMapping[digit])
      .join("")}
      د
    ${afghanMonthMapping[parseInt(afgMonth)]}
    ${afgDay
      .split("")
      .map(digit => afghanDigitMapping[digit])
      .join("")}
    `;

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

  :global(::-moz-selection) {
    background: rgb(146, 216, 167);
  }
  :global(::selection) {
    background: rgb(146, 216, 167);
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
  h2,
  .prettyValue {
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
  .prettyValue {
    margin-top: 1rem;
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

  footer {
    position: fixed;
    bottom: 0.5rem;
    left: 1.5rem;
    right: 1.5rem;
    font-size: 0.8rem;
    text-align: center;
  }
  footer a {
    color: inherit;
    text-decoration: underline;
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

    footer {
      text-align: right;
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
        totalDigits={4}
        on:input={calcAfghan}
        on:next={() => gregMonthField.focus()} />
      <span>-</span>
      <Field
        bind:this={gregMonthField}
        bind:value={gregMonth}
        placeholder="M"
        totalDigits={2}
        max={12}
        on:input={calcAfghan}
        on:next={() => gregDayField.focus()} />
      <span>-</span>
      <Field
        bind:this={gregDayField}
        bind:value={gregDay}
        placeholder="D"
        totalDigits={2}
        max={31}
        on:input={calcAfghan}
        on:next={e => e.detail.$event.target.blur()} />
    </div>
    <p class="prettyValue">&nbsp;</p>
  </section>

  <div class="convertButton-wrap">
    <button tabindex="-1" class="convertButton" title="اړول">
      <img src="/circular-arrows.png" width="30" alt="convert button" />
    </button>
  </div>

  <section class="afghan">
    <h2>لمریز هجري نيټه</h2>
    <div class="dateFields">
      <Field
        bind:value={afgYear}
        placeholder="Y"
        totalDigits={4}
        on:input={calcGreg}
        on:next={() => afgMonthField.focus()} />
      <span>-</span>
      <Field
        bind:this={afgMonthField}
        bind:value={afgMonth}
        placeholder="M"
        totalDigits={2}
        max={12}
        on:input={calcGreg}
        on:next={() => afgDayField.focus()} />
      <span>-</span>
      <Field
        bind:this={afgDayField}
        bind:value={afgDay}
        placeholder="D"
        totalDigits={2}
        max={32}
        on:input={calcGreg}
        on:next={e => e.detail.$event.target.blur()} />
    </div>
    <p dir="rtl" class="prettyValue">{prettyAfghanDate}</p>
  </section>
</main>

<footer>
  <p>
    Provided by
    <a href="http://tolafghan.com">Tolafghan.com</a>
  </p>
</footer>
