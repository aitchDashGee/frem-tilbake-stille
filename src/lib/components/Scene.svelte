<script>
  import { T, useTask } from '@threlte/core'
  import { ContactShadows, Float, Grid, OrbitControls, interactivity } from '@threlte/extras'
  import { spring } from 'svelte/motion'

  interactivity()

  export let minSeconds = 5
  export let maxSeconds = 30

  const minY = -1.6
  const maxY = 1.5

  const torusMinY = 2
  const torusMaxY = 5
  
  const redY = spring(maxY)
  const greenY = spring(maxY)
  const blueY = spring(maxY)
  const yellowY = spring(maxY)
  
  // const redY = spring(minY)
  // const greenY = spring(minY)
  // const blueY = spring(minY)
  // const yellowY = spring(minY)

  const torusY = spring(torusMaxY)

  let timeoutId = 0

  let rotation = 0
  useTask((delta) => {
    rotation += delta
  })

  // const element = document.getElementById("root")
  // element.requestFullscreen()

  console.log("Doing stuff...")

  const startTimer = () => {
    const duration = Math.round(Math.random() * (maxSeconds-minSeconds)) + minSeconds

    console.log("Duration: " + duration)

    showTorus()

    timeoutId = setTimeout(() => {
      timeoutId = 0
      hideTorus()
      showRandomCylinder()
    }, duration * 1000)
  }

  const timerIsRunning = () => {
    return timeoutId > 0
  }

  const stopTimer = () => {
    if (timeoutId) {
      clearTimeout(timeoutId)
      timeoutId = 0
    }
  }

  const showRandomCylinder = () => {
    const cylinderIndex = Math.floor(Math.random()*4)

    console.log("Cylinder index " + cylinderIndex)

    switch (cylinderIndex) {
      case 0:
        redY.set(maxY)
        break
      
      case 1:
        greenY.set(maxY)
        break

      case 2:
        blueY.set(maxY)
        break

      case 3:
        yellowY.set(maxY)
        break
    }
  }

  const showCylinders = () => {
    redY.set(maxY)
    greenY.set(maxY)
    blueY.set(maxY)
    yellowY.set(maxY)
  }

  const hideCylinders = () => {
    redY.set(minY)
    greenY.set(minY)
    blueY.set(minY)
    yellowY.set(minY)
  }

  const showTorus = () => {
    torusY.set(torusMinY)
  }

  const hideTorus = () => {
    torusY.set(torusMaxY)
  }

  const toggleCylinder = () => {
    const cylinderIndex = Math.floor(Math.random()*3)

    switch (cylinderIndex) {
      case 0:
        redY.update((y) => {
          return y >= 0 ? minY : maxY
        })
        break
      
      case 1:
        greenY.update((y) => {
          return y >= 0 ? minY : maxY
        })
        break

      case 2:
        blueY.update((y) => {
          return y >= 0 ? minY : maxY
        })
        break
    }

    torusY.update((y) => {
      return y > torusMinY ? torusMinY : torusMaxY
    })
  }
</script>

<!-- <T.PerspectiveCamera
  makeDefault
  position={[-10, 10, 10]}
  fov={15}
  on:create={({ ref }) => {
    ref.lookAt(0, 1.5, 0)
  }}
/> -->
<T.PerspectiveCamera
  makeDefault
  position={[0, 4, 15]}
  fov={15}
  >
  <OrbitControls
    autoRotate={false}
    enableZoom={true}
    enabled={false}
    enableDamping
    autoRotateSpeed={0.5}
    target.y={1.5}
  />
  <!-- <OrbitControls
    autoRotate
    enableZoom={true}
    enableDamping
    autoRotateSpeed={0.5}
    target.y={1.5}
  /> -->
</T.PerspectiveCamera>

<T.DirectionalLight
  intensity={0.8}
  position.x={5}
  position.y={10}
  position.z={10}
  castShadow
/>
<T.AmbientLight intensity={0.2} />

<Grid
  position.y={-0.001}
  cellColor="#ffffff"
  sectionColor="#ffffff"
  sectionThickness={0}
  fadeDistance={25}
  cellSize={2}
/>

<!-- <ContactShadows
  scale={10}
  blur={2}
  far={2.5}
  opacity={0.5}
/> -->

<!-- <Float
  floatIntensity={1}
  speed={20}
  floatingRange={[0, .1]}
> -->
  <T.Mesh
    position.x={2.25}
    position.y={$greenY}
    rotation.x={0}
    rotation.y={0}
    castShadow
    >
    <T.CylinderGeometry args={[0.5, 0.5, 3, 50, 12]} />
    <T.MeshStandardMaterial color="#22F851" />
  </T.Mesh>
<!-- </Float> -->

<!-- <Float
  floatIntensity={1}
  speed={20}
  floatingRange={[0, .1]}
> -->
  <T.Mesh
    position.x={0.75}
    position.y={$yellowY}
    rotation.x={0}
    rotation.y={0}
    castShadow
    >
    <T.CylinderGeometry args={[0.5, 0.5, 3, 50, 12]} />
    <T.MeshStandardMaterial color="#F8F822" />
  </T.Mesh>
<!-- </Float> -->

<!-- <Float
  floatIntensity={1}
  speed={20}
  floatingRange={[0, .1]}
> -->
<T.Mesh
position.x={-0.75}
position.y={$redY}
rotation.x={0}
rotation.y={0}
castShadow
>
<T.CylinderGeometry args={[0.5, 0.5, 3, 50, 12]} />
<T.MeshStandardMaterial color="#F85122" />
</T.Mesh>
<!-- </Float> -->

<!-- <Float
  floatIntensity={1}
  speed={20}
  floatingRange={[0, .1]}
> -->
  <T.Mesh
    position.x={-2.25}
    position.y={$blueY}
    rotation.x={0}
    rotation.y={0}
    castShadow
    >
    <T.CylinderGeometry args={[0.5, 0.5, 3, 50, 12]} />
    <T.MeshStandardMaterial color="#2251F8" />
  </T.Mesh>
<!-- </Float> -->

<T.Mesh
  rotation.x={3* Math.PI / 2}
  receiveShadow
  on:click={(e) => {
    if (timerIsRunning()) {
      stopTimer()
      hideTorus()
    }
    else {
      hideCylinders()
      showTorus()
      startTimer()
    }
  }}
>
  <T.CircleGeometry args={[4, 40]}/>
  <T.MeshStandardMaterial color="white" />
</T.Mesh>

<T.Mesh
  position.x={0}
  position.y={$torusY}
  rotation.x={rotation}
  rotation.y={rotation}
  castShadow
  >
  <T.TorusKnotGeometry args={[0.35, 0.15, 100, 12, 2, 3]}/>
  <T.MeshStandardMaterial color="white" />
</T.Mesh>
