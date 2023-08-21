<template>
    <div class="panel">
            <span v-on:click="changeState('PLAY')"><i class="gg-play-button" :class="{'active': status == 'PLAY'}"></i></span>
            <span v-on:click="changeState('PAUSE')"><i class="gg-play-pause" :class="{'active': status == 'PAUSE'}"></i></span>
            <span v-on:click="changeState('STOP')"><i class="gg-play-stop"></i></span>
            <span><i class="gg-play-track-prev"></i></span>
            <span><i class="gg-play-track-next"></i></span>
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

    .gg-play-button.active{
        color: var(--live);
    }
    .gg-play-pause.active{
        color: var(--color-text);
    }
</style>

<script>
    import GUI_CONSTANTS from '../../values/gui-constants'

    export default{
        name: 'Player',
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