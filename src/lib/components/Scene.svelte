<script>
  import { T, useTask } from '@threlte/core'
  import { ContactShadows, Float, Grid, OrbitControls, interactivity } from '@threlte/extras'
  import { spring } from 'svelte/motion'

  interactivity()
  const scale = spring(1)

  let rotation = 0
  useTask((delta) => {
    rotation += delta
  })
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
  position={[-10, 10, 10]}
  fov={15}
>
  <OrbitControls
    autoRotate={false}
    enableZoom={false}
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

<Float
  floatIntensity={1}
  floatingRange={[0, 1]}
>
  <T.Mesh
    position.y={1.2}
    position.z={-0.75}
    castShadow
    >
    <T.BoxGeometry />
    <T.MeshStandardMaterial color="#0059BA" />
  </T.Mesh>
</Float>

<Float
  floatIntensity={1}
  floatingRange={[0, 1]}
>
  <T.Mesh
    position={[1.2, 1.5, 0.75]}
    rotation.x={5}
    rotation.y={71}
    castShadow
    >
    <T.TorusKnotGeometry args={[0.5, 0.15, 100, 12, 2, 3]} />
    <T.MeshStandardMaterial color="#F85122" />
  </T.Mesh>
</Float>

<Float
  floatIntensity={1}
  floatingRange={[0, 1]}
>
  <T.Mesh
    position={[-1.4, 1.5, 0.75]}
    rotation={[-5 + rotation, 128 + rotation, 10]}
    scale={$scale}
    on:pointerenter={() => scale.set(1.5)}
    on:pointerleave={() => scale.set(1)}
    castShadow
    >
    <T.IcosahedronGeometry />
    <T.MeshStandardMaterial color="#F8EBCE" />
  </T.Mesh>
</Float>

<T.Mesh
  rotation.x={3* Math.PI / 2}
  receiveShadow
>
  <T.CircleGeometry args={[4, 40]}/>
  <T.MeshStandardMaterial color="white" />
</T.Mesh>
