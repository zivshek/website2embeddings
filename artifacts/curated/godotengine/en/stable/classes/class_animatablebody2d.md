# AnimatableBody2D

Inherits: StaticBody2D < PhysicsBody2D < CollisionObject2D < Node2D <
CanvasItem < Node < Object

A 2D physics body that can't be moved by external forces. When moved manually,
it affects other bodies in its path.

## Description

An animatable 2D physics body. It can't be moved by external forces or
contacts, but can be moved manually by other means such as code,
AnimationMixers (with AnimationMixer.callback_mode_process set to
AnimationMixer.ANIMATION_CALLBACK_MODE_PROCESS_PHYSICS), and
RemoteTransform2D.

When AnimatableBody2D is moved, its linear and angular velocity are estimated
and used to affect other physics bodies in its path. This makes it useful for
moving platforms, doors, and other moving objects.

## Properties

bool | sync_to_physics | `true`  
---|---|---  
  
## Property Descriptions

bool sync_to_physics = `true`

  * void set_sync_to_physics(value: bool)

  * bool is_sync_to_physics_enabled()

If `true`, the body's movement will be synchronized to the physics frame. This
is useful when animating movement via AnimationPlayer, for example on moving
platforms. Do not use together with PhysicsBody2D.move_and_collide().

## User-contributed notes

Please read the User-contributed notes policy before submitting a comment.

* * *

Built with Sphinx using a theme provided by Read the Docs.

  *[void]: No return value.

