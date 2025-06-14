-- Script para destacar jogadores com vermelho neon visível através das paredes

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Função para criar o destaque
local function highlightCharacter(character)
    if character and character:FindFirstChild("HumanoidRootPart") then
        -- Evita múltiplos destaques no mesmo personagem
        if character:FindFirstChild("RedNeonHighlight") then return end

        -- Cria o BoxHandleAdornment
        local adornment = Instance.new("BoxHandleAdornment")
        adornment.Name = "RedNeonHighlight"
        adornment.Adornee = character.HumanoidRootPart
        adornment.AlwaysOnTop = true
        adornment.ZIndex = 10
        adornment.Size = character.HumanoidRootPart.Size + Vector3.new(0.5, 0.5, 0.5)
        adornment.Color3 = Color3.fromRGB(255, 0, 0)
        adornment.Transparency = 0.25
        adornment.Parent = character
        adornment.Material = Enum.Material.Neon
    end
end

-- Função para remover o destaque ao sair
local function removeHighlight(character)
    if character and character:FindFirstChild("RedNeonHighlight") then
        character.RedNeonHighlight:Destroy()
    end
end

-- Atualiza todos os jogadores (exceto você)
local function updateAllHighlights()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            highlightCharacter(player.Character)
        end
    end
end

-- Eventos para quando um jogador entra ou respawna
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        if player ~= LocalPlayer then
            highlightCharacter(character)
        end
    end)
end)

Players.PlayerRemoving:Connect(function(player)
    if player.Character then
        removeHighlight(player.Character)
    end
end)

-- Caso alguém respawne
for _, player in ipairs(Players:GetPlayers()) do
    player.CharacterAdded:Connect(function(character)
        if player ~= LocalPlayer then
            highlightCharacter(character)
        end
    end)
end

-- Inicial
updateAllHighlights()
