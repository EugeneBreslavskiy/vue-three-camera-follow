<template>
  <section class="scene" ref="scene"></section>
</template>

<script>
    import * as THREE from 'three'
  
    export default {
        name: 'Scene',
        data() {
            return {
                container: null,
                camera: null,
                controls: null,
                scene: null,
                renderer: null,
                mouse: new THREE.Vector2()
            }
        },
        methods: {
            init() {
                this.container = this.$refs.scene
                
                this.scene = new THREE.Scene()
                this.scene.background = new THREE.Color( 0xffffff )
                
                this.createCamera()
                this.createLights()
                this.createRenderer()
                this.createObject()
                
                this.renderer.setAnimationLoop(() => {
                    this.update()
                    this.render()
                })

                window.addEventListener( 'resize', this.resizeWindowHandler, false )
                
                window.addEventListener( 'mousemove', this.mouseMoveHandler, false )
            },
            createCamera() {
                this.camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 1, 2000 )
                this.camera.position.set( 0, 0, 10 )
            },
            createLights() {
                const ambientLight = new THREE.AmbientLight( 0xffffff, 0.8 )

                this.scene.add( ambientLight )
            },
            createRenderer() {
                this.renderer = new THREE.WebGLRenderer( { antialias: true, alpha: true } )
                this.renderer.setSize( window.innerWidth, window.innerHeight )
                this.renderer.setPixelRatio( window.devicePixelRatio )

                this.container.appendChild( this.renderer.domElement )
            },
            createObject() {
                const textureLoader = new THREE.TextureLoader()

                textureLoader.load(
                    '/static/img/building.jpg',
                    
                    ( texture ) => {
                        let geometry = new THREE.PlaneGeometry( 12, 12 )
                        let material = new THREE.MeshBasicMaterial( { map: texture } )
                        let plane = new THREE.Mesh( geometry, material )
                        plane.position.set( 0, -2, 0 )
                        
                        this.scene.add( plane )
                    }
                )
                
                const fontLoader = new THREE.FontLoader()
                
                fontLoader.load(
                    '/static/fonts/FrankRuhlLibreBlack.json',

                    ( font ) => {

                        let matLite = new THREE.MeshBasicMaterial( {
                            color: 0x1a1a1a
                        } )

                        let shapes = font.generateShapes( 'EMPIRE\nSTATE\nBUILDING', 0.4 )
                        let geometry = new THREE.ShapeBufferGeometry( shapes )

                        let header = new THREE.Mesh( geometry, matLite )
                        header.position.set( 1, 0.5, 5)

                        this.scene.add( header )
                    }
                )
            },
            mouseMoveHandler(event) {
                this.mouse.x = ( event.clientX - window.innerWidth / 2 )
                this.mouse.y = ( event.clientY - window.innerHeight / 2 )
            },
            update() {
                this.camera.rotation.x += 0.05 * ( - this.mouse.y * 0.0008 - this.camera.rotation.x )
                this.camera.rotation.y += 0.05 * ( - this.mouse.x * 0.0008 - this.camera.rotation.y )
            },
            render() {
                this.renderer.render( this.scene, this.camera )
            },
            resizeWindowHandler() {
                this.camera.aspect = window.innerWidth / window.innerHeight
                this.camera.updateProjectionMatrix()

                this.renderer.setSize( window.innerWidth, window.innerHeight )
            }
        },
        mounted() {
            this.init()
        }
    }
</script>

<style lang="sass">
  
  .scene
    width: 100%
    height: 100%
    position: absolute

</style>
