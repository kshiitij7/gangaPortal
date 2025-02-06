<template>
    <div ref="globeContainer" class="globe-container"></div>
  </template>
  
  <script>
  import * as THREE from 'three'
  import { ref, onMounted, onUnmounted, } from 'vue'
  import gsap from 'gsap'
  
  export default {
    setup(_, { emit }) {
      let scene, camera, renderer, globe, clouds, animationFrameId
      const globeContainer = ref(null)
  
      const createGlobe = () => {
        scene = new THREE.Scene()
        camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
        camera.position.z = 5
  
        renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
        if (globeContainer.value) {
          globeContainer.value.appendChild(renderer.domElement)
          setRendererSize()
        }
  
        // Load Textures
        const textureLoader = new THREE.TextureLoader()
        const earthTexture = textureLoader.load('https://threejs.org/examples/textures/land_ocean_ice_cloud_2048.jpg')
        const cloudTexture = textureLoader.load('https://threejs.org/examples/textures/fair_clouds_4k.png')
  
        // Create Earth Sphere
        const geometry = new THREE.SphereGeometry(2, 64, 64)
        const material = new THREE.MeshPhongMaterial({ map: earthTexture })
        globe = new THREE.Mesh(geometry, material)
        scene.add(globe)
  
        // Create Cloud Layer
        const cloudGeometry = new THREE.SphereGeometry(2.05, 64, 64)
        const cloudMaterial = new THREE.MeshStandardMaterial({ map: cloudTexture, transparent: true, opacity: 0.3 })
        clouds = new THREE.Mesh(cloudGeometry, cloudMaterial)
        scene.add(clouds)
  
        // Lighting
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1)
        directionalLight.position.set(5, 5, 5)
        scene.add(ambientLight)
        scene.add(directionalLight)
  
        // Zoom-in effect
        gsap.to(camera.position, {
          z: 2,
          duration: 3,
          ease: "power2.inOut",
          onComplete: () => {
            emit('globeLoaded') // Notify the parent that the globe has loaded
          }
        })
  
        animateGlobe()
      }
  
      const animateGlobe = () => {
        if (!scene || !camera || !renderer || !globe) return
        globe.rotation.y -= 0.027
        clouds.rotation.y += 0.0017
        renderer.render(scene, camera)
        animationFrameId = requestAnimationFrame(animateGlobe)
      }
  
      const setRendererSize = () => {
        if (globeContainer.value) {
          const containerWidth = globeContainer.value.clientWidth
          const containerHeight = globeContainer.value.clientHeight
          renderer.setSize(containerWidth, containerHeight)
          camera.aspect = containerWidth / containerHeight
          camera.updateProjectionMatrix()
        }
      }
  
      // Update renderer size when window is resized
      window.addEventListener('resize', setRendererSize)
  
      onMounted(createGlobe)
  
      onUnmounted(() => {
        if (animationFrameId) cancelAnimationFrame(animationFrameId)
        if (renderer) renderer.dispose()
        window.removeEventListener('resize', setRendererSize)
      })
  
      return { globeContainer }
    }
  }
  </script>
  
  <style scoped>
  .globe-container {
    position: absolute;
    width: 100%;
    height: 85vh; /* Set the height to 85% of the viewport height */
    background: radial-gradient(circle at center, #0a0a33, #000000);
  }
  </style>
  
