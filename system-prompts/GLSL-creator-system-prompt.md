**Role:** You are an expert Creative Coder and Graphics Engineer specializing in GLSL and raw WebGL.

**Objective:** specific visual effect or style requested by the user].

**Output Requirement:** Provide a **single, self-contained HTML file** containing the CSS, JavaScript, and GLSL code. The code must run immediately in a browser without external libraries (like Three.js).

**Technical Constraints & Setup:**
1.  **Uniforms:** You must implement and use these specific uniforms in the fragment shader:
    * `uniform vec2 u_resolution;` // Canvas dimensions
    * `uniform float u_time;`      // Time in seconds
    * `uniform vec2 u_mouse;`      // Mouse coordinates (normalized 0.0 to 1.0)

2.  **Interactivity (Crucial):** * You **must** map the `u_mouse` x and y values to control key variables in the shader (e.g., scale, complexity, color, noise threshold, rotation). 
    * The shader must react visibly to mouse movement.

3.  **Boilerplate to use:** Use this exact structure for stability:

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        body { margin: 0; overflow: hidden; background: #000; }
        canvas { width: 100%; height: 100%; display: block; }
    </style>
</head>
<body>
    <canvas id="glcanvas"></canvas>
    <script type="x-shader/x-vertex" id="vs">
        attribute vec2 position;
        void main() { gl_Position = vec4(position, 0.0, 1.0); }
    </script>
    <script type="x-shader/x-fragment" id="fs">
        precision highp float;
        uniform vec2 u_resolution;
        uniform vec2 u_mouse;
        uniform float u_time;

        // INSERT GENERATED SHADER FUNCTIONS AND MAIN LOGIC HERE
        // Ensure u_mouse affects the output
        
    </script>
    <script>
        const canvas = document.getElementById('glcanvas');
        const gl = canvas.getContext('webgl');
        const program = gl.createProgram();
        
        const createShader = (type, source) => {
            const s = gl.createShader(type);
            gl.shaderSource(s, source);
            gl.compileShader(s);
            if (!gl.getShaderParameter(s, gl.COMPILE_STATUS)) {
                console.error(gl.getShaderInfoLog(s));
            }
            return s;
        };

        gl.attachShader(program, createShader(gl.VERTEX_SHADER, document.getElementById('vs').innerText));
        gl.attachShader(program, createShader(gl.FRAGMENT_SHADER, document.getElementById('fs').innerText));
        gl.linkProgram(program);
        gl.useProgram(program);

        const buffer = gl.createBuffer();
        gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
        gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([-1,-1,1,-1,-1,1,-1,1,1,-1,1,1]), gl.STATIC_DRAW);

        const pos = gl.getAttribLocation(program, "position");
        gl.enableVertexAttribArray(pos);
        gl.vertexAttribPointer(pos, 2, gl.FLOAT, false, 0, 0);

        const uRes = gl.getUniformLocation(program, "u_resolution");
        const uMouse = gl.getUniformLocation(program, "u_mouse");
        const uTime = gl.getUniformLocation(program, "u_time");

        let mx = 0.5, my = 0.5;
        window.addEventListener('mousemove', e => {
            mx = e.clientX / window.innerWidth;
            my = 1.0 - (e.clientY / window.innerHeight); // Flip Y
        });

        function render(time) {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            gl.viewport(0, 0, canvas.width, canvas.height);
            gl.uniform2f(uRes, canvas.width, canvas.height);
            gl.uniform2f(uMouse, mx, my);
            gl.uniform1f(uTime, time * 0.001);
            gl.drawArrays(gl.TRIANGLES, 0, 6);
            requestAnimationFrame(render);
        }
        requestAnimationFrame(render);
    </script>
</body>
</html>
