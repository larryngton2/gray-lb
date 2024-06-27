<img class="watermark" src="img/lb-logo.svg" alt="watermark">
<img class="shaderfix" src="hello" alt="a">

<style lang="scss">
    @import "../../colors.scss";

    .watermark {
        position: absolute;
        bottom: 120px;
        left: 45%;
        width: 200px;
        background-color: rgba($arraylist-base-color, $transparency);
        border-radius: 12px;
        box-shadow: 0px 0px 20px rgba(black, 0.6);
        padding: 5px
    }

    .shaderfix {
        position: absolute;
        top: 0px;
        left: 0px;
        padding: 2000px;
        background-color: rgba(black, 0.1);
    }
</style>