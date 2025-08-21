-- Baconzinho Hub final com voo infinito (PC + Mobile)
local player = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BaconzinhoHub"
ScreenGui.Parent = player:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(0, 102, 204)
Frame.Size = UDim2.new(0, 220, 0, 320)
Frame.Position = UDim2.new(0.5, -110, 0.5, -160)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Visible = false

local Title = Instance.new("TextLabel")
Title.Parent = Frame
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "🌀 Baconzinho Hub 🌀"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20

-- Caixa de texto velocidade
local SpeedBox = Instance.new("TextBox")
SpeedBox.Parent = Frame
SpeedBox.Size = UDim2.new(1, -20, 0, 40)
SpeedBox.Position = UDim2.new(0, 10, 0, 50)
SpeedBox.PlaceholderText = "Digite velocidade (andar/voar)"
SpeedBox.ClearTextOnFocus = false
SpeedBox.BackgroundColor3 = Color3.fromRGB(200, 200, 255)
SpeedBox.TextColor3 = Color3.fromRGB(0, 0, 0)
SpeedBox.Font = Enum.Font.SourceSans
SpeedBox.TextSize = 18
SpeedBox.BorderSizePixel = 0

-- Botão aplicar velocidade
local SpeedButton = Instance.new("TextButton")
SpeedButton.Parent = Frame
SpeedButton.Size = UDim2.new(1, -20, 0, 40)
SpeedButton.Position = UDim2.new(0, 10, 0, 100)
SpeedButton.Text = "Aplicar Velocidade"
SpeedButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
SpeedButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedButton.Font = Enum.Font.SourceSansBold
SpeedButton.TextSize = 18
SpeedButton.BorderSizePixel = 0

-- Botão resetar velocidade
local ResetButton = Instance.new("TextButton")
ResetButton.Parent = Frame
ResetButton.Size = UDim2.new(1, -20, 0, 40)
ResetButton.Position = UDim2.new(0, 10, 0, 150)
ResetButton.Text = "Resetar Velocidade"
ResetButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
ResetButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ResetButton.Font = Enum.Font.SourceSansBold
ResetButton.TextSize = 18
ResetButton.BorderSizePixel = 0

-- Botão voar
local FlyButton = Instance.new("TextButton")
FlyButton.Parent = Frame
FlyButton.Size = UDim2.new(1, -20, 0, 40)
FlyButton.Position = UDim2.new(0, 10, 0, 200)
FlyButton.Text = "pular alto"
FlyButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
FlyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
FlyButton.Font = Enum.Font.SourceSansBold
FlyButton.TextSize = 18
FlyButton.BorderSizePixel = 0

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Parent = Frame
CloseButton.Size = UDim2.new(1, -20, 0, 40)
CloseButton.Position = UDim2.new(0, 10, 0, 250)
CloseButton.Text = "❌ Fechar"
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 18
CloseButton.BorderSizePixel = 0

-- Botão flutuante
local ToggleButton = Instance.new("TextButton")
ToggleButton.Parent = ScreenGui
ToggleButton.Size = UDim2.new(0, 140, 0, 40)
ToggleButton.Position = UDim2.new(0.05, 0, 0.2, 0)
ToggleButton.Text = "📂 Baconzinho Hub"
ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 18
ToggleButton.BorderSizePixel = 0
ToggleButton.Active = true
ToggleButton.Draggable = true

-- Variáveis do personagem
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local hrp = character:WaitForChild("HumanoidRootPart")
local flying = false
local flySpeed = 50
local jumping = false

-- Atualizar personagem no respawn
player.CharacterAdded:Connect(function(char)
    character = char
    humanoid = character:WaitForChild("Humanoid")
    hrp = character:WaitForChild("HumanoidRootPart")
end)

-- Aplicar velocidade andar/voar
SpeedButton.MouseButton1Click:Connect(function()
    local speed = tonumber(SpeedBox.Text)
    if speed and speed > 0 then
        humanoid.WalkSpeed = speed
        flySpeed = speed
    else
        warn("Digite um número válido")
    end
end)

ResetButton.MouseButton1Click:Connect(function()
    humanoid.WalkSpeed = 16
    flySpeed = 50
end)

-- Abrir/fechar menu
ToggleButton.MouseButton1Click:Connect(function() Frame.Visible = not Frame.Visible end)
CloseButton.MouseButton1Click:Connect(function() Frame.Visible = false end)

-- Ativar/desativar voo
FlyButton.MouseButton1Click:Connect(function()
    flying = not flying
    FlyButton.Text = flying and "Parar de pular" or "pular alto"
end)
-- Detectar tecla espaço (PC)
uis.InputBegan:Connect(function(input, processed)
    if flying and input.KeyCode == Enum.KeyCode.Space then
        jumping = true
    end
end)
uis.InputEnded:Connect(function(input, processed)
    if flying and input.KeyCode == Enum.KeyCode.Space then
        jumping = false
    end
end)

-- Detectar pulo (Mobile + PC)
humanoid.Jumping:Connect(function()
    if flying then
        jumping = true
    end
end)

-- Voo contínuo + movimento horizontal
RunService.RenderStepped:Connect(function(delta)
    if flying and jumping and hrp then
        -- Movimento vertical contínuo (voo infinito)
        hrp.CFrame = hrp.CFrame + Vector3.new(0, flySpeed * delta, 0)
        -- Movimento horizontal: PC (W/A/S/D) ou mobile (joystick)
        local moveDir = humanoid.MoveDirection
        if moveDir.Magnitude > 0 then
            hrp.CFrame = hrp.CFrame + moveDir * flySpeed * delta
        end
    end
end)
