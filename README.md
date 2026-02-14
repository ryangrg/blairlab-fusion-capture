# Fusion 360 Rodent-Eye Capture - Blair Lab

Render 128×128 stereo “rodent-eye” images from specified grid positions in an Autodesk Fusion 360 design. Useful for behavioral or robotics simulation datasets where a rodent-like camera model is needed. Made for Blair Lab's research by John Tian with updates and additional notes from Ryan Grgurich.

Important note: North in the render model, which can be established by recognizing that the room
door is in the east, is actually east in the minigrid environment. So grid positions need to be
rotated counter clockwise 90 degrees when mapping the render model to the minigrid environment.

What it does

- Reads grid positions (outlined in the Blair Lab maze positions Google Sheet) from positions.txt
- Maps grid to model coordinates via anchor points in the Fusion 360 design (axes swapped)
- For each grid cell, renders 8 images:
  - Left/right eye, each facing four directions
  - Corner cells use diagonal directions
- Saves images to photos/<file_prefix> with deterministic names

Camera model

- Eye separation: 0.5 inches (±0.25" along right vector)
- Eye height: 33.577" + 2.5"
- Eye yaw: ±50° from the base direction (left/right)
- Pitch: +15°
- Vertical FOV: 150°
- Output: 128×128 PNG
- “North” is the direction from (5,5) toward (5,0), for more information see the Blair Lab maze positions Google Sheet.

File structure

- photos/
- capture.py
- positions.txt

positions.txt format

- First line: file prefix (subfolder under photos)
- Following lines: grid coordinates as x,y (integers)

Example

```
config0
5,7
5,8
5,9
```

Output naming

- photos/<file*prefix>/<file_prefix>*<x>_<y>_<direction>\_<eye>.png
- Directions at non-corner cells: north (0), east (1), south (2), west (3)
- Directions at corner cells (1,1), (11,1), (1,11), (11,11): NE (0), SE (1), SW (2), NW (3)
- Eye: left (l), right (r)
- Example: photos/config0/config0_5_5_0_l.png

Setup and run (For Fusion version 2604.1.25)

1. Make sure to have CaptureView folder on your computer and that it contains the fusion manifest file.
2. Open your design in Fusion 360 (Design workspace).
3. In the window menu bar go to Utilities then from ADD-INNS drop down menu select Scripts and Add-Ins. In the window that appears select the add button located to right of the search box at the top of the window. Select script or Add-Inn from device then go to the CaptureView folder and open it. The script will now be available to use in the Scripts and Add-Ins menu.
4. You specify the configuration and positions you want captured in the position.txt file. See render-map-fusion-maze.xlsx for coordinate details.
5. The script switches to the Render workspace and performs local renders. Images are overwritten if they exist.

Notes

- Coordinate mapping uses provided anchors with linear interpolation:
  - model X from grid y; model Y from grid x; Z = 33.577"
- Requires the Render workspace and local rendering capability:
  - In Fusion 360 UI, the Render workspace must be available (FusionRenderEnvironment).
- If you see errors about rendering or no active design, ensure a design document is open and active before running.
