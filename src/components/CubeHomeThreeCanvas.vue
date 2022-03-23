

<template>
    <div id="threeCubeContainer"></div>
</template>

<script>
    import * as Three from 'three'

    import {EVENT_EMITTER} from '../libs/constants' 

    
    export default {
        name: "CubeHomeThreeCanvas",

        data() {
            // SOURCE: https://stackoverflow.com/questions/65693108/threejs-component-working-in-vuejs-2-but-not-3
            return {}
        },

        methods: {
            init() {
                let container = document.getElementById('threeCubeContainer');

                this.camera = new Three.PerspectiveCamera(70, container.clientWidth/container.clientHeight, 0.01, 10);
                this.camera.position.z = 1;

                this.scene = new Three.Scene();

                let geometry = new Three.BoxGeometry(0.2, 0.2, 0.2);
                let material = new Three.MeshNormalMaterial();

                this.mesh = new Three.Mesh(geometry, material);
                this.scene.add(this.mesh);

                this.renderer = new Three.WebGLRenderer({antialias: true});
                this.renderer.setSize(container.clientWidth, container.clientHeight);
                container.appendChild(this.renderer.domElement);
            },

            rotateFromCoords(x, y) {
                requestAnimationFrame(this.animate);
                this.mesh.rotation.x = x;
                this.mesh.rotation.y = y;
                this.renderer.render(this.scene, this.camera);  
            },

            animate() {
                requestAnimationFrame(this.animate);
                this.mesh.rotation.x += 0.01;
                this.mesh.rotation.y += 0.02;
                this.renderer.render(this.scene, this.camera);  
            }
        },

        mounted() {
            this.init();
            // this.animate();

            EVENT_EMITTER.on('mouseMovedTo', (event) => {
                this.rotateFromCoords(event.clientX, event.clientY)
            });
        }
    }
</script>

<style scoped lang="scss">
    #threeCubeContainer {
        width: 100%;
        height: 100%;
    }
</style>