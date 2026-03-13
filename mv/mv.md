
### mv.json:
Contains MonsterVision-specific configuration data.

example of what `mv.json` may look like: 
```json
{
    "cameras" : [
        { "mxid" : "18443010E1176A1200", "name" : "Front", "invert" : 0, "monoResolution" : "THE_400_P", "rgbResolution" : "THE_720_P" },
        { "mxid" : "18443010A162011300", "name" : "Rear", "invert" : 0, "monoResolution" : "THE_400_P", "rgbResolution" : "THE_1080_P" },
        { "mxid" : "1944301001564D1300", "name" : "Eclipse", "invert" : 0, "monoResolution" : "THE_400_P", "rgbResolution" : "THE_800_P" }
    ],
    "tagFamily" : "tag36h11",
    "tagSize" : 0.1651,
    "CAMERA_FPS" : 25,
    "DS_SUBSAMPLING" : 4,
    "PREVIEW_WIDTH" : 200,
    "PREVIEW_HEIGHT" : 200,
    "DS_SCALE" : 0.5,
    "showPreview" : true
}
```

| Field | Description |
| --- | --- |
|`tagFamily`| The April Tag family such as `tag36h11` or `tag16h5`|
|`tagSize`| The overall size of the tag in meters.|
|`CAMERA_FPS`| The desired frame rate for image capture. |
|`DS_SUBSAMPLING`| To reduce the bandwidth between the drivers station on the Raspberry Pi, you can have MonsterVision send only a subset of frames to the DS.  This allows you to specify a subset of frame to be sent. |
|`PREVIEW_WIDTH`| currently not used. |
|`PREVIEW_HEIGHT`| currently not used. |
|`DS_SCALE`| another way to reduce bandwidth.  Tha RGB camera image (with annotations) is scaled by this factor before being sent to the drivers station. |
|`showPreview`| If True, the `preview` output of the RGB camera is sent to an XLinkOut for eventual display on systems running a GUI. |


#### `cameras` configures how a camera is used on the robot. `cameras` is an array of dictionaries, each containing:

| Field | Description |
| --- | --- |
|`mxid`| matches the unique identifier of the OAK camera. |
|`name`| allows you to assign a "friendly" name to the camera (usually named by location on robot). |
|`invert`| specifies that the camera is mounted upside down on the robot |
|`useDepth`| set to 1 if you want the camera to compute depth using stereo disparity.  Has no effect on April Tag depth calculation. |
|`nnFile`| Specifies the path to the NN configuration file to be used with this camera. |
