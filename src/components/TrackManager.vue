<template>
    <div class="tracks-wrapper"  :style="{'--tick-width': noteWidth + 'px' }">
        <div class="controls">
            <div class="head">
                <b>Tracks</b>
                <button class="button">+ &nbsp;Add Track</button>
            </div>
            <div class="tracks-container">
            <template v-for="track in tracks">
                <div class="track">
                    <TrackControl :track="track" />
                </div>
            </template>

            </div>
        </div>
        <div class="clips">
                <div class="signature head">
                    <template v-for="tick in ticks">
                        <div class="tick">
                            {{tick}}
                        </div>
                    </template>
                </div>
                <div class="tracks-container">
                    
                    <svg v-bind:width="pianoRollWidth" v-bind:height="(tracks.length * 70) + 'px'">
                        <defs>
                            <pattern id="tracks-pattern" x="0" v-bind:width="4 * noteWidth" v-bind:height="70" patternUnits="userSpaceOnUse">
                                <line class="tick" stroke-width="2" v-bind:x1="0 * noteWidth" v-bind:x2="0 * noteWidth" y1="0" v-bind:y2="70"></line>
                                <line class="tick" stroke-width=".5" v-bind:x1="1 * noteWidth" v-bind:x2="1 * noteWidth" y1="0" v-bind:y2="70"></line>
                                <line class="tick" stroke-width=".5" v-bind:x1="2 * noteWidth" v-bind:x2="2 * noteWidth" y1="0" v-bind:y2="70"></line>
                                <line class="tick" stroke-width=".5" v-bind:x1="3 * noteWidth" v-bind:x2="3 * noteWidth" y1="0" v-bind:y2="70"></line>
                            </pattern>
                        </defs>
                        <rect fill="url(#tracks-pattern)" x="0" v-bind:y="0" width="100%" v-bind:height="(tracks.length * 70) + 'px'"></rect>
                    </svg>

                    <template v-for="track in tracks">
                        <div class="track" :style="{'width': pianoRollWidth + 'px'}">
                        </div>
                    </template>
                </div>
        </div>
    </div>
</template>

<style scoped>
    .tracks-wrapper{
        width: fit-content;
        display: flex;
        background-image: url(track_background);
    }
    .track{
        width: auto;
        min-width: 100%;
        height: 70px;
        display: flex;
        padding: 4px;
    }
    .controls .track:nth-child(2n){background-color: var(--background);}
    .clips .track:nth-child(2n + 1){background-color: var(--background);}
    
    .tracks-container{
        display: flex;
        width: fit-content;
        flex-direction: column;
        position: relative;
    }

    .head{
        height: 36px;
        display: flex;
        position: sticky;
        top: 0;
        background-color: var(--background);
        z-index: 2;
    }

    .controls{
        width: 240px;
        z-index: 110;
        position: sticky;
        left: 0;
        background-color: var(--background-dark);
    }

    .controls .head{
        display: flex;
        justify-content: space-between;
        padding: 0 4px;
    }

    .controls .track{
    }
    .controls .head b{
        margin: auto 0;
        font-weight: 500;
    }
    .controls .head button{margin: auto 0;}

    .clips{
        width: fit-content;
    }

    .clips svg{
        position: absolute;
        top: 0;left: 0;
        pointer-events: none;
    }

    .clips .tick {
        stroke: var(--background-lighter);
        stroke-opacity: 1;
    }

    .clips .signature {
        width: fit-content;
        color: #fff;
        z-index: 10;
    }
    .clips .signature .tick{
        width: var(--tick-width);
        padding: 0 8px;
        height: 100%;
        display: flex;
        align-items: center;
        font-size: .7em;
        border-left: 1px solid var(--background-lighter);
    }
</style>

<script>
    import TrackControl from './TrackControl.vue';
    import GUI_CONSTANTS from '../../values/gui-constants'

    export default {
        components: {
            TrackControl
        },
        props: ['tracks'],
        data(){
            return{
            noteWidth: (GUI_CONSTANTS.QUARTER_WIDTH / GUI_CONSTANTS.TRACKS_SCALE_FACTOR),
            pianoRollWidth: (GUI_CONSTANTS.TRACK_WIDTH / GUI_CONSTANTS.TRACKS_SCALE_FACTOR),
            mainColor: GUI_CONSTANTS.COLORS.PRIMARY,
            ticks:  Array.from({ length: ((GUI_CONSTANTS.TRACK_WIDTH / GUI_CONSTANTS.TRACKS_SCALE_FACTOR) / (GUI_CONSTANTS.QUARTER_WIDTH / GUI_CONSTANTS.TRACKS_SCALE_FACTOR)) }, (value, index) => index),
            }
        }
    }

</script>