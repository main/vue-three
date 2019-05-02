<template>
    <canvas class="three_canvas" ref="threejs"></canvas>
</template>

<script>
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
                antialias:true
            })
            // Configure renderer clear color
            renderer.setClearColor("#000000")
            // Configure renderer size
            renderer.setSize( window.innerWidth, window.innerHeight )
            // ------------------------------------------------
            // FUN STARTS HERE
            // ------------------------------------------------
            // Create a Cube Mesh with basic material
            var geometry = new THREE.BoxGeometry( 1, 1, 1 )
            var material = new THREE.MeshBasicMaterial( { color: "#433F81" } )
            var cube = new THREE.Mesh( geometry, material )
            // Add cube to Scene
            scene.add( cube )
            // Render Loop
            const render = function () {
                requestAnimationFrame( render )
                cube.rotation.x += 0.01
                cube.rotation.y += 0.01
                // Render the scene
                renderer.render(scene, camera)
            }
            render()
        }
    }
</script>

<style scoped>
    .three_canvas {
        width: 100vw;
        height: 100vh;
    }
</style>