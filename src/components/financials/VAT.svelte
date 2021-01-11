<script lang="ts">
  const vatRates: Array<{ label: string; rate: Number; default?: boolean }> = [
    { label: "10%", rate: 0.1 },
    { label: "14%", rate: 0.14, default: true },
    { label: "24%", rate: 0.24 },
  ];

  let price = 0;
  let rate = 0.24;
  let vat = 0;
  let total = 0;

  const round = (num: number) => Math.round(num * 100) / 100;

  const calcPrice = function (totalSum: number, vatRate: number): number {
    return round(total / (1 + vatRate));
  };

  const calcTotal = function (price: number, vatRate: number): number {
    return round(price + price * vatRate);
  };

  const updatePrice = function (event) {
    price = Number(event.currentTarget.value);
    total = calcTotal(price, rate);
    vat = round(total - price);
  };
  const updateRate = function (event) {
    rate = Number(event.currentTarget.value);
    total = calcTotal(price, rate);
    price = calcPrice(total, rate);
    vat = round(total - price);
  };
  const updateTotal = function (event) {
    total = Number(event.currentTarget.value);
    price = calcPrice(total, rate);
    vat = round(total - price);
  };
</script>

<div>

  <div>
    <label for="price">Price without VAT</label>
    <input
      id="price"
      type="number"
      min="0"
      step="any"
      on:input="{updatePrice}"
      bind:value="{price}" />
  </div>

  <div>
    <label for="rate">VAT rate</label>
    <!-- svelte-ignore a11y-no-onchange -->
    <select bind:value="{rate}" on:change="{updateRate}">
      {#each vatRates as { label, rate }}
        <option value="{rate}">{label}</option>
      {/each}
    </select>
  </div>

  <div>
    <label for="years">VAT Amount</label>
    <input id="years" type="number" bind:value="{vat}" disabled />
  </div>

  <div>
    <label for="total">Total</label>
    <input
      id="total"
      type="number"
      min="0"
      step="any"
      on:input="{updateTotal}"
      bind:value="{total}" />
  </div>
</div>

<style>
  div {
    display: block;
  }

  input[type="number"]:disabled {
    background: #fcfcfc;
  }
</style>
