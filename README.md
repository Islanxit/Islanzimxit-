local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

-- Evento remoto para ativar a transformação
local activateTransformationEvent = Instance.new("RemoteEvent")
activateTransformationEvent.Name = "ActivateTransformationEvent"
activateTransformationEvent.Parent = ReplicatedStorage

-- Função de transformação
local function transformToOkarun(player)
    if not player.Character or not player.Character:FindFirstChild("Humanoid") then return end

    local character = player.Character
    local humanoid = character:FindFirstChild("Humanoid")
    local head = character:FindFirstChild("Head")

    -- Ajustar aparência do personagem
    for _, part in ipairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Material = Enum.Material.Neon
            part.Color = Color3.fromRGB(0, 255, 255) -- Cor azul ciano brilhante
            part.Transparency = 0.2
        end
    end

    -- Criar partículas no personagem
    local particleEmitter = Instance.new("ParticleEmitter")
    particleEmitter.Texture = "rbxassetid://6873659530" -- Textura abstrata
    particleEmitter.Rate = 100
    particleEmitter.Lifetime = NumberRange.new(2, 4)
    particleEmitter.Speed = NumberRange.new(3, 6)
    particleEmitter.Size = NumberSequence.new({ NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 3) })
    particleEmitter.Parent = head

    -- Aumentar a velocidade e saltos do jogador (efeito espiritual)
    humanoid.WalkSpeed = 50
    humanoid.JumpPower = 100

    -- Reverter a t
