**Drawingboard**

A web-based interactive drawing board built with React and HTML5 Canvas, featuring hand-drawn style shapes, freehand drawing, text input, element selection, resizing, panning, and undo/redo functionality.

** Features**
   Hand-drawn sketch style using RoughJS
   Freehand pencil drawing with smooth strokes
   Draw lines, rectangles, and text
   Select, move, resize, and edit elements
   Canvas panning using keyboard + mouse
   Undo and Redo support
   Real-time canvas re-rendering

**Libraries Used**
    roughjs
    Creates sketchy, hand-drawn looking shapes.
    perfect-freehand
    Generates smooth, pressure-sensitive freehand strokes for the pencil tool.
    HTML5 Canvas API

Used for rendering all drawing elements.

 Core Concepts Explained
1. Imports and Constants
roughjs is used to render sketch-style shapes.
perfect-freehand calculates smooth paths for pencil drawings.
generator from RoughJS defines shape geometry before drawing.

2. Element Creation (createElement)

A factory function that creates drawing elements based on the selected tool:
Line / Rectangle → Stores start and end coordinates
Pencil  Stores an array of points
Text  Stores coordinates and a text string

Each element is stored as a JavaScript object in state.

3. Hit Detection (Mathematical Logic)
These functions determine the mouse position relative to an element:
nearPoint
Detects if the mouse is near a corner (used for resizing).
onLine
Uses a line distance formula to check if the mouse is on a line.
positionWithinElement
Determines whether the cursor is:
On a corner (resize)
Inside the shape (move)
On the boundary
This logic enables accurate selection and interaction.

4. Selection and Coordination
getElementAtPosition
Iterates through all elements to find the one under the cursor.
adjustElementCoordinates
Normalizes coordinates so:
x1, y1 is always the top-left
x2, y2 is always the bottom-right
cursorForPosition
Dynamically changes the mouse cursor (e.g., resize arrows).

5. Custom Hooks
useHistory
Manages Undo / Redo functionality:
Stores snapshots of element states
Undo moves backward in history
Redo moves forward
usePressedKeys
Tracks keyboard input:
Used mainly for Spacebar panning

6. Drawing Logic (drawElement)
Responsible for rendering elements onto the canvas:
Line / Rectangle
Drawn using roughCanvas.draw()
Pencil
Uses getStroke() and Path2D for smooth freehand paths
Text
Rendered using canvas.fillText()

7. Main Component (App)
Handles all application state and events.
State Management
elements  All drawn items
action  Current user action (draw, move, resize, pan, write)
panOffset  Canvas translation values

9. Rendering Loop (useLayoutEffect)
Triggered whenever state changes:
Clears the canvas
Applies panning translation
Redraws all elements
This ensures smooth real-time rendering.

9. Event Handlers
handleMouseDown
Selects an element (Selection tool)
Creates a new element (Shape tools)
Starts panning if Spacebar is pressed
handleMouseMove
Updates element size while drawing
Moves elements during drag
Resizes specific corners
handleMouseUp
Finalizes the action
Fixes coordinates
Resets the action state

10. Text Handling
Clicking creates a text element
A hidden <textarea> appears at the click position
On blur, text is saved to the element
Textarea is removed after input

12. User Interface (JSX)
Toolbar
Tool selection (Line, Rectangle, Pencil, Text, Select)
Action Bar
Undo / Redo buttons
Canvas
Full-screen drawing surface
