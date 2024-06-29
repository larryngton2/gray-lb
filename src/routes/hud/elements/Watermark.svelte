<img class="watermark" src="img/lb-logo.svg" alt="watermark"/>
<img class="shaderfix" src="hello" alt=""/>

<style lang="scss">
    @import "../../../colors.scss";
    .watermark {
        //position: fixed;
        //top: 15px;
        //left: 15px;
        width: 200px;
        z-index: 1;
        border-radius: 12px;
        background-color: rgba($background-color, $transparency);
        box-shadow: 0px 0px 20px rgba(black, 0.6);
        padding: 5px;
    }

    .shaderfix {
        position: absolute;
        top: -15px;
        left: -15px;
        padding: 2000px;
        background-color: rgba(black, 0.1);
    }
</style>