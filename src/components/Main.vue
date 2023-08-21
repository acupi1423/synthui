<template>
    <div class="main-wrapper panel" :style="{'--primary': mainColor}">
        <div class="header">
            <div class="header-toolbar"></div>
        </div>
        <div class="track-manager-panel">
            <div class="scroll" ref="TrackManagerPanel">
                <TrackManager :tracks="tracks"/>
            </div>
        </div>
        <div class="piano-roll-panel panel" ref="pianoRollPanel" :style="{'min-height': pianoRollMinHeight + 'px', 'height': pianoRollHeight + 'px'}">
            <div class="toolbar">
                <div class="resize"></div>
                <div class="wrapper">
                    <div class="wrap">
                        <div class="panel">
                            <div class="text">Track</div>
                            <input type="text" class="form-control" :value="tracks[selectedTrack]?.name" placeholder="Track Name">
                        </div>
                        <div class="panel">
                            <div class="text">Singer</div>
                            <div class="form-control">{{tracks[selectedTrack]?.singer.name}}</div>
                        </div>
                    </div>
                    <div class="wrap">
                        <Player />
                    </div>
                    <div class="wrap">
                        <!-- <Tools /> -->
                        <div class="panel">
                            <button class="button" :class="{active: pianoMode == 'NOTES'}" v-on:click="pianoMode = 'NOTES'">NOTES</button>
                            <button class="button" :class="{active: pianoMode == 'PITCH'}" v-on:click="pianoMode = 'PITCH'">PITCH</button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="scroll" ref="pianoRollScroll">
                <PianoRoll :track="tracks[selectedTrack]" :mode="pianoMode"/>
                <div class="timing">
                    <TimingCanvas :baseNotes="tracks[selectedTrack].notes"/>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .main-wrapper{
        width: 100%;
        height: 100vh;
        display: flex;
        flex-direction: column;
        margin: auto;
        overflow: hidden;
    }

    .track-manager-panel{
        position: relative;
        top: 0;
        left: 0;
        width: 100%;
        height: auto;
        flex: 1;
        display: flex;
        overflow: hidden;

        background-color: var(--background-dark);
    }

    .piano-roll-panel{
        z-index: 10;
        width: 100%;
        height: auto;
        min-height: 100px;
        position: relative;
        display: flex;
        flex-direction: column;
    }

    .scroll{
        overflow: auto;
        position: relative;
        width: 100%;
        height: 100%;
        scroll-behavior: smooth;
    }

    .toolbar{
        width: 100%;
        display: flex;
        flex-direction: column;
        z-index: 100;
        background-color: var(--background-darker);
    }


    .toolbar .resize{
        width: 100%;
        height: 6px;
        margin-bottom: -5px;
        border-top: 1px solid var(--primary);
        cursor: n-resize;
        opacity: 0;
        position: relative;
    }
    .toolbar .resize:hover{opacity: 1;}

    .toolbar .wrapper{
        padding: 4px;
        width: 100%;
        display: flex;
        justify-content: space-between;
        background-color: var(--background-dark);
        border-bottom: 1px solid var(--background-darker);
    }

    .toolbar .wrap{
        /* width: 200px; */
        display: flex;
        justify-content: space-between;
        /* margin: auto; */
        gap: 10px;
    }

    .toolbar .panel{
        width: 200px;
        height: 24px;
        display: flex;
        align-items: center;
        justify-content: space-around;
        background-color: var(--background);
    }

    .toolbar .panel .text{
        padding: 0 10px;
        font-size: .85em;
        opacity: .7;
        height: 100%;
        line-height: 22px;
        border-right: 1px solid var(--background-darker);
    }
    .toolbar .panel .form-control{
        padding: 4px 8px;
        background-color: var(--background);
        height: 100%;
        font-weight: 500;
        font-size: 1em;
        line-height: 14px;
    }
    .toolbar .panel .form-control:not(input){cursor: pointer;}

    .toolbar .panel:only-child{margin: auto;}

    .timing{
        flex: 1;
        height: fit-content;
        width: fit-content;
        position: sticky;
        bottom: 0;
        border-top: 1px solid var(--background-darker);
        z-index: 100;
    }

</style>

<script>
    import TrackManager from './TrackManager.vue'
    import TrackControl from './TrackControl.vue'
    import PianoRoll from './PianoRoll.vue'
    import Player from './Player.vue'
    import Tools from './Tools.vue'
    import TimingCanvas from './TimingCanvas.vue'
    
    import GUI_CONSTANTS from '../../values/gui-constants'

    import projectData from '../data/mounai.json'

    export default {
        name: "Main",
        components:{
            TrackManager,
            PianoRoll,
            TrackControl,
            Player,
            Tools,
            TimingCanvas,
        },
        data(){
            return{
                mainColor: GUI_CONSTANTS.COLORS.PRIMARY,
                pianoRollMinHeight: 0,
                pianoRollHeight: window.innerHeight - this.trackManagerHeight,
                selectedTrack: 0,
                tracks: projectData.project.tracks,
                pianoMode: 'NOTES',
            }
        },
        methods: {
            resize: function(evt){
                // console.log(projectData);
                // this.pianoRollHeight = evt?.position?.top;
            }
        },
        beforeMount(){
            GUI_CONSTANTS.TRACK_WIDTH = ((this.tracks[this.selectedTrack].notes[this.tracks[this.selectedTrack].notes.length -1].tickOff) / GUI_CONSTANTS.SCALE_FACTOR);
        },
        mounted(){
            this.pianoRollHeight = window.innerHeight - this.$refs.TrackManagerPanel.scrollHeight - 10;
            this.pianoRollMinHeight = this.pianoRollHeight.valueOf();

            // scroll to notes 
            this.$refs.pianoRollScroll.scrollTo(0, ((120 * GUI_CONSTANTS.NOTE_HEIGHT) - (this.tracks[this.selectedTrack].notes[1].key * GUI_CONSTANTS.NOTE_HEIGHT)) - (this.$refs.pianoRollPanel.offsetHeight  / 2));
        }
    }
</script>