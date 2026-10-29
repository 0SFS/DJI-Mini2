# HD model

Official DJI 3D model.

The source model is in `source/` and `textures/`. The model files are not part of the repository. `npm run prepare:hd` creates the local runtime GLB in `public/3d_model/HD/`, preserving materials and textures, converting source units to metres (1 m per source unit) and recentering the scene.

`rotors.json` declares blade groups and motor centres in source coordinates. `gimbal.json` declares separate yaw-arm, roll-yoke and camera-housing pitch joints. The source combines these shells into one mesh, so preparation partitions its disconnected components by triangle indices while retaining all vertex attributes, materials and textures. Component numbers are ordered by descending welded vertex count; expected triangle counts make a changed source fail explicitly. Unassigned components remain attached to their original fixed mount. Only the camera housing and lens follow pitch; the roll yoke follows roll, and the suspension arm follows yaw. Each pivot has its own source-coordinate centre, and preparation preserves the complete rest-world geometry.
