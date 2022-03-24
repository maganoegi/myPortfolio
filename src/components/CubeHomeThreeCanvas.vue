
<template>
    <div id="threeCubeContainer"></div>
</template>

<script>
    /* eslint-disable */
    import * as THREE from 'three'

    import {EVENT_EMITTER} from '../libs/constants' 
    // import {THREEx} from '../libs/dynamictexture' 

    
    export default {
        name: "CubeHomeThreeCanvas",

        data() {
            // SOURCE: https://stackoverflow.com/questions/65693108/threejs-component-working-in-vuejs-2-but-not-3
            return {}
        },

        methods: {
            init() {
                // SOURCE: https://www.youtube.com/watch?v=IA3HjAV2nzU


                let container = document.getElementById('threeCubeContainer');

                this.camera = new THREE.PerspectiveCamera(70, container.clientWidth/container.clientHeight, 0.01, 10);
                this.camera.position.z = 1;

                this.renderer = new THREE.WebGLRenderer({antialias: true});
                this.renderer.setSize(container.clientWidth, container.clientHeight);
                container.appendChild(this.renderer.domElement);

                this.scene = new THREE.Scene();

                // var dynamictexture = new THREEx.DynamicTexture(512, 512);
                // dynamictexture.context.font = "bolder 90px verdana";
                // dynamictexture.texture.needsUpdate = true;
                // dynamictexture.clear('#d35400').drawText('Text', undefined, 256, 'green');
                const ambient = new THREE.AmbientLight(0x222222);
                this.scene.add(ambient);

                const light = new THREE.DirectionalLight(0xffffff);
                light.position.set(10, 0, 6);
                this.scene.add(light)

                const loader = new THREE.TextureLoader();
                const texture = loader.load(
                    require('../assets/marble.jpg')
                )
                const material = new THREE.MeshLambertMaterial({
                    map: texture,
                });

                let geometry = new THREE.BoxGeometry(0.5, 0.5, 0.5);
                
                this.mesh = new THREE.Mesh(geometry, material);
                this.scene.add(this.mesh);
            },

            rotateFromCoords(relX, relY) {
                // requestAnimationFrame(this.animate);
                this.mesh.rotation.y = (Math.PI * 2) * -relX * 1;
                this.mesh.rotation.x = (Math.PI) * relY + Math.PI/2;
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
            this.renderer.render(this.scene, this.camera);  

            EVENT_EMITTER.on('mouseMovedTo', (coords) => {
                this.rotateFromCoords(coords.relX, coords.relY)
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