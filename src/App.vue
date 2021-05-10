<template>
    <div>
      <Tracker />
    </div>
</template>

<script>
import Tracker from './components/Tracker'
import Result from './components/Result'

export default {
    name: 'App',
    components: {
        Tracker,
        Result
    }
}
</script>

<style lang="scss">
    @import "./main.scss";
    #app {
        height: 100%;
        width: 100%;
        margin: 0 auto;
        position: relative;
    }
</style>
