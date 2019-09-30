<template>
    <canvas ref="canvas" />
</template>

<script>
export default {
    props: {
        imageId: {
            type: [ String ],
            required: true
        }
    },
    data () {
        return {
            image: '',
            body: document.body
        }
    },
    methods: {
        draw () {
            this.image = document.getElementById(this.imageId)

            this.image.addEventListener('load', () => {
                let canvas = this.$refs.canvas

                canvas.width = this.image.width
                canvas.height = this.image.height

                canvas
                    .getContext('2d')
                    .drawImage(
                        this.image,
                        0,
                        0,
                        this.image.width,
                        this.image.height,
                        0,
                        0,
                        canvas.width,
                        canvas.height
                    )
            })
        },
        handleMouseOnCanvas (event) {
            event.preventDefault()
            event.stopPropagation()

            if (this.body.style.cursor) {
                return
            }

            this.body.style.cursor = 'url("data:image/x-icon;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABmJLR0QA/wD/AP+gvaeTAAABZklEQVRIibXVu0odURjF8Z+XxhSiYJVAciovUSHBkDSaoE/gC9gGfIBgEbSyFsR3SIoUFukjpBOsNFYiCiFeQY0aUFGPxd5DDoN6ZsZxwWaYb5j/WvtjXyhHffiOUxzhKyolsb3HCaqpsYsXD4V/EFKn4cn4UgTaFJ9DdeBVHDTmhA/gF17iN/YzhskMP4jJdqJJBRvunsF8HoOfqZ+30HWPyTF68hi8xUUKkszkOdZr6v8wnAfehT/Yw2odk8LwHfSiHUt3mFQwmAfeje04umvq95k8GJ6oA2spkxVkWvrpttymARziLMIP8SYLvLNOcngnHGpr6McPvMoCf4rNmDwL/FkWaKKGmOQvXpcNJ6zdKj4+BhymhZ3a8hjwJ8IuXLjl24jQtsLwRuFiuBIukEStmMA5loUFUEhzQu8/x/dRLOIy1r+hrSgcxoT085j1f0dOCSdoKRrHdYTPoLkscK0+YbJs6A0RmY1aTL+JrQAAAABJRU5ErkJggg=="), auto'

        },
        handleMouseLeaveCanvas (event) {
            event.preventDefault()
            event.stopPropagation()

            this.body.style.cursor = ''
        },
        getEventLocation(element, event) {
            var rect = element.getBoundingClientRect()

            return {
              x: event.clientX - rect.left,
              y: event.clientY - rect.top
            }
        },
        rgbToHex(r, g, b) {
            if (r > 255 || g > 255 || b > 255)
                throw "Invalid color component";
            return ((r << 16) | (g << 8) | b).toString(16);
        },
    },
    mounted () {
        this.draw()

        const canvas = this.$refs.canvas

        canvas.addEventListener('mousemove', event => {
            this.handleMouseOnCanvas(event)
        })

        canvas.addEventListener('mouseleave', event => {
            this.handleMouseLeaveCanvas(event)
        })

        canvas.addEventListener('click', event => {
                let eventLocation = this.getEventLocation(canvas, event)
                let context       = canvas.getContext('2d')
                let pixelData     = context.getImageData(eventLocation.x, eventLocation.y, 1, 1).data

                if ((pixelData[0] == 0) && (pixelData[1] == 0) && (pixelData[2] == 0) && (pixelData[3] == 0)){
                    // Do something if the pixel is transparent
                }

                let hex = "#" + ("000000" + this.rgbToHex(pixelData[0], pixelData[1], pixelData[2])).slice(-6)

                this.$emit('hex', hex)
            }, false)
    }
}
</script>