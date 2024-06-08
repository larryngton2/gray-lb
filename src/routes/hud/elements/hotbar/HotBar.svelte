<script lang="ts">
    import Status from "./Status.svelte";
    import {listen} from "../../../../integration/ws";
    import type {PlayerData, TextComponent as TTExtComponent} from "../../../../integration/types";
    import {onMount} from "svelte";
    import {getPlayerData} from "../../../../integration/rest";
    import {fade} from "svelte/transition";
    import TextComponent from "../../../menu/common/TextComponent.svelte";
    import type {ClientPlayerDataEvent, OverlayMessageEvent} from "../../../../integration/events";

    let lastSlot = 0;
    let currentSlot = 0;
    let playerData: PlayerData | null = null;
    let maxAbsorption = 0;
    let slotsElement: HTMLElement | undefined;

    let showItemStackName = false;
    let showItemStackNameTimeout: number | null = null;
    let itemStackName: TTExtComponent | string | null = null;
    let overlayMessage: OverlayMessageEvent | null = null;
    let overlayMessageTimeout: number | null = null;

    function updatePlayerData(s: PlayerData) {
        playerData = s;
        if (playerData.absorption <= 0) {
            maxAbsorption = 0;
        }
        if (playerData.absorption > maxAbsorption) {
            maxAbsorption = playerData.absorption;
        }
        currentSlot = playerData.selectedSlot;
        if (currentSlot !== lastSlot) {
            lastSlot = currentSlot;
            if (playerData.mainHandStack.identifier !== "minecraft:air") {
                itemStackName = playerData.mainHandStack.displayName;
                if (showItemStackNameTimeout !== null) {
                    clearTimeout(showItemStackNameTimeout);
                }
                showItemStackName = true;
                showItemStackNameTimeout = setTimeout(() => {
                    showItemStackName = false;
                }, 2000);
            }
        }
    }

    listen("clientPlayerData", (event: ClientPlayerDataEvent) => {
        updatePlayerData(event.playerData);
    });

    listen("overlayMessage", (event: OverlayMessageEvent) => {
        overlayMessage = event;
        if (overlayMessageTimeout !== null) {
            clearTimeout(overlayMessageTimeout);
        }
        overlayMessageTimeout = setTimeout(() => {
            overlayMessage = null;
        }, 3000)
    });

    onMount(async () => {
        updatePlayerData(await getPlayerData());
    });
</script>

{#if playerData && playerData.gameMode !== "spectator"}
    <div class="hotbar">
        {#if overlayMessage !== null}
            <div class="overlay-message" out:fade={{duration: 200}}
                 style="max-width: {slotsElement?.offsetWidth ?? 0}px">
                <TextComponent fontSize={14} textComponent={overlayMessage.text}/>
            </div>
        {/if}
        {#if showItemStackName && itemStackName !== null}
            <div class="item-name" out:fade={{duration: 200}}>
                <TextComponent fontSize={14} textComponent={itemStackName}/>
            </div>
        {/if}
        <div class="status">

            <div class="pair">
                {#if playerData.armor > 0}
                    <Status
                            max={20}
                            value={playerData.armor}
                            color="#3b3b3b"
                            alignRight={false}
                            icon="shield"
                    />
                {:else}
                    <div></div>
                {/if}

                {#if playerData.air < playerData.maxAir}
                    <Status
                            max={playerData.maxAir}
                            value={playerData.air}
                            color="#3b3b3b"
                            alignRight={true}
                    />
                {:else}
                    <div></div>
                {/if}
            </div>

            {#if playerData.gameMode !== "creative"}
                {#if playerData.absorption > 0}
                    <div class="pair">
                        <Status
                                max={maxAbsorption}
                                value={playerData.absorption}
                                color="#3b3b3b"
                                alignRight={false}
                        />

                        <div></div>
                    </div>
                {/if}
                <div class="pair">
                    <Status
                            max={playerData.maxHealth}
                            value={playerData.health}
                            color="#2e2e2e"
                            alignRight={false}
                            icon="heart"
                    />
                    <Status
                            max={20}
                            value={playerData.food}
                            color="#2e2e2e"
                            alignRight={true}
                            icon="food"
                    />
                </div>
            {/if}
            {#if playerData.experienceLevel > 0}
                <Status
                        max={100} value={playerData.experienceProgress * 100}
                        color="#2e2e2e"
                        alignRight={false}
                        label={playerData.experienceLevel.toString()}
                />
            {/if}
        </div>

        <div class="hotbar-elements">
            <div class="slider" style="left: {currentSlot * 45}px"></div>
            <div class="slots" bind:this={slotsElement}>
                <div class="slot"></div>
                <div class="slot"></div>
                <div class="slot"></div>
                <div class="slot"></div>
                <div class="slot"></div>
                <div class="slot"></div>
                <div class="slot"></div>
                <div class="slot"></div>
                <div class="slot"></div>
            </div>
        </div>

        {#if playerData?.offHandStack.identifier !== "minecraft:air"}
            <div class="offhand-slot"></div>
        {/if}
    </div>
{/if}

<style lang="scss">
  @import "../../../../colors.scss";

  .hotbar {
    //position: fixed;
    //bottom: 15px;
    //left: 50%;
    //transform: translateX(-50%);
  }

  .pair {
    display: grid;
    grid-template-columns: 1fr 1fr;
    column-gap: 25px;
  }

  .status {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
    row-gap: 5px;
    column-gap: 20px;
  }

  .hotbar-elements {
    background-color: rgba($hotbar-base-color, $transparency);
    position: relative;
    border-radius: 12px;
    overflow: hidden;
    //box-shadow: 0px 0px 2px 2px rgba(black, 0.55);
    border: solid 1px rgba($border-thing, 0.5);
    
    .slider {
      //border: solid 2px $accent-color;
      background-color: rgba($accent-color, 0.6);
      height: 45px;
      width: 45px;
      //padding: 10px;
      padding-left: 10px;
      position: absolute;
      border-radius: 12px;
      transition: ease left 0.1s;
    }

    .slots {
      display: flex;
    }

    .slot {
      height: 45px;
      width: 45px;
    }
  }

  .offhand-slot {
    height: 45px;
    width: 45px;
    border-radius: 12px;
    background-color: rgba($hotbar-base-color, $transparency);
    position: absolute;
    bottom: 0;
    left: -65px;
    //box-shadow: 0px 0px 4px 4px rgba(black, $transparency);
  }

  .item-name {
    color: $hotbar-text-color;
    font-size: 14px;
    margin: 0 auto 15px;
    font-weight: 500;
    background-color: rgba($hotbar-base-color, $transparency);
    padding: 5px 8px;
    border-radius: 6px;
    width: max-content;
    //box-shadow: 0px 0px 4px 4px rgba(black, $transparency);
  }

  .overlay-message {
    text-align: center;
    color: $hotbar-text-color;
    margin-bottom: 15px;
    overflow: hidden;
  }
</style>
