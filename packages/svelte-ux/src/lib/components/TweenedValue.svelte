<script lang="ts">
  import { tweened } from 'svelte/motion';
  import type { FormatType } from '../utils/format';
  import { getSettings } from './settings';

  type T = $$Generic;
  type TweenedOptions = Parameters<typeof tweened<T>>[1];

  export let value: T | 0;
  export let format: FormatType | undefined = undefined;
  export let options: TweenedOptions = undefined;
  export let disabled = false;

  const { format: formatUtil } = getSettings();
  const tweenedValue = tweened(value, options);
  $: $tweenedValue = value ?? 0;
  $: displayValue = disabled || value == null ? value : $tweenedValue;
</script>

<slot value={displayValue}>
  {#if format}
    {$formatUtil(displayValue, format)}
  {:else}
    {displayValue}
  {/if}
</slot>
