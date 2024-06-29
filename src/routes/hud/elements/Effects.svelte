<script lang="ts">
    import {listen} from "../../../integration/ws";
    import type {ClientPlayerDataEvent} from "../../../integration/events";
    import type {StatusEffect} from "../../../integration/types";
    import {fade} from "svelte/transition";

    let effects: StatusEffect[] = [];

    listen("clientPlayerData", (event: ClientPlayerDataEvent) => {
        effects = event.playerData.effects;
    });

    function formatTime(duration: number): string {
        return new Date(((duration / 20) | 0) * 1000).toISOString().substring(14, 19);
    }

    function convertToRoman(n: number): string {
        switch (n) {
            case 0: return "1";
            case 1: return "2";
            case 2: return "3";
            case 3: return "4";
            case 4: return "5";
            case 5: return "6";
            case 6: return "7";
            case 7: return "8";
            case 8: return "9";
            case 9: return "10";
            default: return "10+";
        }
    }
</script>

<div class="effects">
    {#each effects as e}
        <div class="effect" transition:fade|global={{duration: 200}}>
            <span class="name" style="color: {'#' + e.color.toString(16)}">{e.localizedName} {convertToRoman(e.amplifier)}:</span>
            <span class="duration">{formatTime(e.duration)}</span>
        </div>
    {/each}
</div>

<style lang="scss">
  @import "../../../colors.scss";

  .effect {
    font-weight: 500;
    font-size: 14px;
    text-align: left;
    background-color: rgba($background-color, $transparency);
    padding: 5px 8px;
    box-shadow: 0px 0px 20px rgba(black, 0.6);

    .duration {
      color: white;
    }
  }

  .effect:first-child {
    border-top-left-radius: 12px;
    border-top-right-radius: 12px;
  }

  .effect:last-child {
    border-bottom-left-radius: 12px;
    border-bottom-right-radius: 12px;
  }
</style>