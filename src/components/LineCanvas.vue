<template>
    <canvas ref=canvas></canvas>
</template>

<style scoped>
    canvas {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;left: 0;
    }
</style>

<script>
    import GUI_CONSTANTS from '../../values/gui-constants';

    export default{
        name: 'LineCanvas',
        props: ['baseNotes', 'color', 'strokeWidth', 'parameter'],
        data(){
            return{
                ctx: null,
                offset: (GUI_CONSTANTS.QUARTER_WIDTH / GUI_CONSTANTS.SCALE_FACTOR) * 2,
                pitchScale: 4,
                basePitch: {},
                mainColor: GUI_CONSTANTS.COLORS.PRIMARY,
            }
        },
        methods: {
                drawLine: async function (){
                    this.ctx.beginPath();

                    Object.keys(this.basePitch).forEach((tick, i)=>{
                        this.ctx.lineTo(this.offset + (tick / GUI_CONSTANTS.SCALE_FACTOR), (this.basePitch[tick] - (GUI_CONSTANTS.NOTE_HEIGHT / 2)) - ((this.parameter ? this.parameter?.values[this.parameter?.ticks.indexOf(Number.parseFloat(tick))] : 0) * GUI_CONSTANTS.PITCH_SCALE));
                        this.ctx.moveTo(this.offset + (tick / GUI_CONSTANTS.SCALE_FACTOR), (this.basePitch[tick] - (GUI_CONSTANTS.NOTE_HEIGHT / 2)) - ((this.parameter ? this.parameter?.values[this.parameter?.ticks.indexOf(Number.parseFloat(tick))] : 0) * GUI_CONSTANTS.PITCH_SCALE));
                    });

                    this.ctx.lineWidth = this.strokeWidth;
                    this.ctx.strokeStyle = this.color;
                    this.ctx.stroke();
                    
                    this.ctx.closePath();
                },
                parseBase: async function(){

                    const anchor = GUI_CONSTANTS.NOTE_HEIGHT * 120;
                    
                    for (let note = 0; note < this.baseNotes.length; note++) {
                        const CurrentNote = this.baseNotes[note];
                        const PreviousNote = this.baseNotes[note - 1] || CurrentNote;
                        const NextNote = this.baseNotes[note + 1] || CurrentNote;

                        let  smoothnessIn = 1  * GUI_CONSTANTS.SMOOTHNESS_IN;
                        let  smoothnessOut = 1  * GUI_CONSTANTS.SMOOTHNESS_OUT;

                        // smoothness threshold
                        if (((CurrentNote.tickOff - CurrentNote.tickOn) / 2) < smoothnessOut) smoothnessOut = ((CurrentNote.tickOff - CurrentNote.tickOn) / 2);

                        // TRANSITION IN
                        for (let i = 0; i < smoothnessIn; i++) {
                            let center = (((CurrentNote.key + PreviousNote.key) / 2) - 1);
                            let keyCenter = (CurrentNote.key - 1) - center
                            
                            this.basePitch[CurrentNote.tickOn + i] = anchor - ((center + (keyCenter * (i / smoothnessIn))) * GUI_CONSTANTS.NOTE_HEIGHT);
                        }

                        // SUSTAIN

                        for (let i = (CurrentNote.tickOn + smoothnessIn); i < (CurrentNote.tickOff - smoothnessOut); i++) {
                            this.basePitch[i] = anchor - ((CurrentNote.key - 1) * GUI_CONSTANTS.NOTE_HEIGHT);
                        }

                        // TRANSITION OUT
                        for (let i = 0; i < smoothnessOut; i++) {
                            let center = (((CurrentNote.key + NextNote.key) / 2) - 1);
                            let keyCenter = (CurrentNote.key - 1) - center
                            
                            this.basePitch[ (CurrentNote.tickOff - smoothnessOut) + i] = anchor - (((center + keyCenter) - (keyCenter * (i / smoothnessOut))) * GUI_CONSTANTS.NOTE_HEIGHT);
                        }
                    }
                },
        },
        mounted(){
            let canvas = this.$refs.canvas;
            canvas.width = GUI_CONSTANTS.TRACK_WIDTH + (this.offset * 2);
            canvas.height = GUI_CONSTANTS.NOTE_HEIGHT * 120;

            this.ctx = canvas.getContext("2d");

            this.parseBase().then(()=>{this.drawLine()});
            
        },
        
    }
</script>