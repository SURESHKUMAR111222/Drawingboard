1. Imports and Constants

roughjs: A library that makes shapes look hand-drawn (sketchy).

perfect-freehand: A library that calculates smooth, pressure-sensitive paths for the pencil tool.

generator: A RoughJS utility used to define the geometry of shapes before drawing them.

2. Element Creation (createElement)

This function is a "factory." Based on the tool selected (line, rectangle, pencil, text), it returns a JavaScript object containing coordinates and specific properties.

Pencil: Stores an array of points.

Text: Stores a text string.

3. Hit Detection (Math Logic)

These functions determine where the mouse is relative to a shape:

nearPoint: Checks if the mouse is hovering over a specific point (like a corner) for resizing.

onLine: Uses the mathematical formula for a line to check if the mouse is clicking "on" a line.

positionWithinElement: This is the "brain" of selection. It checks if the mouse is:

At a corner (to resize).

Inside the shape (to move it).

On the boundary.

4. Selection and Coordination

getElementAtPosition: Loops through all drawn elements to find which one you just clicked on.

adjustElementCoordinates: If you draw a rectangle from bottom-right to top-left, this function flips the coordinates so x1, y1 is always the top-left and x2, y2 is the bottom-right.

cursorForPosition: Changes the mouse cursor (e.g., to a diagonal arrow nwse-resize) when hovering over corners.

5. Custom Hooks

useHistory: This manages Undo/Redo. It keeps an array of "snapshots" of your drawing. When you draw something, it adds a new snapshot. undo simply moves the index back one step.

usePressedKeys: Tracks which keyboard keys are held down (used for the Spacebar to pan the canvas).

6. Drawing Logic (drawElement)

This function actually paints on the HTML5 Canvas:

If it's a Line/Rect, it uses roughCanvas.draw().

If it's Pencil, it uses getStroke to create a smooth path and fills it using Path2D.

If it's Text, it uses the standard canvas fillText method.

7. The Main Component (App)

This is where the state and events are managed.

State Management:

elements: The list of everything drawn.

action: What the user is doing (drawing, moving, resizing, panning, writing, or none).

panOffset: Stores how much the user has scrolled/panned the canvas.

The Rendering Loop (useLayoutEffect):

This runs every time a change happens. It:

Clears the entire canvas.

Applies the panOffset (translation).

Loops through the elements array and calls drawElement for each one.

Event Handlers:

handleMouseDown:

If the tool is Selection, it finds the element clicked.

If the tool is a Shape, it creates a new element at the mouse position.

If Spacebar is held, it starts Panning.

handleMouseMove:

If Drawing: Updates the width/height of the new shape.

If Moving: Calculates the difference in mouse movement and updates element coordinates.

If Resizing: Updates only the specific corner being dragged.

handleMouseUp: Finalizes the action. It "fixes" coordinates (via adjustElementCoordinates) and sets the action back to none.

8. Text Handling

When the text tool is used:

handleMouseDown creates a text element.

The UI renders a hidden <textarea> exactly at that position.

When the user clicks away (onBlur), the text from the textarea is saved into the element, and the textarea disappears.

9. The JSX (The UI)

Toolbar: Radio buttons to switch tools (Line, Pencil, etc.).

Action Bar: Undo/Redo buttons.

Canvas: The main drawing surface that fills the screen.

Summary of Workflow

User Clicks 
→
→
 handleMouseDown identifies tool or existing element.

User Drags 
→
→
 handleMouseMove updates coordinates in state.

State Changes 
→
→
 useLayoutEffect triggers.

Canvas Redraws 
→
→
 Everything is cleared and repainted instantly, creating the illusion of smooth movement.
