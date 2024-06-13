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
<<<<<<< HEAD
        box-shadow: -3px 0px 3px rgba(black, 0.3), 3px 0px 3px rgba(black, 0.3);
=======
        box-shadow: -15px 0px 15px rgba(black, 0.30), 15px 0px 15px rgba(black, 0.30);
>>>>>>> 14b5e7a (a?)
        //margin-top: 1px;
        //border-right: solid 2px rgba($accent-color, 0.5);
        //border-left: solid 2px rgba($accent-color, 1);
        //border-left: solid 1px rgba($border-thing, 0.5);
        //border-right: solid 1px rgba($border-thing, 0.5);
    }

    .module:first-child {
        border-top-right-radius: 7px;
        border-top-left-radius: 7px;
<<<<<<< HEAD
        //border-top: solid 1px rgba($border-thing, 0.5);
        box-shadow: 0px -3px 3px rgba(black, 0.25), -3px 0px 3px 0px rgba(black, 0.25), 3px 0px 3px rgba(black, 0.25);
=======
        border-top: solid 1px rgba($border-thing, 0.5);
        box-shadow: 0px -15px 15px rgba(black, 0.25), -15px 0px 15px 0px rgba(black, 0.25), 15px 0px 15px rgba(black, 0.25);
>>>>>>> 14b5e7a (a?)
    }

    .module:last-child {
        border-bottom-right-radius: 7px;
        border-bottom-left-radius: 7px;
<<<<<<< HEAD
        //border-bottom: solid 1px rgba($border-thing, 0.5);
        box-shadow: 0px 3px 3px rgba(black, 0.25), -3px 0px 3px rgba(black, 0.25), 3px 0px 3px rgba(black, 0.25);
=======
        border-bottom: solid 1px rgba($border-thing, 0.5);
        box-shadow: 0px 15px 15px rgba(black, 0.25), -15px 0px 15px rgba(black, 0.25), 15px 0px 15px rgba(black, 0.25);
>>>>>>> 14b5e7a (a?)
    }
</style>