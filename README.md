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
# World.gd
extends Node3D

const BLOCK_SCENE = preload("res://Block.tscn") # This is your block (we’ll make it below)
const WORLD_WIDTH = 16
const WORLD_LENGTH = 16
const GROUND_HEIGHT = 4

func _ready():
    generate_flat_world()

func generate_flat_world():
    for x in WORLD_WIDTH:
        for z in WORLD_LENGTH:
            for y in GROUND_HEIGHT:
                var block = BLOCK_SCENE.instantiate()
                block.position = Vector3(x, y, z)
                add_child(block)
# World.gd
extends Node3D

const BLOCK_SCENE = preload("res://Block.tscn") # This is your block (we’ll make it below)
const WORLD_WIDTH = 16
const WORLD_LENGTH = 16
const GROUND_HEIGHT = 4

func _ready():
    generate_flat_world()

func generate_flat_world():
    for x in WORLD_WIDTH:
        for z in WORLD_LENGTH:
            for y in GROUND_HEIGHT:
                var block = BLOCK_SCENE.instantiate()
                block.position = Vector3(x, y, z)
                add_child(block)
# FlyMovement.gd
extends Node3D

@export var speed := 10.0

func _process(delta):
    var direction = Vector3.ZERO
    if Input.is_action_pressed("ui_up"):
        direction.z -= 1
    if Input.is_action_pressed("ui_down"):
        direction.z += 1
    if Input.is_action_pressed("ui_left"):
        direction.x -= 1
    if Input.is_action_pressed("ui_right"):
        direction.x += 1
    if Input.is_action_pressed("ui_page_up"):
        direction.y += 1
    if Input.is_action_pressed("ui_page_down"):
        direction.y -= 1

    translate(direction.normalized() * speed * delta)
# TouchControls.gd
extends Control

signal move_input(direction: Vector3)
signal action_input(action: String)

func _ready():
    $Button_MoveForward.pressed.connect(() => emit_signal("move_input", Vector3(0, 0, -1)))
    $Button_MoveBackward.pressed.connect(() => emit_signal("move_input", Vector3(0, 0, 1)))
    $Button_Left.pressed.connect(() => emit_signal("move_input", Vector3(-1, 0, 0)))
    $Button_Right.pressed.connect(() => emit_signal("move_input", Vector3(1, 0, 0)))
    $Button_Up.pressed.connect(() => emit_signal("move_input", Vector3(0, 1, 0)))
    $Button_Down.pressed.connect(() => emit_signal("move_input", Vector3(0, -1, 0)))
    $Button_Place.pressed.connect(() => emit_signal("action_input", "place"))
    $Button_Break.pressed.connect(() => emit_signal("action_input", "break"))
# Inside your Camera or Player script

func _ready():
    var controls = get_node("/root/YourMainScene/CanvasLayer/TouchControls")
    controls.connect("move_input", _on_move_input)
    controls.connect("action_input", _on_action_input)

var move_dir := Vector3.ZERO

func _on_move_input(dir):
    move_dir += dir

func _process(delta):
    translate(move_dir.normalized() * 5 * delta)
    move_dir = Vector3.ZERO # reset each frame

func _on_action_input(action):
    if action == "place":
        _place_block()
    elif action == "break":
        _break_block()
# BlockLibrary.gd (autoload this as a Singleton)
extends Node

var current_block_type := "dirt"

var block_scenes = {
    "dirt": preload("res://blocks/DirtBlock.tscn"),
    "grass": preload("res://blocks/GrassBlock.tscn"),
    "stone": preload("res://blocks/StoneBlock.tscn")
}

func get_current_block():
    return block_scenes[current_block_type]
# Hotbar.gd
extends HBoxContainer

func _ready():
    $Button_Dirt.pressed.connect(() => BlockLibrary.current_block_type = "dirt")
    $Button_Grass.pressed.connect(() => BlockLibrary.current_block_type = "grass")
    $Button_Stone.pressed.connect(() => BlockLibrary.current_block_type = "stone")
# Hotbar.gd
extends HBoxContainer

func _ready():
    $Button_Dirt.pressed.connect(() => BlockLibrary.current_block_type = "dirt")
    $Button_Grass.pressed.connect(() => BlockLibrary.current_block_type = "grass")
    $Button_Stone.pressed.connect(() => BlockLibrary.current_block_type = "stone")
# When placing a block
var new_block = BlockLibrary.get_current_block().instantiate()
new_block.position = (hit_pos + hit_normal).snapped(Vector3.ONE)
get_parent().add_child(new_block)
func _input(event):
    if event is InputEventKey:
        if event.pressed:
            if event.scancode == KEY_S:
                save_world()
            elif event.scancode == KEY_L:
                load_world()
res://
├── main.tscn              # Main scene
├── World.gd               # World script
├── Player.gd              # Player controls
├── BlockLibrary.gd        # Autoloaded block reference
├── UI/
│   ├── TouchControls.tscn
│   └── Hotbar.tscn
├── blocks/
│   ├── DirtBlock.tscn
│   ├── GrassBlock.tscn
│   └── StoneBlock.tscn
├── mobs/
│   └── Slime.tscn
└── assets/
    └── textures/
        ├── dirt.png
        ├── grass.png
        ├── stone.png
        └── slime.png
extends Node3D

const WORLD_WIDTH = 16
const WORLD_LENGTH = 16
const GROUND_HEIGHT = 4

func _ready():
    load_world()

func generate_world():
    for x in WORLD_WIDTH:
        for z in WORLD_LENGTH:
            for y in GROUND_HEIGHT:
                var block = BlockLibrary.block_scenes["dirt"].instantiate()
                block.position = Vector3(x, y, z)
                add_child(block)

func save_world():
    var file = FileAccess.open("user://world.save", FileAccess.WRITE)
    for block in get_children():
        if block.name.begins_with("Block"):
            file.store_line("%s;%s" % [block.scene_file_path, block.position])

func load_world():
    if not FileAccess.file_exists("user://world.save"):
        generate_world()
        return
    var file = FileAccess.open("user://world.save", FileAccess.READ)
    while not file.eof_reached():
        var line = file.get_line()
        var parts = line.split(";")
        var scene = load(parts[0])
        var pos = Vector3(parts[1])
        var block = scene.instantiate()
        block.position = pos
        add_child(block)
extends Node3D

@onready var raycast = $Camera3D/RayCast3D

func _unhandled_input(event):
    if event is InputEventScreenTouch and event.pressed:
        if raycast.is_colliding():
            var pos = raycast.get_collision_point()
            var normal = raycast.get_collision_normal()

            # BREAK
            var target = raycast.get_collider()
            if target.name.begins_with("Block"):
                target.queue_free()

            # PLACE
            var block = BlockLibrary.get_current_block().instantiate()
            block.position = (pos + normal).snapped(Vector3.ONE)
            get_parent().add_child(block)
@export var speed := 10.0
var move_dir = Vector3.ZERO

func _process(delta):
    if Input.is_action_pressed("ui_up"):
        move_dir.z -= 1
    if Input.is_action_pressed("ui_down"):
        move_dir.z += 1
    if Input.is_action_pressed("ui_left"):
        move_dir.x -= 1
    if Input.is_action_pressed("ui_right"):
        move_dir.x += 1
    if Input.is_action_pressed("ui_page_up"):
        move_dir.y += 1
    if Input.is_action_pressed("ui_page_down"):
        move_dir.y -= 1

    translate(move_dir.normalized() * speed * delta)
    move_dir = Vector3.ZERO
extends Node

var current_block_type := "dirt"

var block_scenes = {
    "dirt": preload("res://blocks/DirtBlock.tscn"),
    "grass": preload("res://blocks/GrassBlock.tscn"),
    "stone": preload("res://blocks/StoneBlock.tscn")
}

func get_current_block():
    return block_scenes[current_block_type]
extends HBoxContainer

func _ready():
    $Button_Dirt.pressed.connect(() => BlockLibrary.current_block_type = "dirt")
    $Button_Grass.pressed.connect(() => BlockLibrary.current_block_type = "grass")
    $Button_Stone.pressed.connect(() => BlockLibrary.current_block_type = "stone")
extends Control
signal move_input(Vector3)
signal action_input(String)

func _ready():
    $Forward.pressed.connect(() => emit_signal("move_input", Vector3(0, 0, -1)))
    $Back.pressed.connect(() => emit_signal("move_input", Vector3(0, 0, 1)))
    $Left.pressed.connect(() => emit_signal("move_input", Vector3(-1, 0, 0)))
    $Right.pressed.connect(() => emit_signal("move_input", Vector3(1, 0, 0)))
    $Up.pressed.connect(() => emit_signal("move_input", Vector3(0, 1, 0)))
    $Down.pressed.connect(() => emit_signal("move_input", Vector3(0, -1, 0)))
    $Place.pressed.connect(() => emit_signal("action_input", "place"))
    $Break.pressed.connect(() => emit_signal("action_input", "break"))
extends CharacterBody3D

@export var speed := 2.0
var direction = Vector3.ZERO

func _ready():
    direction = Vector3(randf() * 2 - 1, 0, randf() * 2 - 1).normalized()

func _physics_process(delta):
    move_and_slide(direction * speed)
var place_sound = preload("res://assets/sounds/place_block.ogg")
var break_sound = preload("res://assets/sounds/break_block.ogg")
var audio_player = AudioStreamPlayer3D.new()

func _ready():
    audio_player.stream = place_sound
    add_child(audio_player)
var bg_music = preload("res://assets/sounds/bg_music.ogg")

func _ready():
    var audio_player = AudioStreamPlayer.new()
    audio_player.stream = bg_music
    audio_player.loop = true
    add_child(audio_player)
    audio_player.play()
