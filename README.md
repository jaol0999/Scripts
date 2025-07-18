--[[
    Script Local para Roblox: Roube um Brainrot - Hub personalizado (Oreon Lib)
    Requisitos:
        - OreonLib (coloque o OreonLib na mesma pasta ou carregue via require)
    Funcionalidades:
        - Tab "Roubar" no Oreon Hub
        - Toggle para teleportar (Tween) até a área de coleta dos Brainrots roubados
    Observações:
        - Ajuste a variável `brainrotColetaPos` para o local exato do jogo (posição Vector3 do ponto de coleta)
        - Use este script apenas para fins educativos e sempre respeite os termos do Roblox
]]

-- Carregue o OreonLib (altere o link se necessário, ou coloque o OreonLib como ModuleScript)
local OreonLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/oreon-dev/Oreon-Library/main/OreonLib.lua"))()

-- Defina a posição do ponto de coleta dos Brainrots roubados (ajuste para o local correto do seu jogo!)
local brainrotColetaPos = Vector3.new(0, 10, 0) -- <<== Altere para a posição correta!

-- Função de tween teleport
local function tweenTo(pos)
    local plr = game.Players.LocalPlayer
    local char = plr.Character or plr.CharacterAdded:Wait()
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local tweenService = game:GetService("TweenService")
    local info = TweenInfo.new(
        (hrp.Position - pos).Magnitude / 80, -- duração proporcional à distância (ajuste 80 para velocidade)
        Enum.EasingStyle.Linear
    )
    local goal = {Position = pos}

    local tween = tweenService:Create(hrp, info, goal)
    tween:Play()
end

-- Crie o Hub
local win = OreonLib:CreateWindow({
    Name = "Roube um Brainrot - Hub",
    HidePremium = false,
    SaveConfig = false,
    ConfigFolder = "RoubeBrainrotOreon"
})

-- Tab "Roubar"
local roubarTab = win:CreateTab({
    Name = "Roubar",
    Icon = "rbxassetid://6031071053"
})

-- Toggle de Teleporte
local tpToggle
tpToggle = roubarTab:CreateToggle({
    Name = "Teleporte até Coleta",
    CurrentValue = false,
    Flag = "TPBrainrot",
    Callback = function(state)
        if state then
            tweenTo(brainrotColetaPos)
            -- Desativa o toggle após o teleporte
            task.wait(1)
            tpToggle:Set(false)
        end
    end
})

-- Créditos
roubarTab:CreateParagraph({
    Title = "Créditos",
    Content = "Script por jaol0999 | OreonLib por oreon-dev"
})
