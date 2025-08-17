-- Variáveis principais
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Backpack = LocalPlayer:WaitForChild("Backpack")

local invencivel = false

-- Lista de nomes dos Brainrots
local brainrots = {
    -- Raros
    ["Trippi Troppi"] = true,
    ["Tung Tung Tung Sahur"] = true,
    ["Gangster Footera"] = true,
    ["Boneca Ambalabu"] = true,
    ["Ta Ta Ta Ta Sahur"] = true,

    -- Épicos
    ["Capachino Assassino"] = true,
    ["Trulimero Trulicinea"] = true,
    ["Bananita Dolphinita"] = true,
    ["Brr.Brr. Patapim"] = true,

    -- Lendários
    ["Burbaloni Loliloli"] = true,
    ["Chef Crabracadebra"] = true,

    -- Míticos
    ["Frigo Camelo"] = true,
    ["Bombardiro Crocodilo"] = true,

    -- Deuses
    ["Cocofanto Elefanto"] = true,
    ["Tralalero Tralala"] = true,

    -- Secretos
    ["La Vacca Saturno Saturnita"] = true,
    ["Graipuss Medussi"] = true,
    ["Los Tralaleritos"] = true,
}

-- Criar botão na tela
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Button = Instance.new("TextButton")

Button.Name = "HitToggle"
Button.Size = UDim2.new(0, 100, 0, 50)
Button.Position = UDim2.new(0, 10, 0, 10)
Button.Text = "HIT ON"
Button.BackgroundColor3 = Color3.new(1, 0, 0)
Button.TextColor3 = Color3.new(1, 1, 1)
Button.Parent = ScreenGui

-- Função para verificar se está segurando algum Brainrot válido
local function isHoldingBrainrot()
    local tool = Character:FindFirstChildOfClass("Tool")
    return tool and brainrots[tool.Name]
end

-- Alternar invencibilidade
Button.MouseButton1Click:Connect(function()
    invencivel = not invencivel
    if invencivel then
        Button.Text = "HIT OFF"
        Button.BackgroundColor3 = Color3.new(0, 1, 0)
    else
        Button.Text = "HIT ON"
        Button.BackgroundColor3 = Color3.new(1, 0, 0)
    end
end)

-- Impedir dano quando ativado
local function protegerJogador()
    local humanoide = Character:WaitForChild("Humanoid")
    humanoide:GetPropertyChangedSignal("Health"):Connect(function()
        if invencivel and isHoldingBrainrot() then
            humanoide.Health = humanoide.MaxHealth
        end
    end)
end

-- Atualizar proteção ao renascer
LocalPlayer.CharacterAdded:Connect(function(char)
    Character = char
    protegerJogador()
end)

-- Ativar proteção inicial
protegerJogador()
