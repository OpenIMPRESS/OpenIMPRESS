# OpenIMPRESS

OpenIMPRESS is a toolkit for mixed reality telepresence applications.
In this repository, we link include the core components described in 
our paper published at the [24th symposium on Virtual Reality Software and Technology 2018](https://vrst.acm.org/vrst2018/).

# Getting Started
Clone this repository and initialize the submodules.

```git clone https://github.com/OpenIMPRESS/OpenIMPRESS.git```  

```cd OpenIMPRESS```  

```git submodule update --init --recursive``` 

Our system comes with three major components.

1. The On-Site Operator (OSO) client for AR HMDs such as the Hololens.
2. The Kinect Streamer for fast streaming of 3D point clouds.
3. The Remote Operator (RO) client for VR HMDs (HTC Vive).

In the default configuration, the OSO is at a capture location with one or more Kinects.
The Kinect Streamers are used to stream the Kinect data to the RO.
Using the marker-based alignment procedure, the OSO client estimates and shares the Kinect camera extrinsics
with the RO, so that the point clouds can be rendered consistently relative to each other.
What is more, the OSO streams the low-poly spatial mesh to the RO for a low-detail but wide area representation 
of the capture location.

## OpenIMPRESS Features

While this repository comes with these components `preconfigured`, they can be used individually.
See the respective repositories on the OpenIMPRESS Github page:
- Share Hololens and Vive poses (Head, hands) and other transforms (oi.plugin.transform)
- Share Hololens Spatial Mesh (oi.plugin.spatialmesh)
- Use AR-Markers to align RGBD cameras (oi.plugin.kinectMarkerAlignment)
- Line-based annotation system (oi.plugin.linedrawing)
- Fast RGBD data streaming (see clients in oi.native) and rendering (oi.plugin.rgbd)
- HMD to HMD audio streaming (supporting both Desktop and Hololens, oi.plugin.audio).

## Setup the On-site Operator tool

Requirements: Unity 2017.4 (LTS), Visual Studio with all dependencies for UWP/Hololens development.

- Open the folder `OpenIMPRESS/clients/oi.client.hololens` with Unity.
- Next, import the [MixedRealityToolkit Unity package v2017.4.2.0](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases/download/2017.4.2.0/HoloToolkit-Unity-2017.4.2.0.unitypackage).
- Open the scene `Scenes/OSO`. Look at the GameObjects with the UDPConnector component in the hierarchy (grouped under the OpenIMPRESS object) and configure the IP's of the RO client. 
- Build the project and deploy it to the Hololens.
- For the marker-based alignment procedure, the Kinects at the capture location need to have the AR markers attached to them.
- See the `OpenIMPRESS/resources/Markers` folder for more details.

Once setup and running, simply look at the marker attached to the kinect you want to calibrate, and perform the hololens 'tap' gesture with your hand.

## Setup the Kinect Streamer

Requirements: C/C++ development environment. CMake 3.2+. Libfreenect2 and it's dependencies installed (libjpeg-turbo, glew, etc.).

- Go to `OpenIMPRESS/oi.native` and create a `build` folder to run cmake from.
- I.e., for Visual Studio 2017, run `cmake -G "Visual Studio 15 2017 Win64" ..`.
- The the kinect streamer target is `oi.client.rgbd.libfreenect2`. 
- To start the streamer and stream to the RO client, run `oi.client.rgbd.libfreenect2 -ep [ROIP]:5001 -lp 5005`.

## Setup the Remote Operator tool

Requirements: Unity 2017.4 (LTS). SteamVR/HTC Vive setup.

- Go to `OpenIMPRESS/clients/oi.client.steamvr`. 
- Open the scene `Scenes/RO`. Look at the GameObjects with the UDPConnector component in the hierarchy (grouped under the OpenIMPRESS object) and configure the IP's of the OSO client and Kinect Stream. 
- Run the project.
- The 'grip' buttons on the Vive controller allow the RO to shift through space without walking.
- The 'trigger' buttons are used for the line-drawing/annotation tool.

# TODO's

- Leap Motion streaming
- Other RGBD clients (there are some legacy implementations that need to be updated, see the OpenIMPRESS oi.client.rgbd.* repositories on Github)
- Matchmaking server.
