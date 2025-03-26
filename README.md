**🔧 Project Summary**
Extended a C++ OpenGL-based 3D visualization framework with real-time environment mapping. Implemented cubemap reflections using custom GLSL shaders, and added interactive controls via ImGui.
**🛠️ Technologies Used**
- C++ (core application logic)
- OpenGL (rendering API)
- GLSL (vertex & fragment shaders)
- ImGui (UI for real-time controls)
- GLFW, GLAD, stb_image (windowing, loading, OpenGL bindings)
- GLM, Eigen (math libraries)
- 
**📌 Key Features Implemented**
- Skybox rendering using six textured faces
- Loading cubemap textures as GL_TEXTURE_CUBE_MAP
- Custom fragment shader with ambient, diffuse, and specular lighting
- Ray reflection using surface normals and sampling cubemap
- Reflection blending: add, multiply, average, brightness-based
- Interactive UI: toggle reflections, choose blending mode

**High-Level Rendering Pipeline Explanation**
The rendering pipeline in the LennyGraphics project follows a typical real-time graphics pipeline with custom enhancements for environment mapping. Below is a high-level overview of what happens in each frame, from input handling to final pixel rendering.
1. Input Handling and UI (CPU-side)
At the start of each frame, user input (mouse, keyboard) and GUI interactions (using ImGui) are processed. These interactions modify application state — for example, toggling the environment mapping feature or selecting a blending mode. These updated values are stored in variables inside the C++ application and are sent to the GPU as shader uniforms.
2. Shader Activation and Uniform Updates
Before rendering begins, the active shader program is activated using OpenGL. Uniforms such as lighting settings, camera position, material options, and flags like `enableEnvironmentMapping` are updated using OpenGL API calls. These uniforms control how the shader behaves during rendering.
3. Skybox Rendering
The skybox is rendered first using six separate textured quads (one for each face of a cube). A special flag `isSkybox` is set to true in the shader to skip lighting and simply render the background texture. This creates the illusion of a surrounding environment, essential for reflecting onto 3D models.
4. Model Rendering with Shading & Reflections
Next, 3D models (robots, arms, etc.) are drawn using the same shader but with `isSkybox = false`. The shader performs lighting calculations using ambient, diffuse, and specular components. If environment mapping is enabled, the shader also reflects a view ray off the surface normal and samples the cubemap texture in that direction. The reflected color is then combined with the surface color using one of several blending modes: add, multiply, average, or brightness-based mix.
5. Final Frame Composition
After all geometry is drawn, the final frame is composed and presented to the screen. OpenGL handles the rasterization and blending behind the scenes, ensuring smooth visuals and real-time performance. If enabled, a reference sphere is also drawn with reflections to visually test the environment mapping quality.
This pipeline demonstrates an integration of CPU-side logic (in C++) with GPU-side parallel computation (in GLSL), a key concept in real-time graphics and modern game/visualization engines.
