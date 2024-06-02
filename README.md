teleop_twist_joy [![Build Status](https://travis-ci.org/ros-teleop/teleop_twist_joy.svg?branch=indigo-devel)](https://travis-ci.org/ros-teleop/teleop_twist_joy)
================

Simple joystick teleop for twist robots. See [ROS Wiki](http://wiki.ros.org/teleop_twist_joy)

# GFOE Extensions

* Option to publish `TwistStamped` vice `Twist`.  Activated via `joy_vel_output` parameters (see below).  The `TwistStamped` option is necessary to interface with Project11, e.g., [helm_manager](https://github.com/GFOE/helm_manager).
* 


## teleop_node

### Subscribed Topics



### Published Topics

* `cmd_val`(geometry_msgs/Twist OR geometry_msgs/TwistStamped)


* `bluearrow_mode_cmd` ([gfoe_j1939_blue_arrow_interface::BlueArrowHelmMode](https://bitbucket.org/gfoe/gfoe_j1939_blue_arrow_interface/src/gfoe-devel/msg/BlueArrowHelmMode.msg)):

* `xci_control_enable` (std_msgs::Bool)

* `send_command` (std_msgs::String): Publishes Project 11 piloting mode commands.  In conjuction with parameters `manual_button` and `auto_button` can send "piloting_mode_manual" and "piloting_mode_autonomous" Strings which typically got to the `command_bridge_sender`->`command_bridge_receiver`->`mission_control` nodes.  Currently looks this functionality is commented out, so that only a `piloting_mode_normal` String is published.


### Parameters

* `joy_vel_output` (string, default: "TwistStamped"): If the value is "Twist", publish a `Twist` message as is done in the unmodified teleop_node.  If the value is anything else, publish a `TwistStamped` message.


* `xci_control_button` (int, default: -1)

These parameters specify the buttons used to publish on the `bluearrow_mode_cmd` topic.

* `open_loop_button` (int, default: -1)

* `station_keeep_button` (int, default: -1)

* `virtual_anchor_button` (int, default: -1): When the specified button is pressed 

The following parameters are still in the code, but the functionality is commented out:

* `manual_button` (int, default: -1): When the specified button is pressed a `piloting_mode_manual` String is published on the `send_command` topic.  See above.

* `auto_button` (int, default: -1): When the specified button is pressed a `piloting_mode_autonomous` String is published on the `send_command` topic.  See above.


### Examples

#### [launch/fy21_step2_annie_common.launch](https://bitbucket.org/gfoe/project12/src/gfoe-devel/launch/fy21_step2_annie_common.launch)

```
  <!-- our custom joystick adapter node -->
  <node pkg="teleop_twist_joy" name="teleop_twist_joy" type="teleop_node"
	ns="$(arg namespace)"
	output="screen">
    <remap from="cmd_vel" to="piloting_mode/manual/cmd_vel"/>
    <remap from="xci_control_enable" to="transfer_xci_cmd"/>
    <remap from="send_command" to="project11/send_command"/>
    <rosparam file="$(find project12)/config/teleop_twist_joy.yaml"
	      command="load"/>
  </node>
```

[config/teleop_twist_joy.yaml](https://bitbucket.org/gfoe/project12/src/gfoe-devel/config/teleop_twist_joy.yaml)
```
# Joystick axis scaling for Annie
axis_linear:
  x: 1
  y: 3
scale_linear:
  x: 2.0
  y: -0.56
scale_linear_turbo:
  x: 2.0
  y: -0.56
axis_angular:
  yaw: 0
scale_angular:
  yaw: 0.211

# Enable output
enable_button: 4  # LB 
#enable_turbo_button:
# For switching blue arrow modes
station_keep_button: 0 # A
virtual_anchor_button: 1 # B
open_loop_button: 3 # Y
# Going into auto/manul
auto_button: 5 # RB
manual_button: 10 # Right thumb stick
# Transfer to XCI 
xci_control_button: 2 # X
```
