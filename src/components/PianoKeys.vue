<template>
<template v-for="octave in octaves"> 
            <template v-for="key in keys">
                <div class="key" :class="key.replace('#','-sharp')"><span>{{ key }}{{ octave }}</span></div>
            </template>
        </template> 
</template>

<style scoped>

.piano .key{
    width: 50px;
    height: var(--note-height);
    border-radius: 0 3px 3px 0;
    border: var(--border);
    border-bottom: 0;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    padding: 0 6px;
    font-size: .7em;
    cursor: pointer;
    color: #888;
}

.piano .key.C,
.piano .key.D,
.piano .key.E,
.piano .key.F,
.piano .key.G,
.piano .key.A,
.piano .key.B
{
    background-color: #eaeaea;
    height: calc(var(--note-height) * 1.5);
    margin: calc(var(--note-height) * -.5) 0;
}
.piano .key.C-sharp,
.piano .key.D-sharp,
.piano .key.F-sharp,
.piano .key.G-sharp,
.piano .key.A-sharp 
{
    /* color: #fff; */
    background-color: var(--background-dark);
    width: 70%;z-index: 2;
}
.piano .key.D,
.piano .key.G,
.piano .key.A{
    height: calc(var(--note-height) * 2);
}
.piano .key.C,
.piano .key.F{margin-bottom: 0; padding-top: calc(var(--note-height) * .5);}
.piano .key.E,
.piano .key.B{margin-top: 0; padding-bottom: calc(var(--note-height) * .5);}
.piano .key.C{color: var(--primary); font-weight: 500;}
.piano .key span{pointer-events: none;}
.piano .key:hover{ background-color: var(--primary) !important; color: #fff;}
</style>

<script>
    export default {
        name: 'PianoKeys',
        data() {
            return {
                octaves:  [0,1,2,3,4,5,6,7,8,9],
                keys: ["C", "C#","D", "D#","E","F", "F#","G", "G#", "A", "A#", "B"]
            }
        }
    }
</script>