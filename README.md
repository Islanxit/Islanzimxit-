-- Configuração inicial
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Criar o ponto de expansão do domínio (Part)
local domainPart = Instance.new("Part")
domainPart.Size = Vector3.new(10, 1, 10)
domainPart.Position = character.HumanoidRootPart.Position
domainPart.Anchored = true
domainPart.CanCollide = false
domainPart.BrickColor = BrickColor.new("Bright blue")
domainPart.Parent = workspace
domainPart.Transparency = 1  -- Inicialmente invisível

-- Criar um efeito visual para simular a expansão do domínio
local domainEffect = Instance.new("SpecialMesh")
domainEffect.MeshType = Enum.MeshType.Sphere
domainEffect.Scale = Vector3.new(1, 1, 1)
domainEffect.Parent = domainPart

-- Função para expandir o domínio e criar efeitos
local function expandDomain()
    -- Expansão do domínio
    for i = 1, 20 do
        domainPart.Size = domainPart.Size + Vector3.new(2, 0, 2)  -- Aumenta a área
        domainEffect.Scale = domainEffect.Scale + Vector3.new(0.1, 0.1, 0.1)  -- Expande o efeito visual
        domainPart.CFrame = domainPart.CFrame * CFrame.new(0, 0, 0.1)  -- Ajusta posição lentamente
        wait(0.1)  -- Tempo entre expansões
    end
    
    -- Detectar jogadores dentro da área da expansão e causar dano/matar
    local playersInRange = {}  -- Lista de jogadores dentro da área
    for _, obj in pairs(workspace:GetChildren()) do
        if obj:IsA("Model") and obj:FindFirstChild("Humanoid") then
            local humanoid = obj.Humanoid
            local distance = (domainPart.Position - obj.PrimaryPart.Position).Magnitude
            if distance <= domainPart.Size.X / 2 then  -- Verifica se está dentro da área da expansão
                table.insert(playersInRange, obj)
            end
        end
    end

    -- Matar ou causar dano aos jogadores dentro da expansão
    for _, playerModel in pairs(playersInRange) do
        local playerHumanoid = playerModel:FindFirstChild("Humanoid")
        if playerHumanoid then
            -- Aqui a "morte" do jogador
            playerHumanoid.Health = 0  -- Isso mata o jogador
            -- Ou você pode dar dano, por exemplo:
            -- playerHumanoid:TakeDamage(playerHumanoid.Health)  -- Dano total
        end
    end
end

-- Criar a interface de "Sim ou Não"
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0.5, -150, 0.5, -100)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = screenGui

local questionLabel = Instance.new("TextLabel")
questionLabel.Size = UDim2.new(1, 0, 0.4, 0)
questionLabel.Position = UDim2.new(0, 0, 0, 0)
questionLabel.Text = "Você quer ativar a Expansão de Domínio?"
questionLabel.TextSize = 20
questionLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
questionLabel.BackgroundTransparency = 1
questionLabel.Parent = frame

local yesButton = Instance.new("TextButton")
yesButton.Size = UDim2.new(0.4, 0, 0.3, 0)
yesButton.Position = UDim2.new(0.1, 0, 0.6, 0)
yesButton.Text = "Sim"
yesButton.TextSize = 20
yesButton.TextColor3 = Color3.fromRGB(255, 255, 255)
yesButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
yesButton.Parent = frame

local noButton = Instance.new("TextButton")
noButton.Size = UDim2.new(0.4, 0, 0.3, 0)
noButton.Position = UDim2.new(0.5, 0, 0.6, 0)
noButton.Text = "Não"
noButton.TextSize = 20
noButton.TextColor3 = Color3.fromRGB(255, 255, 255)
noButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
noButton.Parent = frame

-- Função para esconder a interface e ativar a expansão
local function onYesClick()
    -- Esconder UI
    screenGui:Destroy()

    -- Ativar a expansão de domínio
    expandDomain()
end

local function onNoClick()
    -- Esconder UI
    screenGui:Destroy()

    -- Mensagem de "não ativar"
    print("Você escolheu não ativar a Expansão de Domínio.")
end

-- Conectar os botões aos eventos
yesButton.MouseButton1Click:Connect(onYesClick)
noButton.MouseButton1Click:Connect(onNoClick)
