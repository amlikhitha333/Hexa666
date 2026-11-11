# HEXA

**Control a robot arm with the phone in your pocket.**

HEXA lets you teleoperate an SO-101 robot arm just by moving your phone. No second "leader" arm needed. While you drive, every motion can be saved as a LeRobot dataset, ready for training robot policies.

![HEXA simulator demo](docs/media/hexa-sim-demo.gif)

## What is this?

Teaching robots by demonstration needs a lot of human examples. Normally you collect them with a leader arm, which means extra hardware and extra cost. HEXA swaps that for something you already own: a phone with a built-in motion tracker.

- Works with **Android (ARCore)** and **iPhone (ARKit)**
- A **simulator and browser viewer** so you can practise before touching real hardware
- Records **LeRobot-format data**: state, action, camera video and task label
- Built-in **speed and joint limits** for safer control

## How it works

1. The phone tracks its own position and rotation.
2. It sends that pose to a Python server over your Wi-Fi.
3. The server turns the motion into SO-101 joint angles (inverse kinematics) and applies safety limits.
4. The arm (or the simulator) moves, the browser viewer shows it live, and the recorder logs the episode if enabled.

## Screenshots

![Connect screen](docs/media/hexa-app-connect.jpg)
![Control pad](docs/media/hexa-app-controls.jpg)
![Browser viewer](docs/media/hexa-sim-viewer.jpg)

## What is inside

The project is packaged in `HEXA-Team.zip`. Unzip it to get the `EmberArm-Team` folder:

| Item | Purpose |
| --- | --- |
| `EmberArm-Final.apk` | Android app, ready to install |
| `PhoneTeleop.ipa` | iOS app, shows up as HEXA (sideload it) |
| `server/` | Python server: kinematics, safety, robot control, recorder, viewer |
| `app-source/` | Phone app source (React Native with native ARCore and ARKit modules) |
| `FRIEND-SETUP.md` | One-page Windows quick start |

## Try it in the simulator

You need: Python 3.10+, a phone with ARCore or ARKit, and the phone and computer on the same Wi-Fi.

```bash
unzip HEXA-Team.zip
cd EmberArm-Team/server
chmod +x emberarm.sh
./emberarm.sh setup --no-lerobot
./emberarm.sh start --sim
```

On Windows you can use `emberarm.bat` instead of `emberarm.sh`.

The server prints an address like `ws://192.168.x.x:4443/ws`. Then:

1. Install the app on your phone (APK for Android, sideload the IPA for iPhone).
2. Open the app, enter the address and tap **Connect**.
3. Tap **Start tracking** and wave the phone slowly over a bright, textured surface until it says **Ready**.
4. Tap **Calibrate**, then hold the pad and move. The arm follows.
5. Open http://localhost:8080 to watch the simulated arm.

**Pad controls:** hold to move, left half for slow and precise, right half for normal speed, slide up or down for the gripper, release to stop.

## Recording demonstrations

```bash
./emberarm.sh record --sim --max-speed 150
```

Add `--task "pick up the cube"` to label episodes. Data is saved to `server/datasets/emberarm_demo`. If LeRobot is not installed, episodes fall back to plain NPZ files.

## Using the real arm

```bash
./emberarm.sh setup        # installs LeRobot
./emberarm.sh calibrate    # move each joint through its full range once
./emberarm.sh start        # dry run, nothing moves
./emberarm.sh start --live # drives the real arm
```

Useful flags: `--arm-port`, `--id`, `--roll-sign -1`, `--invert-gripper`, `--max-speed N`.

## Safety

The real arm really moves. Keep a hand near the power supply and go slowly. The arm only moves while you hold the pad and tracking is Ready, and it stops commands if Wi-Fi drops for 3 seconds. This is a lab tool, not a certified safety system.

## Tested on

| Device | Result |
| --- | --- |
| iPhone 13 (ARKit) | Works |
| Samsung Galaxy A35 (ARCore) | Works |
| Real SO-101 arm | Works |
| Simulator and browser viewer | Works |

## Team

Built by **Deepak**, **A M Likhitha** and **Chiranthan P** as part of our robotics research work.

## Credits

Thanks to the SO-101 / SO-ARM100 project, Hugging Face LeRobot, onshape-to-robot, Google ARCore, Apple ARKit, React Native, three.js, websockets, NumPy and pySerial.

## License

HEXA's code is released under the MIT License (see `LICENSE`). The SO-101 design files and third-party libraries keep their own licenses.