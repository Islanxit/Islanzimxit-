local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local Debris = game:GetService("Debris")

-- Evento remoto para ativar habilidades
local activateTransformationEvent = Instance.new("RemoteEvent")
activateTransformationEvent.Name = "ActivateTransformationEvent"
activateTransformationEvent.Parent = ReplicatedStorage

local clickAttackEvent = Instance.new("RemoteEvent")
clickAttackEvent.Name = "ClickAttackEvent"
clickAttackEvent.Parent = ReplicatedStorage

-- Transformar jogador na forma espiritual
local function transformToSpiritForm(player)
    if not player.Character or not player.Character:FindFirstChild("Humanoid") then return end

    local character = player.Character
    local humanoid = character:FindFirstChild("Humanoid")
    local head = character:FindFirstChild("Head")

    -- Modificar aparência
    for _, part in ipairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Material = Enum.Material.Neon
            part.Color = Color3.fromRGB(0, 255, 255)
            part.Transparency = 0.2
        end
    end

    -- Criar partículas no personagem
    local particleEmitter = Instance.new("ParticleEmitter")
    particleEmitter.Texture = "rbxassetid://6873659530"
    particleEmitter.Rate = 50
    particleEmitter.Lifetime = NumberRange.new(1, 2)
    particleEmitter.Speed = NumberRange.new(2, 5)
    particleEmitter.Size = NumberSequence.new({ NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 3) })
    particleEmitter.Parent = head

    -- Supervelocidade
    humanoid.WalkSpeed = 50

    -- Remover partículas após 30 segundos e reverter transformação
    Debris:AddItem(particleEmitter, 30)
    task.delay(30, function()
        humanoid.WalkSpeed = 16
        for _, part in ipairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.Material = Enum.Material.Plastic
                part.Color = Color3.fromRGB(255, 255, 255)
                part.Transparency = 0
            end
        end
    end)
end

-- Atacar outros jogadores com clique
local function clickAttack(player, target)
    if target and target:IsA("Player") and target.Character and target.Character:FindFirstChild("Humanoid") then
        local humanoid = target.Character:FindFirstChild("Humanoid")
        humanoid.Health = 0
