<template>
    <div class="panel-wrapper" :style="{ 'width': pianoRollWidth + 'px' }">
        <div class="wave absolute" :style="{ 'padding-left': (50 + offset) + 'px' }">
                <svg  id="waveform" height="100" :width="pianoRollWidth + 'px'">
            <path stroke="currentColor"></path>
        </svg>
        </div>
        <div class="labels absolute">
            <template v-for="note of baseNotes">
                <div class="label" :style="{ 'left': 50 + (offset + (note.tickOn / scale)) + 'px', 'width': ((note.tickOff - note.tickOn) / scale) + 'px' }">
                    <span class="text-overflow">{{ note.phoneme }}</span></div>
            </template>
        </div>
    </div>
</template>

<style scoped>
.panel-wrapper {
    height: 100px;
    background-color: var(--background-transparent);
}
.absolute{
    position: absolute;
    bottom: 0;left: 0;
    width: 100%;
    height: 100%;
}

.labels {
    height: 100%;
}

.wave {
    height: 100%;
}

.label {
    position: absolute;
    top: 0;
    height: 100px;
    border-left: 1px solid var(--background-lighter);
}

.label span {
    display: block;
    width: 100%;
    font-size: .85em;
    padding: 4px;
}

.label:hover {
    background-color: var(--background);
}

.wave{
    pointer-events: none;
    z-index: 10;
}


#waveform{
    height: 100%;
    position: relative;
}

#waveform path{color: var(--background-lighter);}

</style>

<script>
import GUI_CONSTANTS from '../../values/gui-constants';
import { getAudioData, linearPath } from "waveform-path";

export default {
    name: 'LineCanvas',
    props: ['baseNotes'],
    data() {
        return {
            ctx: null,
            offset: (GUI_CONSTANTS.QUARTER_WIDTH / GUI_CONSTANTS.SCALE_FACTOR) * 2,
            scale: GUI_CONSTANTS.SCALE_FACTOR,
            pianoRollWidth: GUI_CONSTANTS.TRACK_WIDTH,
            file: "/mounai_melody.wav",
        }
    },
    components: {
    },
    methods: {
        AudioPath: async function(file) {
        const audioData = await getAudioData(file);

        const wavePath = linearPath(audioData,{ samples: this.pianoRollWidth,  type: 'steps', width: this.pianoRollWidth, normalize: true });
        document.querySelector("#waveform path").setAttribute('d', wavePath);

    }
    },
    mounted() {
        this.AudioPath("/mounai_melody.wav");
    },

}
</script>