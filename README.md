# cabstream
Embedded multi-user remote streaming system

cabstream is designed to provide a hands-off streaming solution for people who share a single device for different sessions, for example, arcade machines.

Repository structure:
scripts/ - example txproto scripts for a few arcade machines
hardware/ - description and setup info of hardware used
api/ - API server and example client
os/ - custom OS deployment images or scripts

### Rationale

Arcades in Japan will sometimes provide streaming setups for players to use with a computer, capture card, monitor, webcam and default OBS config.

Unfortunately, this is not viable over here in the United States, as if you put a free to use computer next to an arcade cabinet for streaming purposes, it will be gone by closing time. If not, it will either be used for illicit purposes or damaged. You're lucky to even get video out of machines.

Thus, the need for a setup that is invisible to the general public, but usable by frequent players.

This project will:
- stream to a server of choice (default will be Twitch) using user-provided credentials
- support capture from any UVC capable capture card for any given input, taking into account rotation, frame rate, etc.
- optionally allow audio from an external microphone, in addition to the sound provided via said UVC capture card (or other sound input when the video input device does not provide audio)
- support "overlays", customizable objects on screen that can be placed in different locations according to default or user configuration. HTML webview is important for common streaming setups to services such as Twitch, for a view of the chat.
- provide external API for users to: start and stop stream, provide streaming server URL and authentication, switch scenes
- be relatively portable regarding hardware

### Hardware

Targeting embedded hardware that runs a Unix operating system with either USB or PCIE capture devices. Windows is out of scope, but theoretically possible.

Example: Raspberry Pi, Khadas VIM4, Lenovo ThinkCentre.

### Software

The software handling all multimedia functions is [txproto](https://github.com/cyanreg/txproto/). txproto abstracts a lot of multimedia functions into a scriptable language, and was chosen for the portability that offers.

Targeted operating systems are NetBSD and Alpine Linux. ffmpeg (and thus, txproto) handles video inputs from various operating systems, but NetBSD was chosen as a primary target due to its simplicity offering stability in a potential long-form embedded environment and wide range of hardware support. It includes a v4l2-compatile interface, video(4). For finicky hardware that is not supported easily on NetBSD, deployments are tested on Alpine Linux as well.

### API

The API, for ease of use into existing local communities, will contain a standalone server that accepts authenticated commands for stream operation.

There will be an example TypeScript/Python client that provides a Discord bot interface, which will be used in my deployments, as our local community has an already established Discord presence.
