<script lang="ts">
    import {listen} from "../../../../integration/ws.js";
    import type {PlayerData} from "../../../../integration/types";
    import {REST_BASE} from "../../../../integration/host";
    import {fade} from "svelte/transition";
    import HealthProgress from "./HealthProgress.svelte";
    import type {TargetChangeEvent} from "../../../../integration/events";

    let target: PlayerData | null = null;
    let visible = true;

    let hideTimeout: number;

    function startHideTimeout() {
        hideTimeout = setTimeout(() => {
            visible = false;
        }, 500);
    }

    listen("targetChange", (data: TargetChangeEvent) => {
        target = data.target;
        visible = true;
        clearTimeout(hideTimeout);
        startHideTimeout();
    });

    startHideTimeout();
</script>

{#if visible && target != null}
    <div class="targethud" transition:fade|global={{duration: 200}}>
        <div class="main-wrapper">
            <div class="avatar">
                <img src="{REST_BASE}/api/v1/client/resource/skin?uuid={target.uuid}" alt="avatar" />
            </div>
    
            <div class="name">{target.username}</div>
            <div class="health-stats">
                <div class="stat">
                    <div class="value">{Math.floor(target.actualHealth + target.absorption)}</div>
                    <img
                            class="icon"
                            src="img/hud/targethud/icon-health.svg"
                            alt="health"
                    />
                </div>
            </div>
        </div>    
        
        <HealthProgress maxHealth={target.maxHealth + target.absorption} health={target.actualHealth + target.absorption} />
    </div>
{/if}

<style lang="scss">
    @import "../../../../colors.scss";

    .targethud {
        //position: fixed;
        //top: 50%;
        //left: calc(50% + 20px);
        //transform: translateY(-50%); // overwrites the component transform
        width: 270px;
        background-color: rgba($targethud-base-color, $transparency);
        border-radius: 12px;
        overflow: hidden;
        animation: fade 0.5s;
        //box-shadow: 0px 0px 3px 3px rgba(black, 0.35);
        box-shadow: 0px 0px 2px 2px rgba(black, 0.35);
        height: 66px;
         
    }

    .main-wrapper {
        display: grid;
        grid-template-areas:
            "a b d"
            "f c e";
        padding: 7px;
    }

    .name {
        grid-area: a;
        color: $targethud-text-color;
        font-weight: 500;
        align-self: flex-start;
        padding-left: 58px;
        padding-top: 9px;
    }

    .health-stats {
        grid-area: a;
        padding-left: 228px;
        padding-top: 9px;
        //align-self: end;
        //padding-right: 11px;
        .stat {
            .value {
                color: $targethud-text-dimmed-color;
                font-size: 14px;
                min-width: 18px;
                display:inline-block;
            }
        }
    }

    .avatar {
        grid-area: a;
        height: 50px;
        width: 50px;
        position: relative;
        image-rendering: pixelated;
        background-image: url("/img/steve.png");
        background-repeat: no-repeat;
        background-size: cover;
        border-radius: 6px;
        overflow: hidden;
        padding-left: 11px;
        padding-top: 11px;

        img {
            position: absolute;
            scale: 6.25;
            left: 118px;
            top: 118px;
        }
    }
</style>