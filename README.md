local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local attackDistance = 10 -- Distância para atacar (em studs)

-- Função para verificar se um modelo é um mob (NPC) e não um jogador
local function isMob(model)
    local humanoid = model:FindFirstChildOfClass("Humanoid")
    if humanoid and model ~= character then
        -- Se não for um personagem de jogador, considera como mob
        return not game.Players:GetPlayerFromCharacter(model)
    end
    return false
end

-- Loop principal
while true do
    for _, model in ipairs(workspace:GetChildren()) do
        if isMob(model) then
            local mobRoot = model:FindFirstChild("HumanoidRootPart")
            local humanoid = model:FindFirstChildOfClass("Humanoid")
            if mobRoot and humanoid and humanoid.Health > 0 then
                local distance = (humanoidRootPart.Position - mobRoot.Position).magnitude
                if distance <= attackDistance then
                    humanoid.Health = 0 -- Mata o mob
                end
            end
        end
    end
    task.wait(0.2) -- Espera para não travar o jogo
end
