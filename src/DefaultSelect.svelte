<script>
  export let container;
  export let input;
  export let containerClasses;
  export let containerStyles;
  export let hasError;
  export let isMulti;
  export let isDisabled;
  export let isFocused;
  export let handleClick;
  export let Icon;
  export let iconProps;
  export let selectedValue;
  export let MultiSelection;
  export let getSelectionLabel;
  export let activeSelectedValue;
  export let multiFullItemClearable;
  export let handleMultiItemClear;
  export let handleFocus;
  export let _inputAttributes;
  export let filterText;
  export let placeholderText;
  export let inputStyles;
  export let showSelectedItem;
  export let isWaiting;
  export let handleClear;
  export let ClearIcon;
  export let isClearable;
  export let showIndicator;
  export let showChevron;
  export let isSearchable;
  export let indicatorSvg;
  export let Selection;
</script>

<div
  class="selectContainer {containerClasses}"
  class:hasError
  class:multiSelect={isMulti}
  class:disabled={isDisabled}
  class:focused={isFocused}
  style={containerStyles}
  on:click={handleClick}
  bind:this={container}
>
  {#if Icon}
    <svelte:component this={Icon} {...iconProps} />
  {/if}

  {#if isMulti && selectedValue && selectedValue.length > 0}
    <svelte:component
      this={MultiSelection}
      {selectedValue}
      {getSelectionLabel}
      {activeSelectedValue}
      {isDisabled}
      {multiFullItemClearable}
      on:multiItemClear={handleMultiItemClear}
      on:focus={handleFocus}
    />
  {/if}

  {#if isDisabled}
    <input
      {..._inputAttributes}
      bind:this={input}
      on:focus={handleFocus}
      bind:value={filterText}
      placeholder={placeholderText}
      style={inputStyles}
      disabled
    />
  {:else}
    <input
      {..._inputAttributes}
      bind:this={input}
      on:focus={handleFocus}
      bind:value={filterText}
      placeholder={placeholderText}
      style={inputStyles}
    />
  {/if}

  {#if !isMulti && showSelectedItem}
    <div class="selectedItem" on:focus={handleFocus}>
      <svelte:component
        this={Selection}
        item={selectedValue}
        {getSelectionLabel}
      />
    </div>
  {/if}

  {#if showSelectedItem && isClearable && !isDisabled && !isWaiting}
    <div class="clearSelect" on:click|preventDefault={handleClear}>
      <svelte:component this={ClearIcon} />
    </div>
  {/if}

  {#if showIndicator || (showChevron && !selectedValue) || (!isSearchable && !isDisabled && !isWaiting && ((showSelectedItem && !isClearable) || !showSelectedItem))}
    <div class="indicator">
      {#if indicatorSvg}
        {@html indicatorSvg}
      {:else}
        <svg width="100%" height="100%" viewBox="0 0 20 20" focusable="false">
          <path
            d="M4.516 7.548c0.436-0.446 1.043-0.481 1.576 0l3.908 3.747
            3.908-3.747c0.533-0.481 1.141-0.446 1.574 0 0.436 0.445 0.408 1.197 0
            1.615-0.406 0.418-4.695 4.502-4.695 4.502-0.217 0.223-0.502
            0.335-0.787 0.335s-0.57-0.112-0.789-0.335c0
            0-4.287-4.084-4.695-4.502s-0.436-1.17 0-1.615z"
          />
        </svg>
      {/if}
    </div>
  {/if}

  {#if isWaiting}
    <div class="spinner">
      <svg class="spinner_icon" viewBox="25 25 50 50">
        <circle
          class="spinner_path"
          cx="50"
          cy="50"
          r="20"
          fill="none"
          stroke="currentColor"
          stroke-width="5"
          stroke-miterlimit="10"
        />
      </svg>
    </div>
  {/if}
</div>

<style>
  .selectContainer {
    --padding: 0 16px;

    border: var(--border, 1px solid #d8dbdf);
    border-radius: var(--borderRadius, 3px);
    height: var(--height, 42px);
    position: relative;
    display: flex;
    align-items: center;
    padding: var(--padding);
    background: var(--background, #fff);
  }

  .selectContainer input {
    cursor: default;
    border: none;
    color: var(--inputColor, #3f4f5f);
    height: var(--height, 42px);
    line-height: var(--height, 42px);
    padding: var(--inputPadding, var(--padding));
    width: 100%;
    background: transparent;
    font-size: var(--inputFontSize, 14px);
    letter-spacing: var(--inputLetterSpacing, -0.08px);
    position: absolute;
    left: var(--inputLeft, 0);
  }

  .selectContainer input::placeholder {
    color: var(--placeholderColor, #78848f);
    opacity: var(--placeholderOpacity, 1);
  }

  .selectContainer input:focus {
    outline: none;
  }

  .selectContainer:hover {
    border-color: var(--borderHoverColor, #b2b8bf);
  }

  .selectContainer.focused {
    border-color: var(--borderFocusColor, #006fe8);
  }

  .selectContainer.disabled {
    background: var(--disabledBackground, #ebedef);
    border-color: var(--disabledBorderColor, #ebedef);
    color: var(--disabledColor, #c1c6cc);
  }

  .selectContainer.disabled input::placeholder {
    color: var(--disabledPlaceholderColor, #c1c6cc);
    opacity: var(--disabledPlaceholderOpacity, 1);
  }

  .selectedItem {
    line-height: var(--height, 42px);
    height: var(--height, 42px);
    overflow-x: hidden;
    padding: var(--selectedItemPadding, 0 20px 0 0);
  }

  .selectedItem:focus {
    outline: none;
  }

  .clearSelect {
    position: absolute;
    right: var(--clearSelectRight, 10px);
    top: var(--clearSelectTop, 11px);
    bottom: var(--clearSelectBottom, 11px);
    width: var(--clearSelectWidth, 20px);
    color: var(--clearSelectColor, #c5cacf);
    flex: none !important;
  }

  .clearSelect:hover {
    color: var(--clearSelectHoverColor, #2c3e50);
  }

  .selectContainer.focused .clearSelect {
    color: var(--clearSelectFocusColor, #3f4f5f);
  }

  .indicator {
    position: absolute;
    right: var(--indicatorRight, 10px);
    top: var(--indicatorTop, 11px);
    width: var(--indicatorWidth, 20px);
    height: var(--indicatorHeight, 20px);
    color: var(--indicatorColor, #c5cacf);
  }

  .indicator svg {
    display: inline-block;
    fill: var(--indicatorFill, currentcolor);
    line-height: 1;
    stroke: var(--indicatorStroke, currentcolor);
    stroke-width: 0;
  }

  .spinner {
    position: absolute;
    right: var(--spinnerRight, 10px);
    top: var(--spinnerLeft, 11px);
    width: var(--spinnerWidth, 20px);
    height: var(--spinnerHeight, 20px);
    color: var(--spinnerColor, #51ce6c);
    animation: rotate 0.75s linear infinite;
  }

  .spinner_icon {
    display: block;
    height: 100%;
    transform-origin: center center;
    width: 100%;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    -webkit-transform: none;
  }

  .spinner_path {
    stroke-dasharray: 90;
    stroke-linecap: round;
  }

  .multiSelect {
    display: flex;
    padding: var(--multiSelectPadding, 0 35px 0 16px);
    height: auto;
    flex-wrap: wrap;
    align-items: stretch;
  }

  .multiSelect > * {
    flex: 1 1 50px;
  }

  .selectContainer.multiSelect input {
    padding: var(--multiSelectInputPadding, 0);
    position: relative;
    margin: var(--multiSelectInputMargin, 0);
  }

  .hasError {
    border: var(--errorBorder, 1px solid #ff2d55);
    background: var(--errorBackground, #fff);
  }

  @keyframes rotate {
    100% {
      transform: rotate(360deg);
    }
  }
</style>
