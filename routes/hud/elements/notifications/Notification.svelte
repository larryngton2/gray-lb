<script lang="ts">
    export let title: string;
    export let message: string;
    export let severity: string;
</script>

<div class="notification">
    <div class="icon {severity.toString().toLowerCase()}"/>
    <div class="title">{title}</div>
    <div class="message">{message}</div>
</div>

<style lang="scss">
  @import "../../../../colors.scss";
  
  .notification {
    min-width: 170px;
    display: grid;
    grid-template-areas:
            "a b"
            "a c";
    grid-template-columns: max-content 1fr;
    column-gap: 10px;
    background: rgba(0, 0, 0, $transparency);
    border-radius: 12px;
    overflow: hidden;
    padding-right: 12px;
    padding-top: 6px;
    padding-bottom: 6px;
    padding-left: 6px;
    margin-bottom: 10px;
    box-shadow: 0px 0px 4px 4px rgba($accent-color, $transparency);
  }

  .icon {
    height: 40px;
    width: 40px;
    background-position: center;
    background-repeat: no-repeat;
    border-radius: 6px;
    grid-area: a;
    transition: background-color 0.2s;
    position: relative;

    &.success {
      background-color: #3b3b3b;
      background-image: url("/img/hud/notification/icon-success.svg");
    }

    &.error {
      background-color: #3b3b3b;
      background-image: url("/img/hud/notification/icon-error.svg");
    }

    &.info {
      background-color: #3b3b3b;
      background-image: url("/img/hud/notification/icon-info.svg");
    }

    &.disabled,
    &.enabled {
      &::after {
        content: "";
        position: absolute;
        height: 10px;
        width: 10px;
        border-radius: 6px;
        top: 50%;
        transform: translate(-50%, -50%);
        background: white;
        transition: all 0.2s ease;
      }

      background-image: url("/img/hud/notification/icon-toggle.svg");
    }

    &.enabled {
      background-color: #3b3b3b;

      &::after {
        left: 62%;
      }
    }

    &.disabled {
      background-color: #3b3b3b;

      &::after {
        left: 38%;
      }
    }
  }

  .title {
    grid-area: b;
    font-size: 14px;
    color: white;
    font-weight: 600;
  }

  .message {
    grid-area: c;
    font-size: 12px;
    color: #cbd1e3;
  }
</style>