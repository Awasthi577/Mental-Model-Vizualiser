# Axion – A System for Better Thinking

Axion is an interactive, browser-based application designed to help you understand and apply powerful mental models and cognitive biases. It uses interactive 3D visualizations and AI-powered examples to create a hands-on learning experience.

---

## Preview Video
A brief demonstration of the interactive features in Axion.

[![Watch the Preview Video](https://github.com/Awasthi577/Mental-Model-Vizualiser/blob/Assets/Axion_Image.jpg?raw=true)](https://drive.google.com/file/d/1kAx4vTDKnkxprU3JUIylP7abMbOaCZOJ/view?usp=sharing)

A brief demonstration of the interactive features in Axion.
---

## Features

- **Interactive 3D Visualizations**: Explore concepts like *Inversion*, *Sunk Cost Fallacy*, and more with hands-on 3D models.  
- **AI-Powered Examples**: Use the “Ask Axion” feature (powered by the Gemini API) to get unlimited, context-specific examples for any model.  
- **Dynamic Reflection System**: Receive new journaling prompts each time you visit a concept and keep a history of your thoughts.  
- **360° View & Zoom**: All visualizations include full camera controls (drag to rotate, scroll to zoom).  
- **Self-Contained**: Runs entirely in the browser from a single HTML file. No installation or dependencies are required.  
- **Responsive Design**: Works on desktop and mobile devices.

---

## Installation & Running

1. **Download** the `axion.html` file to your local machine.  
2. **Open in Browser**: Double-click the file to open in Chrome, Firefox, Safari, or Edge.  

That’s it! The application runs entirely locally.

---

## How It Works

The logic is contained within a single `<script>` tag in the HTML file.

### Content Storage
- **`mosaicData` Object**: Contains all mental models and cognitive biases, each with definition, application, and reflection questions.

### Rendering Engine
- `renderMosaic()` dynamically creates the HTML for each category/tile.

### Modal Management
- `openModal()` and `closeModal()` handle detailed tile views.  
- `switchTab()` changes the active tab inside the modal.

### Visualization Engine
- `initVisualization()` creates a new **Three.js** scene, camera, and renderer.  
- Uses a **`ResizeObserver`** to fix the blank-canvas issue when rendering in hidden tabs.  
- Individual setup functions (e.g., `setupInversionVisual()`) define object creation and interactivity.  
- `addGenericControls()` provides drag-rotate and zoom controls.

---

### Core Visualization Logic

```javascript
function initVisualization(tileId) {
    destroyVisualization(); 
    
    const mount = document.getElementById('visual-container');
    if (!mount) return;

    // ... scene, camera, renderer setup ...

    const vizInstance = setupFunction(scene, camera, renderer, controlsContainer, mount);

    let animationFrameId;
    function animate() {
        animationFrameId = requestAnimationFrame(animate);
        if (vizInstance && typeof vizInstance.update === 'function') {
            vizInstance.update();
        }
        renderer.render(scene, camera);
    }
    animate();

    const resizeObserver = new ResizeObserver(() => {
        const width = mount.clientWidth;
        const height = mount.clientHeight;
        camera.aspect = width / height;
        camera.updateProjectionMatrix();
        renderer.setSize(width, height);
    });
    resizeObserver.observe(mount);

    activeVisualization = {
        destroy: () => {
            cancelAnimationFrame(animationFrameId);
            resizeObserver.disconnect();
            // cleanup logic...
        }
    };
}
```

### Customizing Content

One modify the mosaicData array to adding their own models:

```
{
    id: "your-unique-id",
    hasVisual: true,
    title: "Your New Model",
    definition: "A clear and concise definition of the concept.",
    application: "A real-world example of the concept in action.",
    reflections: [
        "A good first reflection question.",
        "A second question.",
        "A third question."
    ]
}
```

### Future Development

1. Connecting tiles to form a “latticework” of mental models.

2. User accounts & cloud sync.

3. Custom tiles and notes.

4. Community-shared mosaics.

### Acknowledgments

1. Three.js

2. Tailwind CSS

3. Lucide Icons

4. Google Gemini API


