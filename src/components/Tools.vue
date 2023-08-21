<template>
    <div class="panel">
            <span v-on:click="changeTool('POINTER')"><i class="gg-arrow-top-left" :class="{'active': status == 'PLAY'}"></i></span>
            <span v-on:click="changeTool('PENCIL')"><i class="gg-pen" :class="{'active': status == 'PAUSE'}"></i></span>
            <span v-on:click="changeTool('LINE')"><i class="gg-border-style-solid" :class="{'active': status == 'LINE'}"></i></span>
            <span v-on:click="changeTool('ERASER')"><i class="gg-erase" :class="{'active': status == 'ERASER'}"></i></span>
    </div>
</template>

<style scoped>
    span{
        display: block;
        width: 1em;
        text-align: center;
        cursor: pointer;
    }

    span:hover i{
        color: var(--color-text);
    }

    i{
        color: var(--background-lighter);
    }

    span:hover i{
        color: var(--color-text);
    }

    .gg-border-style-solid{
        transform: rotate(45deg);
    }
    i.active{
        color: var(--color-text);
    }
</style>

<script>
    import GUI_CONSTANTS from '../../values/gui-constants'

    export default{
        name: 'Tools',
        data(){
            return{
                status: GUI_CONSTANTS.PLAYER_STATUS,
            }
        },
        methods: {
            changeState: function(status){
                this.status = status;
            },
        }
    }

</script>