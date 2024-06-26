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
        <div class="module"  animate:flip={{ duration: 100 }} in:fly={{ x: 50, duration: 100 }} out:fly={{ x: 50, duration: 100 }}>
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
        border-top-left-radius: 4px;
        border-bottom-left-radius: 4px;
        padding-left: 8px;
        padding-top: 5px;
        padding-right: 8px;
        padding-bottom: 5px;
        width: max-content;
        font-weight: 500;
        margin-left: auto;
        position: relative;
        box-shadow: -15px 0px 15px rgba(black, 0.2), 15px 0px 15px rgba(black, 0.2);
    }

    .module:first-child {
        border-top-right-radius: 7px;
        border-top-left-radius: 7px;
        box-shadow: 0px -15px 15px rgba(black, 0.15), -15px 0px 15px 0px rgba(black, 0.15), 15px 0px 15px rgba(black, 0.15);
    }

    .module:last-child {
        border-bottom-right-radius: 7px;
        border-bottom-left-radius: 7px;
        box-shadow: 0px 15px 15px rgba(black, 0.15), -15px 0px 15px rgba(black, 0.15), 15px 0px 15px rgba(black, 0.15);
    }
</style>
