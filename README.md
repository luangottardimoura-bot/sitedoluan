class_name InimigoBase
extends CharacterBody3D

@export_group("Atributos")
@export var vida_maxima: int = 30
@export var velocidade: float = 3.0
@export var dano_ataque: int = 10
@export var intervalo_ataque: float = 1.0

var vida_atual: int
var jogador: Node3D = null
var pode_atacar: bool = true

@onready var area_ataque: Area3D = $AreaAtaque

func _ready():
	vida_atual = vida_maxima
	jogador = get_tree().get_first_node_in_group("Jogador")
	
	# Conecta os sinais de ataque da área
	if area_ataque:
		area_ataque.body_entered.connect(_on_area_ataque_body_entered)

func _physics_process(delta: float):
	if jogador:
		# Persegue o jogador
		var posicao_alvo = jogador.global_position
		posicao_alvo.y = global_position.y
		look_at(posicao_alvo, Vector3.UP)
		
		var direcao = (posicao_alvo - global_position).normalized()
		velocity.x = direcao.x * velocidade
		velocity.z = direcao.z * velocidade
		move_and_slide()

func receber_dano(quantidade: int):
	vida_atual -= quantidade
	if vida_atual <= 0:
		morrer()

func morrer():
	queue_free()

func _on_area_ataque_body_entered(body):
	# Se o corpo que entrou na área for o jogador e o inimigo poder atacar
	if body.is_in_group("Jogador") and pode_atacar:
		atacar(body)

func atacar(alvo):
	pode_atacar = false
	if alvo.has_method("receber_dano"):
		alvo.receber_dano(dano_ataque)
	
	# Aguarda o tempo de recarga para poder atacar novamente
	await get_tree().create_timer(intervalo_ataque).timeout
	pode_atacar = true