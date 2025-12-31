-- HIBLOX HUB V3.4 - FLY PRONTO INTEGRADO
repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

-- Limpar execuções anteriores
if LocalPlayer.PlayerGui:FindFirstChild("HibloxSystem") then
    LocalPlayer.PlayerGui.HibloxSystem:Destroy()
end

-- Configurações Premium
_G.Aimbot = false
_G.Hitbox = false
_G.Esp = false
_G.Noclip = false
_G.Spinbot = false
_G.NoSit = false
_G.Speed = false
_G.Jump = false
_G.Fly = false
_G.Fov = 120
_G.InfJump = false

-- FOV Circle Premium
local fovc = Drawing.new("Circle")
fovc.Thickness = 2
fovc.Color = Color3.fromRGB(0, 255, 150)
fovc.Filled = false
fovc.Transparency = 0.8
fovc.Visible = false

-- Interface Moderna
local sg = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
sg.Name = "HibloxSystem"
sg.ResetOnSpawn = false

-- Main Frame
local frame = Instance.new("Frame", sg)
frame.Size = UDim2.new(0, 240, 0, 520)
frame.Position = UDim2.new(0.02, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
frame.Active = true
frame.Draggable = true

local gradient = Instance.new("UIGradient", frame)
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(35, 35, 45)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(15, 15, 25))
}
gradient.Rotation = 45

local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 16)
local stroke = Instance.new("UIStroke", frame)
stroke.Color = Color3.fromRGB(0, 255, 150)
stroke.Thickness = 2

-- BOLINHA PERFEITA
local ball = Instance.new("TextButton", sg)
ball.Size = UDim2.new(0, 60, 0, 60)
ball.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
ball.Text = "HIBLOX"
ball.TextColor3 = Color3.new(1,1,1)
ball.Font = Enum.Font.GothamBold
ball.TextSize = 14
ball.Visible = false
ball.Active = true
ball.Draggable = true

local ballCorner = Instance.new("UICorner", ball)
ballCorner.CornerRadius = UDim.new(1, 0)
local ballGradient = Instance.new("UIGradient", ball)
ballGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 255, 150)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 200, 100))
}

-- Header
local header = Instance.new("Frame", frame)
header.Size = UDim2.new(1, 0, 0, 50)
header.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
header.BackgroundTransparency = 0.1
local headerCorner = Instance.new("UICorner", header)
headerCorner.CornerRadius = UDim.new(0, 16)

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -40, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "HIBLOX HUB V3.4"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 16

local close = Instance.new("TextButton", header)
close.Size = UDim2.new(0, 30, 0, 30)
close.Position = UDim2.new(1, -35, 0.5, -15)
close.Text = "✕"
close.TextColor3 = Color3.new(1,1,1)
close.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
close.Font = Enum.Font.GothamBold
close.TextSize = 18
Instance.new("UICorner", close).CornerRadius = UDim.new(0, 8)

-- ScrollFrame
local scroll = Instance.new("ScrollingFrame", frame)
scroll.Size = UDim2.new(1, -20, 1, -70)
scroll.Position = UDim2.new(0, 10, 0, 60)
scroll.BackgroundTransparency = 1
scroll.CanvasSize = UDim2.new(0, 0, 0, 900)
scroll.ScrollBarThickness = 6
scroll.ScrollBarImageColor3 = Color3.fromRGB(0, 255, 150)
local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0, 8)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Função Botão Premium
local function createToggle(name, var, callback)
    local container = Instance.new("Frame", scroll)
    container.Size = UDim2.new(0.95, 0, 0, 40)
    container.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    local contCorner = Instance.new("UICorner", container)
    contCorner.CornerRadius = UDim.new(0, 12)
    local contStroke = Instance.new("UIStroke", container)
    contStroke.Color = Color3.fromRGB(50, 50, 55)
    contStroke.Thickness = 1
    
    local label = Instance.new("TextLabel", container)
    label.Size = UDim2.new(0.75, 0, 1, 0)
    label.Text = name
    label.TextColor3 = Color3.new(1,1,1)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.Gotham
    label.TextSize = 14
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local toggle = Instance.new("TextButton", container)
    toggle.Size = UDim2.new(0, 50, 0, 25)
    toggle.Position = UDim2.new(1, -55, 0.5, -12.5)
    toggle.Text = "OFF"
    toggle.TextColor3 = Color3.new(1,1,1)
    toggle.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
    toggle.Font = Enum.Font.GothamBold
    toggle.TextSize = 12
    local togCorner = Instance.new("UICorner", toggle)
    togCorner.CornerRadius = UDim.new(0, 12)
    
    toggle.MouseButton1Click:Connect(function()
        _G[var] = not _G[var]
        local status = _G[var] and "ON" or "OFF"
        local color = _G[var] and Color3.fromRGB(0, 255, 150) or Color3.fromRGB(60, 60, 65)
        
        toggle.Text = status
        toggle.BackgroundColor3 = color
        
        if var == "Aimbot" then 
            fovc.Visible = _G[var]
            fovc.Color = _G[var] and Color3.fromRGB(0, 255, 150) or Color3.fromRGB(255, 0, 0)
        end
        
        if callback then callback() end
    end)
end

-- SEPARADOR VISÍVEL
local function createSeparator(text)
    local sepContainer = Instance.new("Frame", scroll)
    sepContainer.Size = UDim2.new(0.95, 0, 0, 35)
    sepContainer.BackgroundTransparency = 1
    
    local sep = Instance.new("TextLabel", sepContainer)
    sep.Size = UDim2.new(1, 0, 0.6, 0)
    sep.Text = text
    sep.TextColor3 = Color3.fromRGB(0, 255, 150)
    sep.BackgroundTransparency = 1
    sep.Font = Enum.Font.GothamBold
    sep.TextSize = 15
    sep.TextXAlignment = Enum.TextXAlignment.Center
    
    local line = Instance.new("Frame", sepContainer)
    line.Size = UDim2.new(1, 0, 0, 2)
    line.Position = UDim2.new(0, 0, 0.8, 0)
    line.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
    line.BorderSizePixel = 0
    local lineCorner = Instance.new("UICorner", line)
end

-- SOM FREE BROOKHAVEN
local function playBrookhavenSoundFree(soundId)
    soundId = tostring(soundId):gsub("%D", "")
    pcall(function()
        ReplicatedStorage.RE["1Play1Radio1"]:FireServer(soundId)
    end)
    pcall(function()
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name == "Radio" or obj.Name:find("radio") then
                pcall(function()
                    obj.Event:FireServer(soundId)
                end)
            end
        end
    end)
    pcall(function()
        local sound = Instance.new("Sound", workspace)
        sound.SoundId = "rbxassetid://" .. soundId
        sound.Volume = 1
        sound:Play()
        game:GetService("Debris"):AddItem(sound, 10)
    end)
end

-- INTERFACE COMPLETA
createSeparator("⚔️ COMBATE")
createToggle("🎯 Aimbot Premium (M2)", "Aimbot")
createToggle("📦 Hitbox Expander", "Hitbox")
createToggle("👁️ ESP Wallhack", "Esp")

createSeparator("🚀 MOVIMENTAÇÃO")
createToggle("🚶 Anti-Assento", "NoSit")
createToggle("👻 Noclip", "Noclip")
createToggle("💫 Spinbot", "Spinbot")
createToggle("⚡ Speed (120)", "Speed")
createToggle("🦘 Jump Power (120)", "Jump")
createToggle("✈️ Infinite Jump (SPACE)", "InfJump")

-- FLY PRONTO INTEGRADO (SEU SCRIPT EXATO)
local flyToggle = nil
createToggle("🛩️ Fly GUI Pronto", "Fly", function()
    if _G.Fly then
        -- Executa o fly GUI pronto
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fly-Gui-45909"))()
        flyToggle.Text = "ON"
    else
        -- Tenta destruir fly GUI se existir (caso tenha toggle off)
        pcall(function()
            if LocalPlayer.PlayerGui:FindFirstChild("FlyGui") then
                LocalPlayer.PlayerGui.FlyGui:Destroy()
            end
        end)
    end
end)

createSeparator("🎵 SOM FREE")
local soundBox = Instance.new("TextBox", scroll)
soundBox.Size = UDim2.new(0.95, 0, 0, 40)
soundBox.PlaceholderText = "ID (ex: 130776739)"
soundBox.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
soundBox.TextColor3 = Color3.new(1,1,1)
soundBox.Font = Enum.Font.Gotham
soundBox.TextSize = 14
local soundCorner = Instance.new("UICorner", soundBox)
soundCorner.CornerRadius = UDim.new(0, 12)

local playSoundBtn = Instance.new("TextButton", scroll)
playSoundBtn.Size = UDim2.new(0.95, 0, 0, 40)
playSoundBtn.Text = "🎵 TOCAR FREE (SEM CAIXA)"
playSoundBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
playSoundBtn.TextColor3 = Color3.new(1,1,1)
playSoundBtn.Font = Enum.Font.GothamBold
playSoundBtn.TextSize = 14
Instance.new("UICorner", playSoundBtn).CornerRadius = UDim.new(0, 12)

playSoundBtn.MouseButton1Click:Connect(function()
    local id = soundBox.Text
    if id and id ~= "" then
        playBrookhavenSoundFree(id)
        playSoundBtn.Text = "✅ SOM ENVIADO!"
        playSoundBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        task.wait(1)
        playSoundBtn.Text = "🎵 TOCAR FREE (SEM CAIXA)"
        playSoundBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
    end
end)

-- Aimbot e resto das funções (igual antes)
local function getBestTarget()
    local bestTarget, shortestDistance = nil, _G.Fov + 1
    local mousePos = Vector2.new(Mouse.X, Mouse.Y)
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local head = player.Character.Head
            local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
            if onScreen then
                local distance = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
                if distance <= _G.Fov and distance < shortestDistance then
                    bestTarget = head
                    shortestDistance = distance
                end
            end
        end
    end
    return bestTarget
end

-- Loops principais
RunService.RenderStepped:Connect(function()
    fovc.Position = Vector2.new(Mouse.X, Mouse.Y + 36)
    fovc.Radius = _G.Fov
    
    if _G.Aimbot and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
        local target = getBestTarget()
        if target then
            local currentCFrame = Camera.CFrame
            local targetCFrame = CFrame.new(currentCFrame.Position, target.Position)
            Camera.CFrame = currentCFrame:lerp(targetCFrame, 0.25)
        end
    end

    local character = LocalPlayer.Character
    if character and character:FindFirstChildOfClass("Humanoid") then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        humanoid.WalkSpeed = _G.Speed and 120 or 16
        humanoid.JumpPower = _G.Jump and 120 or 50
        if _G.NoSit then humanoid.Sit = false end
        
        local rootPart = character:FindFirstChild("HumanoidRootPart")
        if _G.Spinbot and rootPart then 
            rootPart.CFrame = rootPart.CFrame * CFrame.Angles(0, math.rad(40), 0)
        end
    end
    
    if _G.InfJump and UserInputService:IsKeyDown(Enum.KeyCode.Space) then
        if LocalPlayer.Character then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local head = player.Character:FindFirstChild("Head")
            if head then
                if _G.Hitbox then
                    head.Size = Vector3.new(8, 8, 8)
                    head.Transparency = 0.6
                    head.CanCollide = false
                else
                    head.Size = Vector3.new(2, 1, 1)
                end
            end
            
            local highlight = player.Character:FindFirstChild("HibloxESP")
            if _G.Esp then
                if not highlight then
                    highlight = Instance.new("Highlight", player.Character)
                    highlight.Name = "HibloxESP"
                    highlight.FillColor = Color3.fromRGB(0, 255, 150)
                    highlight.OutlineColor = Color3.new(1,1,1)
                    highlight.FillTransparency = 0.5
                end
            elseif highlight then
                highlight:Destroy()
            end
        end
    end
end)

RunService.Stepped:Connect(function()
    if _G.Noclip and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- Controles Menu
local menuOpen = true
close.MouseButton1Click:Connect(function()
    menuOpen = false
    frame.Visible = false
    ball.Visible = true
    ball.Position = frame.Position
end)

ball.MouseButton1Click:Connect(function()
    menuOpen = true
    ball.Visible = false
    frame.Visible = true
    frame.Position = ball.Position
end)

print("✅ Hiblox Hub V3.4 - FLY PRONTO INTEGRADO!")
