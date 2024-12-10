# Islanzimxit-
-- Variáveis
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Workspace = game.Workspace

-- Cria a esfera de domínio
local function createDomainSphere(position, size, parent)
    local sphere = Instance.new("Part")
    sphere.Shape = Enum.PartType.Ball
    sphere.Size = size
    sphere.Position = position
    sphere.Anchored = true
    sphere.CanCollide = false
    sphere.Material = Enum.Material.Neon
    sphere.Color = Color3.fromRGB(75, 0, 130) -- Cor base
    sphere.Parent = parent

    -- Adiciona partículas de galáxia
    local particleEmitter = Instance.new("ParticleEmitter")
    particleEmitter.Texture = "rbxassetid://6873659530" -- Textura de galáxia
    particleEmitter.Rate = 50
    particleEmitter.Lifetime = NumberRange.new(2, 4)
    particleEmitter.Speed = NumberRange.new(5, 10)
    particleEmitter.Size = NumberSequence.new({ NumberSequenceKeypoint.new(0, 5), NumberSequenceKeypoint.new(1, 0) })
    particleEmitter.Parent = sphere

    return sphere
end

-- Expansão de domínio
local function domainExpansion()
    -- Tela branca em todos os jogadores
    for _, player in ipairs(Players:GetPlayers()) do
        if player:FindFirstChild("PlayerGui") then
            local screen = Instance.new("ScreenGui", player.PlayerGui)
            screen.IgnoreGuiInset = true
            screen.ResetOnSpawn = false

            local frame = Instance.new("Frame", screen)
            frame.Size = UDim2.new(1, 0, 1, 0)
            frame.BackgroundColor3 = Color3.new(1, 1, 1) -- Branco
            frame.BackgroundTransparency = 1

            -- Efeito de flash
            local tween = TweenService:Create(
                frame,
                TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.Out),
                { BackgroundTransparency = 0 }
            )
            tween:Play()

            -- Volta ao normal
            tween.Completed:Connect(function()
                local fadeOut = TweenService:Create(
                    frame,
                    TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.Out),
                    { BackgroundTransparency = 1 }
                )
                fadeOut:Play()
                fadeOut.Completed:Connect(function()
                    screen:Destroy()
                end)
            end)
        end
    end

    -- Criação da esfera de domínio
    local domainCenter = Workspace:FindFirstChild("DomainCenter") or Instance.new("Part", Workspace)
    domainCenter.Name = "DomainCenter"
    domainCenter.Anchored = true
    domainCenter.CanCollide = false
    domainCenter.Transparency = 1
    domainCenter.Size = Vector3.new(1, 1, 1)
    domainCenter.Position = Vector3.new(0, 50, 0) -- Posição da esfera

    local sphere = createDomainSphere(domainCenter.Position, Vector3.new(50, 50, 50), Workspace)

    -- Puxar jogadores para o centro
    for _, player in ipairs(Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local rootPart = player.Character.HumanoidRootPart
            rootPart.Anchored = false
            local bodyPosition = Instance.new("BodyPosition")
            bodyPosition.MaxForce = Vector3.new(1e6, 1e6, 1e6)
            bodyPosition.Position = domainCenter.Position
            bodyPosition.P = 1e5
            bodyPosition.Parent = rootPart

            -- Remove o BodyPosition após 3 segundos
            game:GetService("Debris"):AddItem(bodyPosition, 3)
        end
    end

    -- Expansão da esfera
    local sphereTween = TweenService:Create(
        sphere,
        TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.Out),
        { Size = Vector3.new(200, 200, 200) }
    )
    sphereTween:Play()

    sphereTween.Completed:Connect(function()
        sphere:Destroy()
    end)
end

-- Ativa a expansão de domínio
domainExpansion()
