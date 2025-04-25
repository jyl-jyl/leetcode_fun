## Sliding Windows
Leetcode 209

> Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

Sliding windows has two ptrs: start_index, end_index
We need to move the end_index anyway
For each end_index, we need to find all the start_index that fullfill the requirement in the prompt

```
int minSubArrayLen(int s, vector<int>& nums) {
        int result = INT_MAX;
        int sum = 0;
        int i = 0; // start
        for (int j = 0; j < nums.size(); ++j)
        {
            sum += nums[j];
            while(sum >= s)
            {
                auto len = j-i+1;
                result = min(result, len);
                sum -= nums[i];
                i++;
            }
        }
        return result == INT_MAX ? 0 : result;
    }
```

## Spiral Matrix
Leetcode 59
This one is more of a simulation question but an regular algorithm question. BUT, I asked ChatGPT to create an animation for me

<details>
  <summary>Code (html)</summary>
  You can just saved the following html in a file then open it in the browser
  
  ```html
  <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Spiral Matrix Animation - Colored Cubes</title>
  <style>
    body { margin: 0; overflow: hidden; background: #111; }
    canvas { display: block; }
  </style>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>

  <script>
    const n = 5; // Matrix size
    const cubeSize = 1;
    const spacing = 0.2;
    const cubes = [];
    const steps = [];

    // Scene setup
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.set(n / 2, n, n * 1.5);
    camera.lookAt(n / 2, 0, n / 2);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    // Lights
    const ambient = new THREE.AmbientLight(0xffffff, 0.6);
    scene.add(ambient);
    const directional = new THREE.DirectionalLight(0xffffff, 0.8);
    directional.position.set(10, 10, 10);
    scene.add(directional);

    // Create grid
    const geometry = new THREE.BoxGeometry(cubeSize, cubeSize, cubeSize);

    for (let i = 0; i < n; i++) {
      cubes[i] = [];
      for (let j = 0; j < n; j++) {
        const material = new THREE.MeshStandardMaterial({ color: 0x222222 }); // dark initial
        const cube = new THREE.Mesh(geometry, material);
        cube.scale.y = 0.01;
        cube.position.set(
          j * (cubeSize + spacing),
          0,
          i * (cubeSize + spacing)
        );
        scene.add(cube);
        cubes[i][j] = cube;
      }
    }

    // Spiral steps
    function generateSpiralSteps(n) {
      let startx = 0, starty = 0;
      let loop = Math.floor(n / 2);
      let offset = 1;
      let count = 1;
      let i, j;

      while (loop--) {
        i = startx;
        j = starty;
        for (; j < n - offset; j++) steps.push([i, j, count++]);
        for (; i < n - offset; i++) steps.push([i, j, count++]);
        for (; j > starty; j--) steps.push([i, j, count++]);
        for (; i > startx; i--) steps.push([i, j, count++]);
        startx++;
        starty++;
        offset++;
      }

      if (n % 2 === 1) {
        let mid = Math.floor(n / 2);
        steps.push([mid, mid, count]);
      }
    }

    generateSpiralSteps(n);

    // Helper to get a random bright color
    function getRandomColor() {
      const hue = Math.floor(Math.random() * 360);
      return new THREE.Color(`hsl(${hue}, 100%, 50%)`);
    }

    // Animate spiral
    let stepIndex = 0;
    const animationSpeed = 300;

    function animateSpiral() {
      if (stepIndex >= steps.length) return;

      const [i, j, num] = steps[stepIndex];
      const cube = cubes[i][j];
      let t = 0;

      // Assign random color
      cube.material.color = getRandomColor();

      // Animate height
      const grow = () => {
        t += 0.05;
        cube.scale.y = Math.min(t, 1);
        cube.position.y = cube.scale.y / 2;
        if (t < 1) {
          requestAnimationFrame(grow);
        }
      };
      grow();

      stepIndex++;
      setTimeout(animateSpiral, animationSpeed);
    }

    function render() {
      requestAnimationFrame(render);
      renderer.render(scene, camera);
    }

    animateSpiral();
    render();
  </script>
</body>
</html>
  ```
</details>  

<img src="https://github.com/user-attachments/assets/f2684115-ee23-4ad7-ba8f-e68dea3b62ec" alt="Spiral Matrix Preview" height="400"/>

