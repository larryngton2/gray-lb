<script lang="ts">
    import {onMount} from "svelte";
    import type {Module} from "../../../integration/types";
    import {getModules} from "../../../integration/rest";
    import {listen} from "../../../integration/ws";
    import {getTextWidth} from "../../../integration/text_measurement";
    import {flip} from "svelte/animate";
    import {fly} from "svelte/transition";
    import {convertToSpacedString, spaceSeperatedNames} from "../../../theme/theme_config";

    let enabledModules: Module[] = [];

    async function updateEnabledModules() {
        enabledModules = (await getModules())
            .filter((m) => m.enabled && !m.hidden)
            .sort(
                (a, b) =>
                    getTextWidth($spaceSeperatedNames ? convertToSpacedString(b.name) : b.name, "500 14px Inter") -
                    getTextWidth($spaceSeperatedNames ? convertToSpacedString(a.name) : a.name, "500 14px Inter"),
            );
    }

    spaceSeperatedNames.subscribe(async () => {
        await updateEnabledModules();
    });

    onMount(async () => {
        await updateEnabledModules();
    });

    listen("toggleModule", async () => {
        await updateEnabledModules();
    });

    listen("refreshArrayList", async () => {
        await updateEnabledModules();
    });
</script>

<div class="arraylist">
    {#each enabledModules as { name } (name)}
        <div class="module" animate:flip={{ duration: 200 }} in:fly={{ x: 50, duration: 200 }}>
            {$spaceSeperatedNames ? convertToSpacedString(name) : name}
        </div>
    {/each}
</div>

<style lang="scss">
    @import "../../../colors.scss";

    .arraylist {
        //position: fixed;
        //top: 0;
        //right: 0;
    }

    .module {
        background-color: rgba($arraylist-base-color, $transparency);
        color: $arraylist-text-color;
        font-size: 14px;
        //border-top-left-radius: 2px;
        //border-bottom-left-radius: 2px;
        padding-left: 6px;
        padding-top: 5px;
        padding-right: 6.5px;
        padding-bottom: 5px;
        width: max-content;
        font-weight: 500;
        margin-left: auto;
        position: relative;
        //box-shadow: -2px 0px 2px rgba(black, 0.55), 2px 0px 2px rgba(black, 0.55);
        //margin-top: 1px;
        //border-right: solid 2px rgba($accent-color, 0.5);
        //border-left: solid 2px rgba($accent-color, 1);
    }

    .module:first-child {
        border-top-right-radius: 7px;
        //border-top-left-radius: 7px;
        //box-shadow: 0px -2px 2px rgba(black, 0.45), -2px 0px 2px 0px rgba(black, 0.45), 2px 0px 2px rgba(black, 0.45);
    }

    .module:last-child {
        border-bottom-right-radius: 7px;
        //border-bottom-left-radius: 7px;
        //box-shadow: 0px 2px 2px rgba(black, 0.45), -2px 0px 2px rgba(black, 0.45), 2px 0px 2px rgba(black, 0.45);
    }
</style>