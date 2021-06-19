<template>
  <div class="darkPadding">
    <img class="cursor-pointer" :src="(mode === 'dark') ?  Moon  : Sun " @click="$emit('nightMode')"/>
  </div>
</template>

<script>
import Sun from '../assets/images/icon-sun.svg'
import Moon from '../assets/images/icon-moon.svg'
export default {
    props: ['mode'],
    name: 'NightModeButton',
    data() {
        return{
            Sun,
            Moon,
        }
    }

}
</script>