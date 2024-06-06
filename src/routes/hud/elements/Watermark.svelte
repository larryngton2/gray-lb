<img class="watermark" src="img/lb-logo.svg" alt="watermark" />

<style lang="scss">
    @import "../../../colors.scss";
    .watermark {
        //position: fixed;
        //top: 15px;
        //left: 15px;
        width: 170px;
        z-index: 1;
        border-radius: 12px;
        //background-color: rgba($arraylist-base-color, $transparency);
        //box-shadow: 0px 0px 4px 4px rgba($accent-color, 0.55);
        padding: 6px;
    }

    .watermark::backdrop { 
          line-height: 1.8;
          background: rgba(204, 204, 204, 0.0);
          padding-right: 4%;
    }
</style>