<template>
    <div class="panel transparent-panel">
        
        <template v-if="track.type != 'audio'">
            <div class="singer-picture fill pointer">
                <template v-if="track?.singer?.img">
                    <img v-bind:src="track?.singer?.img" class="img">
                </template>
                <i class="icon gg-user" v-else></i>
            </div>
        </template>
        <template v-else>
            <div class="singer-picture">
                <i class="icon gg-loadbar-sound"></i>
            </div>
        </template>
        <div class="data">
            <div class="title"><input type="text" class="form-control" :value="track?.name"></div>
            <div class="flex">

                <template v-if="track?.type != 'audio'">
                    <div class="singer-name">{{track?.singer?.name}} <span>({{track?.singer?.version}})</span></div>
                </template>
                <template v-else>
                    <div class="singer-name"><span>Audio Track</span></div>
                </template>
                
                <div class="track-modes">
                    <button class="button">M</button>
                    <button class="button">S</button>
                </div>
            </div>
        </div>
        <div class="controls">

        </div>
    </div>
</template>

<style scoped>
    .panel{
        background-color: var(--background-transparent);
        display: flex;
        overflow: hidden;

        --singer-size: 60px;
    }

    .singer-picture{
        width: var(--singer-size);height: var(--singer-size);
        background-color: var(--background-light);
        display: flex;
        overflow: hidden;
    }
    .singer-picture.fill{background-color: var(--primary);}
    .singer-picture img{
        width: 100%;
        height: 100%;
        object-fit: cover;
        transform: scale(1.5);
        filter: saturate(1.5);
    }
    .singer-picture .icon{
        color: var(--color-text);
        margin: auto;
        zoom: 1.2;
    }
    .gg-loadbar-sound{
        zoom: 1.5;
    }

    
    .singer-name{
        font-size: .85em;
        opacity: .7;
        margin: auto 0;
    }
    .singer-name span{opacity: .5;}
    .data{
        display: flex;
        width: calc(100% - var(--singer-size));
        flex-direction: column;
        justify-items: center;
        margin: auto 0;
        padding: 0 10px;
        line-height: 1.2;
    }
    .title{
        display: flex;
    }
    .title input{
        border: 0;
        font-weight: 500;
        line-height: 1;
    }
    .title input:not(:focus){
        padding-left: 0;padding-right: 0;
        background-color: transparent;
    }

    .flex{display: flex; width: 100%; justify-content: space-between;}
    .track-modes{display: flex; justify-content: flex-end;}
    .track-modes button{
        width: 20px;height: 20px;
        margin-top: auto;
        margin-left: 4px;
        cursor: pointer;
        padding: 0;
        font-size: 10px;
    }
</style>

<script>
    export default {
        props: ['track']
    }
</script>