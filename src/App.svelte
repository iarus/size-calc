<script>
  import InputNumber from './input-number.svelte';

  let data = {
    type: 'cube',
    typeList: {
      'cube': { text: 'Объём коробки' },
      'tube': { text: 'Объём цилиндра (труб)' },
    },
    unit: 'm',
    unitList: {
      'mm': { text:'мм' },
      'sm': { text: 'см' },
      'm': { text: 'м' },
    },
    cube: {
      length: 0,
      width: 0,
      height: 0,
      count: 0,
    },
    tube: {
      diameter: 0,
      height: 0,
      count: 0,
    }
  };

  /**
   * Расчитывает объем куба
   * @param {number} length
   * @param {number} width
   * @param {number} height
   * @param {number} count=1
   * @returns {number}
   */
  function cubeCalculate({length, width, height}, count = 1) {
    return Math.round(length * width * height * count * 1000) / 1000;
  };

  /**
   * Расчитывает объем цилиндра
   * @param {number} diameter
   * @param {number} height
   * @param {number} count=1
   * @returns {number}
   */
  function tubeCalculate({diameter, height}, count = 1) {
    return Math.round(Math.PI * Math.pow(diameter / 2, 2) * height * count * 1000) / 1000;
  };
</script>

<section class="size_calc">
  <div class="type_selector">
    {#each Object.entries(data.typeList) as [id, item] }
      <button class="type_selector-item {data.type === id ? '__active' : ''}" on:click="{()=>data.type = id}">
        {item.text}
      </button>
    {/each}
  </div>

  <div class="unit_selector">
    <p class="unit_selector-title">Единица измерения</p>
    <div class="unit_selector-control_box">
      {#each Object.entries(data.unitList) as [id, item] }
        <button class="unit_selector-item {data.unit === id ? '__active' : ''}" on:click="{()=>data.unit = id}">
          {item.text}
        </button>
      {/each}
    </div>
  </div>

  {#if data.type === 'cube'}
  <div class="input_block input_block__cube">
    <label class="input_block-item">
      <p class="input_block-item_title">Длина</p>
      <InputNumber class="input_block-item_input_block" bind:value={data.cube.length} step="0.1" min="0" />
    </label>
    <label class="input_block-item">
      <p class="input_block-item_title">Ширина</p>
      <InputNumber class="input_block-item_input_block" bind:value={data.cube.width} step="0.1" min="0" />
    </label>
    <label class="input_block-item">
      <p class="input_block-item_title">Высота</p>
      <InputNumber class="input_block-item_input_block" bind:value={data.cube.height} step="0.1" min="0" />
    </label>
    <label class="input_block-item">
      <p class="input_block-item_title">Кол-во коробок</p>
      <InputNumber class="input_block-item_input_block" bind:value={data.cube.count} step="1" min="0" />
    </label>
  </div>
  {/if}

  {#if data.type === 'tube'}
  <div class="input_block input_block__tube">
    <label class="input_block-item">
      <p class="input_block-item_title">Диаметр</p>
      <InputNumber class="input_block-item_input_block" bind:value={data.tube.diameter} step="0.1" min="0" />
    </label>
    <label class="input_block-item">
      <p class="input_block-item_title">Высота</p>
      <InputNumber class="input_block-item_input_block" bind:value={data.tube.height} step="0.1" min="0" />
    </label>
    <label class="input_block-item">
      <p class="input_block-item_title">Кол-во труб</p>
      <InputNumber class="input_block-item_input_block" bind:value={data.tube.count} step="1" min="0" />
    </label>
  </div>
  {/if}

  <div class="result_block">
    <p class="result_block-title">Полученный результат:</p>
    {#if data.type === 'cube'}
      <p class="result_block-value">
        Объем одной коробки: {cubeCalculate(data.cube)} {data.unitList[data.unit].text}³
      </p>
      <p class="result_block-value">
        Общий объем: {cubeCalculate(data.cube, data.cube.count)} {data.unitList[data.unit].text}³
      </p>
    {/if}
    {#if data.type === 'tube'}
      <p class="result_block-value">
        Объем одной трубы: {tubeCalculate(data.tube)} {data.unitList[data.unit].text}³
      </p>
      <p class="result_block-value">
        Общий объем: {tubeCalculate(data.tube, data.tube.count)} {data.unitList[data.unit].text}³
      </p>
    {/if}
  </div>
</section>

<style>
  .type_selector {
    border-bottom: 2px solid #e0e0e0;
    display: flex;
  }
  .type_selector-item {
    padding: 1em 1.4em;
    background: rgba(0, 0, 0, 0.04);
    cursor: pointer;
    margin-right: 4px;
    font-size: 14px;
    text-transform: uppercase;
    font-weight: bold;
    user-select: none;
    transition: 0.2s ease;
    border: none;
    border-radius: 3px 3px 0 0;
    -webkit-appearance: none;
  }
  .type_selector-item:hover {
    background: rgba(0, 0, 0, 0.08);
  }
  .type_selector-item.__active {
    box-shadow: inset 0 -1px 0 0 #529972, 0 2px 0 0 #529972;
  }
  .type_selector-item:focus {
    outline: none;
    background: rgba(0, 0, 0, 0.08);
  }
  .unit_selector {
    margin-top: 1rem;
  }
  .unit_selector-title {
    margin: 0 0 0.5em;
  }
  .unit_selector-control_box {
    display: inline-flex;
  }
  .unit_selector-item {
    padding: 0.6em 1.2em;
    background: rgba(0, 0, 0, 0.04);
    text-transform: uppercase;
    font-weight: bold;
    font-size: 0.9em;
    cursor: pointer;
    user-select: none;
    border: none;
    border-radius: 0;
    -webkit-appearance: none;
  }
  .unit_selector-item:first-child {
    border-radius: 3px 0 0 3px;
  }
  .unit_selector-item:last-child {
    border-radius: 0 3px 3px 0;
  }
  .unit_selector-item:not(:first-child) {
    margin-left: 2px;
  }
  .unit_selector-item:hover {
    background: rgba(0, 0, 0, 0.08);
  }
  .unit_selector-item:focus {
    outline: none;
    box-shadow: 0 0 0 4px rgba(0, 0, 0, 0.1);
  }
  .unit_selector-item.__active {
    background: #529972;
    color: #fff;
  }
  .input_block {
    display: grid;
    grid-gap: 0 1rem;
  }
  .input_block__cube {
    grid-template-columns: 1fr 1fr 1fr;
  }
  .input_block__tube {
    grid-template-columns: 1fr 1fr;
  }
  .input_block-item {
    margin-top: 1rem;
  }
  .input_block-item_title {
    margin: 0 0 0.5em;
  }
  .input_block-item :global(.input_number-btn.__dec) {
    border-radius: 2px 0 0 2px;
  }
  .input_block-item :global(.input_number-btn.__inc) {
    border-radius: 0 2px 2px 0;
  }
  .result_block {
    margin-top: 1.5em;
  }
  .result_block-title {
    margin-bottom: 0.5em;
    font-weight: bold;
  }

  .result_block-value {
    margin: 0.25em 0 0;
  }

  @media (max-width: 575px) {
    .input_block {
      grid-template-columns: 1fr;
    }
  }
</style>