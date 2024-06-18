<script lang="ts">
    export let maxHealth: number;
    export let health: number;

    let width = Math.ceil(health / maxHealth * 100);
    $: width = Math.ceil(health / maxHealth * 100);
</script>

<div class="health-progress">
    <div class="thumb" style="width: {width}%;"></div>
</div>

<style lang="scss">
    @import "../../../../colors.scss";

    .health-progress {                   /*edited from riselb*/
        position: relative;
        bottom: 23px;
        left: 65px;
        border-radius: 6px;
        max-width: 70.5%;
        background-color: rgba($arraylist-base-color, 0.3);
    }

    .thumb {
        height: 10px;
        background-color: $accent-color;
        transition: ease width 0.5s;
        border-radius: 6px;
    }    
</style>