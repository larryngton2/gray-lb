<script lang="ts">
    import {listen} from "../../../integration/ws";
    import type {PlayerData, Scoreboard} from "../../../integration/types";
    import TextComponent from "../../menu/common/TextComponent.svelte";
    import type {ClientPlayerDataEvent} from "../../../integration/events";

    let scoreboard: Scoreboard | null = null;

    listen("clientPlayerData", (e: ClientPlayerDataEvent) => {
        const playerData: PlayerData = e.playerData;
        scoreboard = playerData.scoreboard;
    });
</script>

{#if scoreboard}
    <div class="scoreboard">
        {#if scoreboard.header}
            <div class="header">
                <TextComponent fontSize={14} allowPreformatting={true} textComponent={scoreboard.header}/>
            </div>
        {/if}
        <div class="entries">
            {#each scoreboard.entries as {name, score}}
                <div class="row">
                    <TextComponent fontSize={14} allowPreformatting={true} textComponent={name}/>
                    <TextComponent fontSize={14} allowPreformatting={true} textComponent={score}/>
                </div>
            {/each}
        </div>
    </div>
{/if}

<style lang="scss">
  @import "../../../colors.scss";

  .scoreboard {
    width: max-content;
    //position: fixed;
    //left: 15px;
    //top: 550px;
    border-radius: 12px;
    overflow: hidden;
    font-size: 14px;
    box-shadow: 0px 0px 20px 20px rgba(black, 0.25);
     
    filter: grayscale(1);
  }

  .entries {
    background-color: rgba($scoreboard-base-color, $transparency);
    padding: 10px;
    font-family: Arial, Helvetica, sans-serif;
  }

  .row {
    display: flex;
    column-gap: 15px;
    justify-content: space-between;
    font-family: Arial, Helvetica, sans-serif;
  }

  .header {
    text-align: center;
    background-color: rgba($scoreboard-base-color, $transparency);
    padding: 7px 10px;
    font-weight: 600;
    font-family: Arial, Helvetica, sans-serif;
  }
</style>
