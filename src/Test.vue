<template>
    <div>
        <img id="image-to-replace" src="./assets/cake1.jpg" />
        <ImageColorDropper
            imageId="image-to-replace"
            @hex="showHex"
        />
    </div>
</template>

<script>
import ImageColorDropper from './ImageColorDropper.vue'

export default {
    components: {
        ImageColorDropper
    },
    methods: {
        showHex (hex) {
            console.log(hex)
        }
    }
}
</script>