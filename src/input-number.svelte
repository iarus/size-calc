<script>
	export {
		value,
		step,
		min,
		max
	};
	let value = null;
	let step = 1;
	let min = null;
  let max = null;

  // Определение количества знаков после запятой в переданном параметре step
	let roundCount = Math.pow(10, step.toString().includes('.') ? step.toString().split('.').pop().length : 0);

	// Уменьшение значения
	function decrement () {
		let stepL = parseFloat(step);
		let minL = parseFloat(min);
		let valueL = parseFloat(value);
		if (Number.isNaN(valueL)) {
			valueL = 0;
		}
		valueL = valueL - stepL;
		if (valueL < minL && !Number.isNaN(minL) && typeof minL == 'number' ) {
			valueL = minL;
		}
		value = Math.round(valueL * roundCount) / roundCount;
	};

	// Увеличение значения
	function increment () {
		let stepL = parseFloat(step);
		let maxL = parseFloat(max);
		let valueL = parseFloat(value);
		if (Number.isNaN(valueL)) {
			valueL = 0;
		}
		valueL = valueL + stepL;
		if (valueL > maxL && !Number.isNaN(maxL) && typeof maxL == 'number' ) {
			valueL = maxL;
		}
		value = Math.round(valueL * roundCount) / roundCount;
	};
</script>

<div class="input_number {$$props.class}">
  <a class="input_number-btn __dec" href="#dec" on:click|preventDefault={decrement} tabindex="-1">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 298.7 298.7">
      <path d="M0 128h299v43H0z"/>
    </svg>
  </a>
	<input class="input_number-input __number" type="number" bind:value={value} {step} {min} {max}>
	<a class="input_number-btn __inc" href="#inc" on:click|preventDefault={increment} tabindex="-1">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 298.7 298.7">
      <path d="M171 128V0h-43v128H0v43h128v128h43V171h128v-43z"/>
    </svg>
  </a>
</div>

<style type="text/scss">
  .input_number {
    position: relative;
    z-index: 0;
    min-height: 1em;

    &-input {
      padding-left: 2.5em;
      padding-right: 2.5em;
      width: 100%;
      text-align: center;
      -webkit-appearance: textfield;
      -moz-appearance: textfield;

      &::-webkit-outer-spin-button,
      &::-webkit-inner-spin-button {
        -webkit-appearance: none;
      }
    }

    &-btn {
      position: absolute;
      z-index: 1;
      color: transparent;
      text-decoration: none;
      top: 0;
      width: 2.5em;
      height: 100%;
      text-indent: 101%;
      overflow: hidden;
      fill: #3c3c3e;
      transition: 0.3s ease;

      &:hover {
        background-color: rgba(0, 0, 0, 0.08);
        fill: #000;
      }
      &:focus {
        outline: none;
      }
      &.__dec {
        left: 0;
      }
      &.__inc {
        right: 0;
      }
      > svg {
        position: absolute;
        width: 0.8em;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
      }
    }
  }
</style>