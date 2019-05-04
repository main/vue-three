<template>
    <div class="three_container">
        <canvas class="three_canvas" ref="threejs"></canvas>
    </div>
</template>

<script>
import { setTimeout, clearTimeout } from 'timers';
    const THREE = require('three')

    export default {
        name: "Three.vue",
        mounted() {
            // Create an empty scene
            const scene = new THREE.Scene()
            // Create a basic perspective camera
            const camera = new THREE.PerspectiveCamera( 75, window.innerWidth/window.innerHeight, 0.1, 1000 )
            camera.position.z = 4
            // Create a renderer with Antialiasing
            const renderer = new THREE.WebGLRenderer({
                canvas: this.$refs.threejs,
                antialias: true
            })
            // Configure renderer clear color
            renderer.setClearColor("#000000")
            // Configure renderer size
            renderer.setSize( window.innerWidth, window.innerHeight )

            const render = this.init(renderer, scene, camera)

            const rfn = () => {
                render()
                requestAnimationFrame(rfn)
            }

            requestAnimationFrame(rfn)

            const resizeObserverCallback = (() => {
                let w, h
                return () => {
                    if (
                        w !== this.$el.clientWidth ||
                        h !== this.$el.clientHeight
                    ) {
                        h = this.$el.clientHeight
                        w = this.$el.clientWidth
                        this.$refs.threejs.style.height = h + 'px'
                        this.$refs.threejs.style.width = w + 'px'

                        renderer.setSize( w, h )

                        camera.aspect = w / h;
                        camera.updateProjectionMatrix();
                    }

                    this._resizeObserverTimeout = setTimeout(resizeObserverCallback, 200)
                }
            })()

            resizeObserverCallback()
        },
        beforeDestroy() {
            clearTimeout(this._resizeObserverTimeout)
        },
        methods: {
            // Should return render function
            init(renderer, scene, camera) {
                const geometry = new THREE.BoxGeometry( 1, 1, 1 )
                const material = new THREE.MeshBasicMaterial( { color: "#433F81" } )
                const cube = new THREE.Mesh( geometry, material )
                
                scene.add( cube )

                return () => {
                    cube.rotation.x += 0.01
                    cube.rotation.y += 0.01
                    
                    renderer.render(scene, camera)
                }
            }
        }
    }
</script>

<style scoped>
    .three_container {
        /* display: block; */
        margin: 0;
        padding: 0;
    }
    .three_canvas {
        display: block;
        margin: 0;
        padding: 0;
    }
</style>