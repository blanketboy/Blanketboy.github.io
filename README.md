# Player.gd
extends CharacterBody3D

@export var speed := 5.0
@export var jump_velocity := 6.0
@export var gravity := 9.8

func _physics_process(delta):
    var direction = Vector3.ZERO
    if Input.is_action_pressed("ui_right"):
        direction.x += 1
    if Input.is_action_pressed("ui_left"):
        direction.x -= 1
    if Input.is_action_pressed("ui_up"):
        direction.z -= 1
    if Input.is_action_pressed("ui_down"):
        direction.z += 1

    direction = direction.normalized()
    velocity.x = direction.x * speed
    velocity.z = direction.z * speed

    if not is_on_floor():
        velocity.y -= gravity * delta
    else:
        if Input.is_action_just_pressed("ui_accept"):
            velocity.y = jump_velocity

    move_and_slide()