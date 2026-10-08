# Annotatorfor3DFiles
Annotator for 3D models -- second version after first version failed. 
---
Prototype below as of Oct.: 

<img width="753" height="773" alt="Screenshot 2026-10-08 at 4 20 03 PM" src="https://github.com/user-attachments/assets/b59789aa-b9be-4fc2-a0e0-b944b44e9634" />

---
Info so far about this tool (files not uploaded for now as I'm severely editing and it won't run on github for obvious reasons): 

1. The tool is meant to be a local authoring tool (Python, runs locally, used to run in Docker) that turns an existing 3D model (.obj, .stl, .glb) into a sealed, interactive viewer. One HTML file showing the model, where clicking highlighted regions reveals associated text and hieroglyphs. The viewer would be embedded on websites with an <iframe>.

2. The export is a self-contained .glb/.html file. Three.js, the GLTF loader, viewer code, styles, the base64 GLB, the project JSON, and fonts are all inlined. No CDN or network needed to run is the goal.

3. One important detail: no editor, file only contains viewer, all edits are _final_ meaning if you screwed up, you gotta do it all over again. I am too ridiculously lazy to go back and fix it, might be for another V0.0 but as of now, since i worked on this thing for MONTHS (actually almost a year -- i had to learn everything from scratch).

4. A SHA-256 of the file's content is written into a <meta> tag and returned by the server so it can be published separately.

5. Remember how i said it's embedded via <iframe>? WEll, well, well ideally with sandbox="allow-scripts". Expected file size will roughly be 1.33x the GLB size (base64). My warning: you might wanna decimate your mesh/lower the amount of vertices/polygons/etc. for larger models unless you've got a super-powerful computer/build unlike me. Even then...*hands up, shrugging*. 

6. Box-select produces face indices. So, when you, the user, drags a rectangle or a lil crayon to highlight something, for each triangle, the tool tests whether it lies inside the rectangle. so back-facing triangles are discarded and triangles hidden beind others are discarded using occlusion testing (BVH accelerated raycasts or GPU ID buffer picking). The remaining triangle indices would be stored in region.faces.

7. Finally, for now and in practice, convert OBJ/STL to GLB _once_ at upload. That GLB is the single source of truth (region selected in the wizard runs on that GLB and loads the same way the viewer loads it, just with different handling). No edits possible after, as mentioned before. The export embeds those exact GLB bytes, so if you want to change the model, you'd have to upload a new one, and all regions would be discarded, so be mindful of your workflow. Then, obviously, expect bugs. 
