# teleop_twist_keyboard
Generic Keyboard Teleoperation for ROS

## Run

```sh
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

Publishing to a different topic (in this case `my_cmd_vel`).
```sh
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap cmd_vel:=my_cmd_vel
```

## Docker (ROS 2 Jazzy)

Build the image from the repository root:
```sh
docker build -f docker/Dockerfile -t teleop_twist_keyboard:jazzy .
```

Run the node. `-it` is required because the node reads keypresses from the terminal,
and `--network host` lets it talk to ROS 2 nodes on the host or in other containers.
```sh
docker run --rm -it --network host teleop_twist_keyboard:jazzy
```

Pass extra arguments by overriding the command, e.g. to remap the topic:
```sh
docker run --rm -it --network host teleop_twist_keyboard:jazzy \
  ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap cmd_vel:=my_cmd_vel
```

Set `-e ROS_DOMAIN_ID=<id>` if your other nodes use a non-default domain.

## Usage

```
This node takes keypresses from the keyboard and publishes them as Twist
messages. It works best with a US keyboard layout.
---------------------------
Moving around:
   u    i    o
   j    k    l
   m    ,    .

For Holonomic mode (strafing), hold down the shift key:
---------------------------
   U    I    O
   J    K    L
   M    <    >

t : up (+z)
b : down (-z)

anything else : stop

q/z : increase/decrease max speeds by 10%
w/x : increase/decrease only linear speed by 10%
e/c : increase/decrease only angular speed by 10%

CTRL-C to quit
```

## Parameters
- `stamped (bool, default: false)`
  - If false (the default), publish a `geometry_msgs/msg/Twist` message.  If true, publish a `geometry_msgs/msg/TwistStamped` message.
- `frame_id (string, default: '')`
  - When `stamped` is true, the frame_id to use when publishing the `geometry_msgs/msg/TwistStamped` message.
- `speed (double, default: 0.5)`
  - The speed the node starts with by default.
- `turn (double, default: 1.0)`
  - The turn rate (rad/s) the node starts with by default.
- `publish_rate (double, default: 20.0)`
  - Rate (Hz) at which the latest command is republished, so the robot keeps moving while no key is pressed. Set to `0.0` to publish only on keypress.
