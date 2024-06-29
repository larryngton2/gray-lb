<script lang="ts">
    export let max: number;
    export let value: number;
    export let color: string;
    export let alignRight: boolean;
    export let label: string | null = null;
    export let icon: string | null = null;
</script>

<div class="progress" class:align-right={alignRight}>
    {#if label}
        <div class="label">{label}</div>
    {/if}
    {#if icon}
        <img class="icon" src="img/hud/hotbar/icon-{icon}.svg" alt={icon}>
    {/if}
    <div
            class:align-right={alignRight}
            class="progress-bar"
            style="width: {Math.floor(
            (value / max) * 100,
        )}%; background-color: {color};"
    ></div>
</div>

<style lang="scss">
  @import "../../../../colors.scss";

  .label {
    color: $text-color;
    position: absolute;
    font-size: 14px;
    right: 5px;
    top: 50%;
    transform: translateY(-50%);
  }

  .icon {
    position: absolute;
    left: 5px;
    top: 50%;
    transform: translateY(-50%);
  }

  .progress {
    position: relative;
    border-radius: 12px;
    background-color: rgba($background-color, 0.5);
    box-shadow: 0px 0px 20px rgba(black, 0.6);

    &.align-right {
      .label {
        left: 5px;
        right: unset;
      }

      .icon {
        right: 5px;
        left: unset;
      }
    }
  }

  .progress-bar {
    border-radius: 12px;
    height: 20px;
    will-change: width;
    transition: ease width 0.2s;

    &.align-right {
      margin-left: auto;
    }
  }
</style>
