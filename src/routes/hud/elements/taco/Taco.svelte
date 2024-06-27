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
    <img class="miko" src="img/hud/miko/miko.gif" alt="miko"/>
</div>

<style lang="scss">
  @import "../../../../colors.scss";

  .taco {
    position: relative;

    img {
      position: absolute;
      padding: 5px;
      background-color: rgba($arraylist-base-color, $transparency);
      height: 50px;
      border-radius: 12px;
      box-shadow: 0px 0px 20px rgba(black, 0.6);
      }
    }
</style>

