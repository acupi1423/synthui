<template>
    <div class="piano-wrapper" :style="{ '--note-height': noteHeight + 'px','--note-width': noteWidth + 'px' }">
        <div class="piano" :style="{ 'height': (120 * noteHeight) + 'px' }">
            <PianoKeys />
        </div>
        <div class="grid" v-bind:class="mode.toLowerCase() + '-mode'">
            <div class="signature" v-on:click="changeMarkerPos">
                <div class="tick"></div>
                <div class="tick"></div>
                <template v-for="tick in ticks">
                    <div class="tick">
                        {{tick}}
                    </div>
                </template>
            </div>
                <svg v-bind:width="(pianoRollWidth + (noteWidth * 4))" v-bind:height="120 * noteHeight">
                <defs>
                    <pattern id="pattern" x="0" width="1" v-bind:height="12 * noteHeight" patternUnits="userSpaceOnUse">
                        <rect v-bind:width="pianoRollWidth" class="white" x="0" v-bind:y="0 * noteHeight"></rect>
                        <rect v-bind:width="pianoRollWidth" class="black" x="0" v-bind:y="1 * noteHeight"></rect>
                        <rect v-bind:width="pianoRollWidth" class="white" x="0" v-bind:y="2 * noteHeight"></rect>
                        <rect v-bind:width="pianoRollWidth" class="black" x="0" v-bind:y="3 * noteHeight"></rect>
                        <rect v-bind:width="pianoRollWidth" class="white" x="0" v-bind:y="4 * noteHeight"></rect>
                        <rect v-bind:width="pianoRollWidth" class="black" x="0" v-bind:y="5 * noteHeight"></rect>
                        <rect v-bind:width="pianoRollWidth" class="white" x="0" v-bind:y="6 * noteHeight"></rect>
                        <!-- ----- -->
                        <rect v-bind:width="pianoRollWidth" height="0.5" class="row-border" x="0" v-bind:y="7 * noteHeight - 1"></rect>
                        <rect v-bind:width="pianoRollWidth" class="white" x="0" v-bind:y="7 * noteHeight"> </rect>
                        <rect v-bind:width="pianoRollWidth" class="black" x="0" v-bind:y="8 * noteHeight"> </rect>
                        <rect v-bind:width="pianoRollWidth" class="white" x="0" v-bind:y="9 * noteHeight"> </rect>
                        <rect v-bind:width="pianoRollWidth" class="black" x="0" v-bind:y="10 * noteHeight"></rect>
                        <rect v-bind:width="pianoRollWidth" class="white" x="0" v-bind:y="11 * noteHeight"></rect>
                        <!-- ----- -->
                        <rect v-bind:width="pianoRollWidth" height="0.5" class="row-border" x="0" v-bind:y="12 * noteHeight - 1"></rect>
                    </pattern>
                    <pattern id="grid-pattern" x="0" v-bind:width="4 * noteWidth" v-bind:height="12 * noteHeight" patternUnits="userSpaceOnUse">
                        <line class="tick" stroke-width="2" v-bind:x1="0 * noteWidth" v-bind:x2="0 * noteWidth" y1="0" v-bind:y2="12 * noteHeight"></line>
                        <line class="tick" stroke-width=".5" v-bind:x1="1 * noteWidth" v-bind:x2="1 * noteWidth" y1="0" v-bind:y2="12 * noteHeight"></line>
                        <line class="tick" stroke-width=".5" v-bind:x1="2 * noteWidth" v-bind:x2="2 * noteWidth" y1="0" v-bind:y2="12 * noteHeight"></line>
                        <line class="tick" stroke-width=".5" v-bind:x1="3 * noteWidth" v-bind:x2="3 * noteWidth" y1="0" v-bind:y2="12 * noteHeight"></line>
                    </pattern>
                </defs>
                
                <rect fill="url(#pattern)" class="darken" x="0" v-bind:y="0" v-bind:width="2 * noteWidth" v-bind:height="120 * noteHeight"></rect>
                <rect fill="url(#pattern)" v-bind:x="2 * noteWidth" v-bind:y="0" v-bind:width="pianoRollWidth" v-bind:height="120 * noteHeight"></rect>
                <rect fill="url(#grid-pattern)" x="0" v-bind:y="0" width="100%" v-bind:height="120 * noteHeight"></rect>
                <rect fill="url(#pattern)" class="darken" v-bind:x="(2 * noteWidth) + pianoRollWidth" v-bind:y="0" v-bind:width="4 * noteWidth" v-bind:height="120 * noteHeight"></rect>
                </svg>

            <div class="notes absolute">
                <template v-if="true">
                    <template v-for="note in track.notes">
                        <Note :note="note"/>
                    </template>

                </template>
            </div>
            <div class="pitch absolute" v-show="mode == 'NOTES'">
                <LineCanvas :baseNotes="track.notes" :strokeWidth="1" :color="'#cacaca'" :parameter="track.pitch"/>
            </div>
            <div class="pitch absolute" v-show="mode == 'PITCH'">
                <LineCanvas :baseNotes="track.notes" :strokeWidth="1" :color="'#aaaaaa'" :parameter="null"/>
                <LineCanvas :baseNotes="track.notes" :strokeWidth="2" :color="mainColor" :parameter="track.pitch"/>
            </div>
        </div>

        <div class="playback-marker" :style="{'height': (100 + (120 * noteHeight)) + 'px', 'left': markerPos + 'px'}"></div>
    </div>
</template>

<style scoped>
.piano-wrapper {
    display: flex;
    width: fit-content;
}

.grid {
    width: auto;
    flex: 1;
    position: relative;
    display: flex;
    flex-direction: column;
}

.grid .white {
    fill: var(--background);
    height: var(--note-height);
}

.grid .black {
    fill: var(--background-dark);
    height: var(--note-height);
}

.grid .darken{filter: brightness(.7);}

.grid .tick {
    stroke: var(--background-lighter);
    stroke-opacity: 1;
}

.grid .signature {
    position: sticky;
    top: 0;
    width: 100%;
    height: 26px;
    margin-bottom: -26px;
    background-color: var(--background-transparent);
    border-bottom: 1px solid var(--background-dark);
    color: #fff;
    display: flex;
    z-index: 101;
    /* backdrop-filter: blur(4px); */

    cursor: pointer;
}
.grid .signature .tick{
    width: var(--note-width);
    padding: 0 8px;
    height: 100%;
    display: flex;
    align-items: center;
    font-size: .85em;
    border-left: 1px solid var(--background-lighter);
    pointer-events: none;
}

.absolute{
    position: absolute;
    bottom: 0;left: 0;
    width: 100%;
    height: 100%;
}

.piano {
    display: flex;
    flex-direction: column-reverse;
    position: relative;
    position: sticky;
    left: 0;
    z-index: 200;
}

.notes{
    width: 100%;
    height: 100%;

    --color-text: #000;
    --display-phonemes: block;
    --lyrics-index: 100;
    --cursor: pointer;
}


.pitch{
    width: 100%;
    height: 100%;
    z-index: 10;
    pointer-events: none;
}

.scroll{
    position: relative;
}


/* MODES */

.pitch-mode .notes{
    --primary: var(--background-light);
    --background: var(--background-lighter);
    --color-text: #fffa;
    --display-phonemes: none;
    --cursor: unset;
    --lyrics-index: 1;

    cursor:crosshair;
}
</style>
<script>
import PianoKeys from './PianoKeys.vue';
import Note from './Note.vue';
import LineCanvas from './LineCanvas.vue';
import GUI_CONSTANTS from '../../values/gui-constants';

export default {
    name: 'PianoRoll',
    props: ['track', 'mode'],
    components: { PianoKeys, Note, LineCanvas},
    data() {
        return {
            noteHeight: GUI_CONSTANTS.NOTE_HEIGHT,
            noteWidth: (GUI_CONSTANTS.QUARTER_WIDTH / GUI_CONSTANTS.SCALE_FACTOR),
            pianoRollWidth: GUI_CONSTANTS.TRACK_WIDTH,
            mainColor: GUI_CONSTANTS.COLORS.PRIMARY,
            ticks:  Array.from({ length: (GUI_CONSTANTS.TRACK_WIDTH / (GUI_CONSTANTS.QUARTER_WIDTH / GUI_CONSTANTS.SCALE_FACTOR)) }, (value, index) => index),
            markerPos: 530,
        }
    },
    methods: {
        changeMarkerPos: function(ev){
            this.markerPos = document.querySelector(".piano-roll-panel .scroll").scrollLeft + ev.x;
        }
    },
    onBeforeMount() {
    },
    mounted(){
    }
}

</script>