<div class="button-container">
    <slot />
</div>

<style lang="scss">
    @import "../../../../colors.scss";

    .button-container {
      background-color: rgba($menu-base-color, $transparency);
      padding: 15px 30px;
      width: max-content;
      border-radius: 12px;
      display: flex;
      column-gap: 20px;
    }
</style>