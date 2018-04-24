# OpenIMPRESS - Complete project
This includes, as submodules, all OpenIMPRESS components.

# Setup
Clone this repository and initialize the submodules.

```git clone https://github.com/OpenIMPRESS/OpenIMPRESS.git```
```cd OpenIMPRESS```
```git submodule update --init --recursive```

Then, you can compile/run the components you need.
A typical basic setup contains the RGBD streaming and a Unity
client to receive the stream. For this, compile one of the
rgbd streamer clients in ```clients/oi.client.rgbd.*```,
and open the unity default client in ```clients/oi.client.desktop```
with Unity3D. See the respective README's in these folders for
further instructions.
