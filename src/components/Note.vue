<template>
    <div class="note overflow" :style="style">
        <span class="lyric">{{note.lyric}}</span>
        <span class="phoneme overflow" v-if="note.phoneme">[{{note.phoneme}}]</span>
    </div>
</template>

<style scoped>
    .note{
        position: absolute;
        box-sizing: border-box;
        border-radius: 4px;
        background-color: var(--primary);
        border: 1px solid var(--background);
        display: flex;
        padding: 0 6px;
        
        cursor: var(--cursor);
    }

    .note span{
        margin: auto 0;
        font-size: .9em;
        z-index: var(--lyrics-index);
    }
    .note .lyric{color: var(--color-text); }
    .note .phoneme{
        display: var(--display-phonemes);
        color: var(--primary);
        filter: brightness(.6);
        margin-left: 6px;
    }

    .overflow{
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
</style>

<script>
    import GUI_CONSTANTS from '../../values/gui-constants';

    export default{
        name: 'Note',
        props: [
            'note',
        ],
        computed:{
            style(){
                let offset = (GUI_CONSTANTS.QUARTER_WIDTH / GUI_CONSTANTS.SCALE_FACTOR) * 2;
                return {'height': GUI_CONSTANTS.NOTE_HEIGHT + 'px', 'left': (((this.note.tickOn / GUI_CONSTANTS.SCALE_FACTOR) + offset) + 'px'), 'bottom': (GUI_CONSTANTS.NOTE_HEIGHT * (this.note.key - 1)) + 'px', 'width': ((this.note.tickOff / GUI_CONSTANTS.SCALE_FACTOR) - (this.note.tickOn / GUI_CONSTANTS.SCALE_FACTOR))+'px', "font-size": (GUI_CONSTANTS.NOTE_HEIGHT * .6) + 'px'}
            }
        }
    }
</script>