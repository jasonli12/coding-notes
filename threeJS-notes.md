ThreeJS Notes

Great tutorial links:
https://threejs.org/docs/index.html#manual/introduction/Creating-a-scene
https://codepen.io/rachsmith/post/beginning-with-3d-webgl-pt-1-the-scene

```JavaScript

var scene = new THREE.Scene();
// A scene is where you place objects and lighting.

var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

// A camera simulates the behavior of a film camera in real life. The position of the camera and direction it is facing will determine the parts of the scene that gets rendered to the screen.

// THREE.PerspectiveCamera(fov, aspect, near, far)

// field of view (fov) = 75; The vertical fov. This dictates the size of the vertical space your camera's view can reach.

// aspect = window.innerWidth/window.innerHeight; This is the aspect ratio used to determine the horizontal fov based on the vertical fov.

// near = 0.1; This is the nearest plane of view (where the camera's view begins)

// far = 1000; This is the far plane of view (where the camera's view ends)

// near and far clipping panes are z-plane coordinates.

var renderer = new THREE.WebGLRenderer();
// A renderer is a view that contains your camera's picture

renderer.setSize(window.innerWidth, window.innerHeight);
// We want the renderer to fill up our app (i.e. the browser window)
// For performance intensive apps, you can consider setting smaller values (i.e. window.innerWidth/2, window.innerHeight/2).
// You can also add false as a third argument to render at a lower resolution.

document.body.appendChild(renderer.domElement);
// Add the renderer element to the DOM so it is on our page (this is a canvas element) The HTML <canvas> element is used to draw graphics, on the fly, via JavaScript. The <canvas> element is only a container for graphics. You must use JavaScript to actually draw the graphics. Canvas has several methods for drawing paths, boxes, circles, text, and adding images.

var geometry = new THREE.BoxGeometry(1, 1, 1);
// BoxGeometry is an object that contains all the points (vertices) and fill (faces of the cube).

var material = new THREE.MeshBasicMaterial( {color: 0x00ff00} );

// MeshBasicMaterial colors the object. It tkaes an object of properties like CSS styling.

var cube = new THREE.Mesh(geometry, material);
// Mesh is an object that takes a geometry and applies a material to it, which we then can insert into the screen and move around.

scene.add(cube);
// Adds the object at (0,0,0). We need to move the camera from (0,0,0) so that the camera and cube will not be inside each other.

camera.position.z = 5;
camera.position.x = 2;
camera.position.y = 1;

function animate() {
  requestAnimationFrame(animate);
  cube.rotation.x += 0.1;
  cube.rotation.y += 0.1;
  renderer.render(scene, camera);
}

animate();
//This creates a loop that causes the renderer to draw the scene 60 times per second.

//Lights

// Directional Lights: a large light from far away that shines from one direction (think of the sun).
// Ambient Lights: less of a light source and more of a soft color tint for the scene.
// Point Lights: think of a lightbulb - shines in every direction & has a limited range.
// Spot Lights: just like a spotlight works in real life.
// Hemisphere Lights: an ambient (non directional) light coming from the ceiling and floor of the scene.

var light = new THREE.PointLight(0xFFFF00);
// position the light so it shines on the cube (x, y, z) //
light.position.set(10, 0, 25);
scene.add(light);


```
