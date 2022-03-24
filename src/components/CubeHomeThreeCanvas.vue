
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
            return {}
        },

        methods: {
            init() {

                let container = document.getElementById('threeCubeContainer');

                this.camera = new THREE.PerspectiveCamera(70, container.clientWidth/container.clientHeight, 0.01, 10);
                this.camera.position.z = 1;

                this.renderer = new THREE.WebGLRenderer({antialias: true});
                this.renderer.setSize(container.clientWidth, container.clientHeight);
                container.appendChild(this.renderer.domElement);

                this.scene = new THREE.Scene();

                const ambient = new THREE.AmbientLight(0x222222);
                this.scene.add(ambient);

                const light = new THREE.DirectionalLight(0xffffff);
                light.position.set(10, 0, 6);
                this.scene.add(light)

                const loader = new THREE.TextureLoader();
                const texture = loader.load(
                    require('../assets/marble.jpg')
                )
                // const texture = this.SpriteSheetTexture('../assets/grid-sprite.jpg', 4, 4, 100, 16);
                const material = new THREE.MeshLambertMaterial({
                    map: texture,
                });

                let geometry = new THREE.BoxGeometry(0.5, 0.5, 0.5);
                
                this.mesh = new THREE.Mesh(geometry, material);
                this.scene.add(this.mesh);
            },

            SpriteSheetTexture(imageURL, framesX, framesY, frameDelay, _endFrame) {
                // https://stackoverflow.com/questions/16029103/three-js-using-2d-texture-sprite-for-animation-planegeometry/16039034#16039034
                var timer, frameWidth, frameHeight, 
                x = 0, y = 0, count = 0, startFrame = 0, 
                endFrame = _endFrame || framesX * framesY,
                canvas = document.createElement('canvas'),
                ctx = canvas.getContext('2d'),
                canvasTexture = new THREE.CanvasTexture(canvas),
                img = new Image();
                img.onload = function(){
                    canvas.width = frameWidth = img.width / framesX;
                    canvas.height = frameHeight = img.height / framesY;
                    timer = setInterval(nextFrame, frameDelay);
                }
                img.src = imageURL;
                function nextFrame() {
                    count++;

                    if(count >= endFrame ) {
                        count = 0;
                    };

                    x = (count % framesX) * frameWidth;
                    y = ((count / framesX)|0) * frameHeight;

                    ctx.clearRect(0, 0, frameWidth, frameHeight);
                    ctx.drawImage(img, x, y, frameWidth, frameHeight, 0, 0, frameWidth, frameHeight);

                    canvasTexture.needsUpdate = true;
                }
                return canvasTexture;
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