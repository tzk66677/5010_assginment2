# 5010_assginment2
# Dyson Sphere with a Hexagonal Global Discrete Grid

This document describes how to create a static p5.js sketch that visualizes a Dyson Sphere using a **hexagonal global discrete grid** style. Below, you will find:

1. **Conceptual Instructions (Fluxus-Style)** — a set of non-code, English instructions outlining how to compose the artwork.
2. **p5.js Code** — a JavaScript implementation that follows these instructions in the [p5.js Web Editor](https://editor.p5js.org/).

---

## 1. Conceptual Instructions (Non-Code)

1. **Create a Square Canvas and Black Background**  
   - The canvas represents your scene in space.  
   - Use a black background to symbolize the darkness of outer space.

2. **Central Star (Bright Circle)**  
   - In the center of the canvas, draw a large glowing circle to represent the star.  
   - Use a bright color (e.g., yellowish-white) and consider layering multiple translucent ellipses to simulate a radiant halo.

3. **Hexagonal Grid Over a Spherical Boundary**  
   - Generate a hexagonal grid that could cover the entire canvas.  
   - Only show the parts of this grid that fall within a larger circle (the “Dyson Sphere” boundary) encompassing the star.  
   - Each hexagon can have a faint outline or fill in futuristic colors (teal, blue, green, etc.).  
   - Stagger the columns to form a proper honeycomb layout.

4. **Additional Shells or Layers**  
   - Optionally, draw multiple rings or shells at various radii to depict a multi-layer Dyson Sphere structure.  
   - Vary colors, brightness, or transparency to distinguish each layer.

5. **Starry Background**  
   - Scatter small white dots or faint flickers in the background to represent distant stars, enhancing the cosmic feel.

6. **Static Composition**  
   - Once all elements are drawn, the final image remains stationary (no animation).

---

## 2. p5.js Implementation

Below is a sample code snippet in p5.js that follows the above instructions. Feel free to modify colors, sizes, or the number of elements to suit your artistic goals.

```javascript
function setup() {
  createCanvas(800, 800);
  noLoop();  // Static, no animation
}

function draw() {
  background(0); // 1. Black background, representing outer space
  
  // 2. Central "Star" (a brighter circle)
  // ----------------------------------------------------------------------
  let starX = width / 2;
  let starY = height / 2;
  let starDiameter = 200;
  
  // Glowing star - create multiple translucent ellipses for a halo effect
  noStroke();
  for (let r = 150; r > 0; r -= 10) {
    fill(255, 230, 100, 30); // Semi-transparent yellow
    ellipse(starX, starY, r * 2);
  }
  fill(255, 255, 150);
  ellipse(starX, starY, starDiameter);

  // 3. Hexagonal grid covering the sphere
  // ----------------------------------------------------------------------
  // We will generate a hexagonal grid across the entire canvas,
  // but only render hexagons whose centers lie within the Dyson Sphere boundary.
  
  let sphereRadius = 300;  // Visible outer radius of the Dyson Sphere
  let hexSize = 15;        // Distance from hex center to a vertex
  let rowHeight = hexSize * Math.sqrt(3);
  
  // Horizontal spacing between columns in a hex grid
  let colWidth = hexSize * 1.5;

  // Iterate over rows and columns to place hexagons
  for (let row = 0; row < height / rowHeight + 2; row++) {
    for (let col = 0; col < width / colWidth + 2; col++) {
      
      // Stagger columns for even/odd rows
      let hexCenterX = col * colWidth + (row % 2) * (colWidth / 2);
      let hexCenterY = row * rowHeight;

      // Slight offset adjustments to position the grid near the canvas center
      hexCenterX -= (width / 2) * 0.2; 
      hexCenterY -= (height / 2) * 0.2; 
      
      // Translate the entire grid so it’s roughly centered
      let cx = hexCenterX + 50;
      let cy = hexCenterY + 50;

      // Check if this hexagon falls within the Dyson Sphere boundary
      let distFromCenter = dist(cx, cy, width / 2, height / 2);
      if (distFromCenter < sphereRadius) {
        // Assign a "techy" color palette
        let hueVal = random(120, 180);
        fill(hueVal, 200, 255, 100);
        stroke(hueVal, 255, 255, 150);
        strokeWeight(1);

        // Draw the hexagon
        drawHexagon(cx, cy, hexSize);
      }
    }
  }

  // 5. Random starry background
  // ----------------------------------------------------------------------
  // Add small dots to represent distant stars
  for (let i = 0; i < 200; i++) {
    let sx = random(width);
    let sy = random(height);
    let sSize = random(1, 3);
    noStroke();
    fill(255, random(100, 255));
    ellipse(sx, sy, sSize);
  }

  // Optional: outline to visualize the Dyson Sphere boundary
  noFill();
  stroke(150, 200, 255);
  strokeWeight(2);
  ellipse(width / 2, height / 2, sphereRadius * 2);
}

// Utility function: draw a 6-sided hexagon at (cx, cy) with radius r
function drawHexagon(cx, cy, r) {
  beginShape();
  for (let i = 0; i < 6; i++) {
    let angle = TWO_PI / 6 * i + PI / 6; // Rotate to make one side horizontal
    let x = cx + cos(angle) * r;
    let y = cy + sin(angle) * r;
    vertex(x, y);
  }
  endShape(CLOSE);
}
```
## Sample Image

<img width="600" alt="sample" src="https://github.com/user-attachments/assets/5c4cb24a-1e08-46db-9b21-06f9f9744123" />

