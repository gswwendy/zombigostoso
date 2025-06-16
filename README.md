--[[ 
    Script Roblox Lua: Auto-Equip "Pistol" and Attack NPCs with Specific Names by Clicking on Their Heads
    ⚠️ Este script é para uso local (ex: executores de scripts) e NÃO deve ser usado para prejudicar jogos ou violar ToS do Roblox.
--]]

-- Configurações
local ITEM_NAME = "Pistol"
local NPC_NAME_KEYWORDS = {"monster", "slime", "zombie", "tank"}
local HEAD_PARTS = {"Head", "head", "HeadPart", "headpart"}

-- Serviços Roblox
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

-- Função para checar se o nome do NPC contém alguma das palavras-chave
local function isTargetNPC(npcName)
    npcName = npcName:lower()
    for _, keyword in ipairs(NPC_NAME_KEYWORDS) do
        if npcName:find(keyword) then
            return true
        end
    end
    return false
end

-- Função para equipar o item "Pistol" infinitamente
spawn(function()
    while true do
        local backpack = LocalPlayer:FindFirstChild("Backpack")
        if backpack then
            local pistol = backpack:FindFirstChild(ITEM_NAME)
            if pistol then
                LocalPlayer.Character.Humanoid:EquipTool(pistol)
            end
        end
        task.wait(0.1) -- mais rápido
    end
end)

-- Função para fazer cliques automáticos infinitos e o mais rápido possível nas cabeças dos NPCs-alvo
spawn(function()
    while true do
        for _, npc in ipairs(workspace:GetDescendants()) do
            if npc:IsA("Model") and npc ~= LocalPlayer.Character and isTargetNPC(npc.Name) then
                for _, partName in ipairs(HEAD_PARTS) do
                    local head = npc:FindFirstChild(partName)
                    if head and head:IsA("BasePart") then
                        local cam = workspace.CurrentCamera
                        local screenPos, onScreen = cam:WorldToViewportPoint(head.Position)
                        if onScreen then
                            mousemoveabs(screenPos.X, screenPos.Y) -- Precisa de executor que suporte mousemoveabs
                            mouse1click()
                        end
                        -- Nenhum wait aqui para máxima velocidade
                    end
                end
            end
        end
        -- Pequena pausa para evitar travamento, mas mantém máxima velocidade
        task.wait()
    end
end)

-- Funções de simulação de mouse (usadas por alguns executores, ex: Synapse X)
function mousemoveabs(x, y)
    -- Exemplo para Synapse X: mousemoveabs(x, y)
    -- Consulte a documentação do executor que você usa.
    -- Esta função é só um placeholder!
end

function mouse1click()
    -- Exemplo para Synapse X: mouse1click()
    -- Esta função é só um placeholder!
end
