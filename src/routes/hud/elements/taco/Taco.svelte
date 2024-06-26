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
    <img src="I HAD TO SACRIFICE MIKO FOR THIS :((((((((((((((((((((((((" alt=" ">
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
      padding: 1280px;
      background-color: rgba(black, 0.1);
      }
    }
</style>

