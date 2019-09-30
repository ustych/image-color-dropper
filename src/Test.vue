<template>
    <div>
        <img id="image-to-replace" src="https://picsum.photos/200/300" crossOrigin="" />
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