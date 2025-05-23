# AnimationNodeExtension

Experimental: This class may be changed or removed in future versions.

Inherits: AnimationNode < Resource < RefCounted < Object

Base class for extending AnimationRootNodes from GDScript, C#, or C++.

## Description

AnimationNodeExtension exposes the APIs of AnimationRootNode to allow users to
extend it from GDScript, C#, or C++. This class is not meant to be used
directly, but to be extended by other classes. It is used to create custom
nodes for the AnimationTree system.

## Methods

PackedFloat32Array | _process_animation_node(playback_info: PackedFloat64Array, test_only: bool) virtual  
---|---  
float | get_remaining_time(node_info: PackedFloat32Array, break_loop: bool) static  
bool | is_looping(node_info: PackedFloat32Array) static  
  
## Method Descriptions

PackedFloat32Array _process_animation_node(playback_info: PackedFloat64Array,
test_only: bool) virtual

A version of the AnimationNode._process() method that is meant to be
overridden by custom nodes. It returns a PackedFloat32Array with the processed
animation data.

The PackedFloat64Array parameter contains the playback information, containing
the following values encoded as floating point numbers (in order): playback
time and delta, start and end times, whether a seek was requested (encoded as
a float greater than `0`), whether the seek request was externally requested
(encoded as a float greater than `0`), the current LoopedFlag (encoded as a
float), and the current blend weight.

The function must return a PackedFloat32Array of the node's time info,
containing the following values (in order): animation length, time position,
delta, LoopMode (encoded as a float), whether the animation is about to end
(encoded as a float greater than `0`) and whether the animation is infinite
(encoded as a float greater than `0`). All values must be included in the
returned array.

float get_remaining_time(node_info: PackedFloat32Array, break_loop: bool)
static

Returns the animation's remaining time for the given node info. For looping
animations, it will only return the remaining time if `break_loop` is `true`,
a large integer value will be returned otherwise.

bool is_looping(node_info: PackedFloat32Array) static

Returns `true` if the animation for the given `node_info` is looping.

## User-contributed notes

Please read the User-contributed notes policy before submitting a comment.

* * *

Built with Sphinx using a theme provided by Read the Docs.

  *[virtual]: This method should typically be overridden by the user to have any effect.
  *[static]: This method doesn't need an instance to be called, so it can be called directly using the class name.

