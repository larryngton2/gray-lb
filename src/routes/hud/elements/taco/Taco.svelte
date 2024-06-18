<script lang="ts">
    let left = -100;
    setInterval(() => {
        left += 100;

        if (left > window.innerWidth + 100) {
            left = -100;
        }
    }, 500);
</script>

<div class="taco">
    <img src="img/hud/miko/miko.gif" alt="miko no load :((">
</div>

<style lang="scss">
  @import "../../../../colors.scss";

  .taco {
    position: relative;
    height: 50px;
    border-radius: 6px;

    img {
      position: absolute;
      height: 100%;
      padding: 5px;
      background-color: rgba($clickgui-base-color, $transparency);
      border-radius: 12px;
      box-shadow: 0px 0px 2px 2px rgba(black, 0.35);
      }
    }
</style>

