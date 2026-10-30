# DJI Mini 2

Declarative aircraft mod `0sfs/dji-mini2`. It depends on `0SFS/DJI-Quadcopter`, which depends on `0SFS/Quadcopter`.

The `DJI-Mini2-propulsion` engine mod owns the motor and propeller data and selects the shared `electric-sound` generator. This repository owns the airframe numbers and the JSBSim file `public/jsbsim-data/aircraft/dji-mini2/dji-mini2.xml`. Mass is 0.249 kg. The still is 4000×3000 with an 83° diagonal field of view. The camera model string written into EXIF is `FC7303`.

The gallery family is `dji-mini2`, an aircraft of its own; DJI Air 2S is a separate aircraft. The flight controller loads only when this aircraft is applied. Gimbal stabilization lives in `camera-gimbal`; camera capture lives in `drone-camera`.

Do not publish this repository from a local task. Topics, when published: `0sfs-mod`, `0sfs-aircraft-mod`. [Modding guide](https://github.com/0SFS/docs/blob/main/modding.md).

```sh
node scripts/validate-mod.mjs
```

## Models and camera

The HD level is the official DJI 3D model. Its source lives in `3d_model/HD/source/` with its textures beside it. Run `npm run prepare:hd` to create the local runtime GLB under `public/3d_model/HD/`. Auto uses HD inside 15 metres and the procedural silhouette farther away; either level can also be selected directly in Aircraft → Model detail. Model bytes are fetched only when that level is needed after applying the aircraft.

The model files are ignored by Git. The flight build links the prepared GLB, so the default modpack ships it on the site. `npm run render:thumbnail` renders the gallery image `public/aircraft/thumbnails/dji-mini2.png` from that GLB with Blender; it shows the model, so it is ignored by Git as well and ships the same way. The repository's AGPL code licence does not relicense the model.

The aircraft manifest declares its own chase distance, zoom limits and near clip. Pilot overrides in the existing Camera settings take precedence.

The airframe selects camera mod `0SFS/DJI-FC7303`, which owns sensor, lens, exposure and metadata data and depends on `0SFS/drone-camera`. The airframe owns gimbal mount limits and sets `controls.vertical` to `climb-rate`: Shift/Ctrl commands ascent/descent while held, A/D yaws, Q/E translates laterally, W/S moves fore/aft. Controls → Keyboard axes overrides those key pairs.

Camera → Roll and Yaw are simulator manual offsets from the stabilized horizon and aircraft heading. The mount data bounds roll to ±35° and yaw to ±20°, using the [manufacturer's mechanical range](https://dl.djicdn.com/downloads/DJI_Mini_2/DJI_Mini_2_User_Manual_EN.pdf). These are simulator overrides; they do not claim the stock remote offers those controls. The gimbal component separates fixed mounting, yaw arm, roll yoke and camera-housing pitch. Spherical capture temporarily centers roll/yaw offsets and restores the pilot targets afterward.
