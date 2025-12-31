-- 🇧🇷 BRUZUCA HUB V6.4 - ESP + SCRIPT CORRIGIDOS
repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportService = game:GetService("TeleportService")
local SoundService = game:GetService("SoundService")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

-- VARIÁVEIS GLOBAIS
local Settings = {
    Aimbot = false,
    Hitbox = false,
    Esp = false,
    Noclip = false,
    Spinbot = false,
    NoSit = false,
    Speed = false,
    Jump = false,
    InfJump = false,
    Fov = 120,
    WindUI = nil
}

-- FOV Circle
local fovc = Drawing.new("Circle")
fovc.Thickness = 2
fovc.Color = Color3.fromRGB(0, 255, 150)
fovc.Filled = false
fovc.Transparency = 0.8
fovc.Visible = false

-- ESP Highlights Storage
local ESPHighlights = {}

-- 🎵 MÚSICA AUTOMÁTICA (PROTEGIDA)
pcall(function()
    local welcomeSound = Instance.new("Sound")
    welcomeSound.SoundId = "rbxassetid://97011217688307"
    welcomeSound.Volume = 0.5
    welcomeSound.Parent = SoundService
    welcomeSound:Play()
    game:GetService("Debris"):AddItem(welcomeSound, 35)
end)

-- WINDUI (FALLBACK SE FALHAR)
local success, WindUI = pcall(function()
    return loadstring(game:HttpGet("https://github.com/Footagesus/WindUI/releases/latest/download/main.lua"))()
end)

if not success then
    WindUI = {Notify = function() print("WindUI falhou, use console") end}
    print("❌ WindUI falhou - HUB carregado no console!")
end

Settings.WindUI = WindUI
local Window = WindUI:CreateWindow({
    Title = "🇧🇷 BRUZUCA HUB V6.4 🇧🇷",
    Icon = "rbxassetid://6031087047",
    Size = UDim2.fromOffset(650, 500),
    Theme = "Dark",
    Resizable = true,
    BackgroundImageTransparency = 0.35,
    BackgroundImage = "rbxassetid://8425069728"
})

-- 🥵 MENSAGEM INICIAL
pcall(function()
    WindUI:Notify({
        Title = "🥵 VOCÊ ESTÁ USANDO O BRUZUCA HUB 🥵",
        Content = "🎵 Música tocando por 35s! ID: 97011217688307 🔥🇧🇷",
        Duration = 8,
        Image = "success"
    })
end)

-- 🌟 ABA 1: ⚔️ COMBATE (ESP CORRIGIDO)
local CombatTab = Window:Tab({ Title = "⚔️Combate", Icon = "sword" })
CombatTab:Toggle({ Title = "🎯 Aimbot", Default = false, Callback = function(v) Settings.Aimbot = v; fovc.Visible = v end })
CombatTab:Toggle({ Title = "📦 Hitbox", Default = false, Callback = function(v) Settings.Hitbox = v end })
CombatTab:Toggle({ Title = "👁️ ESP Players", Default = false, Callback = function(v) Settings.Esp = v end })
CombatTab:Slider({ Title = "FOV aimbot", Min = 50, Max = 500, Default = 120, Callback = function(v) Settings.Fov = v; fovc.Radius = v end })

-- 🌟 ABA 2: 🚀 MOVIMENTAÇÃO
local MovementTab = Window:Tab({ Title = "🚀 Movimentação", Icon = "rocket" })
MovementTab:Toggle({ 
    Title = "⚡ Speed", 
    Default = false, 
    Callback = function(v) 
        Settings.Speed = v
        local char = LocalPlayer.Character
        if char and char:FindFirstChildOfClass("Humanoid") then 
            char.Humanoid.WalkSpeed = v and 120 or 16 
        end
        if v then
            pcall(function()
                WindUI:Notify({
                    Title = "🤣 TOMARA QUE SE MATE 🤣",
                    Content = "Speed ativado! Corre filho da puta! 🔥",
                    Duration = 4,
                    Image = "success"
                })
            end)
        end
    end 
})
MovementTab:Toggle({ Title = "🦘 Jump 120", Default = false, Callback = function(v) Settings.Jump = v; local char = LocalPlayer.Character; if char and char:FindFirstChildOfClass("Humanoid") then char.Humanoid.JumpPower = v and 120 or 50 end end })
MovementTab:Toggle({ Title = "👻 Noclip", Default = false, Callback = function(v) Settings.Noclip = v end })
MovementTab:Toggle({ Title = "⭐ Spinbot", Default = false, Callback = function(v) Settings.Spinbot = v end })
MovementTab:Toggle({ Title = "✈️ Inf Jump", Default = false, Callback = function(v) Settings.InfJump = v end })
MovementTab:Button({ Title = "✈️ Fly", Callback = function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fly-Gui-45909"))() end })

-- 🌟 ABA 3: 🎵 BROOKHAVEN
local AudioTab = Window:Tab({ Title = "🎵 Brookhaven", Icon = "music" })
AudioTab:Button({ 
    Title = "🔄 REJOIN AGORA", 
    Callback = function() 
        pcall(function()
            WindUI:Notify({Title = "🚀 REJOIN...", Duration = 3, Image = "success"})
        end)
        wait(1)
        TeleportService:Teleport(game.PlaceId, LocalPlayer) 
    end 
})

local SoundInput = AudioTab:Textbox({ Title = "ID Som", Placeholder = "130776739" })
AudioTab:Button({
    Title = "▶️ TOCAR RÁDIO",
    Callback = function()
        local soundId = SoundInput.Value:gsub("%D", "")
        if #soundId > 5 then
            pcall(function()
                if ReplicatedStorage:FindFirstChild("RE") then
                    for _, obj in pairs(ReplicatedStorage.RE:GetChildren()) do
                        if obj:IsA("RemoteEvent") then
                            pcall(function() obj:FireServer("rbxassetid://" .. soundId) end)
                        end
                    end
                end
            end)
            pcall(function() WindUI:Notify({Title = "✅ RÁDIO: " .. soundId, Duration = 3}) end)
        end
    end
})

-- 🌟 ABA 4: 👤 PLAYER
local PlayerTab = Window:Tab({ Title = "👤 Player", Icon = "user" })
PlayerTab:Toggle({ Title = "🚶 Anti-Sit", Default = false, Callback = function(v) Settings.NoSit = v end })
PlayerTab:Button({ Title = "🤣 NÃO TIRA!", Callback = function() WindUI:Notify({Title = "🤣 TIRA NÃO SEU FILHO DA PUTA 🤣", Duration = 6}) end })

-- 🔒 ANTI-FECHAR
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt, false)
mt.__namecall = newcclosure(function(self, ...)
    local m = getnamecallmethod()
    if m == "Destroy" and tostring(self):find("WindUI") then
        pcall(function() WindUI:Notify({Title = "🤣 TIRA NÃO SEU FILHO DA PUTA 🤣", Duration = 6}) end)
        return
    end
    return old(self, ...)
end)
setreadonly(mt, true)

-- LOOP PRINCIPAL (ESP CORRIGIDO)
RunService.Heartbeat:Connect(function()
    -- FOV
    fovc.Position = Vector2.new(Mouse.X, Mouse.Y + 36)
    fovc.Radius = Settings.Fov
    fovc.Visible = Settings.Aimbot
    
    -- Aimbot
    if Settings.Aimbot and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
        local closest = nil
        local dist = math.huge
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Head") then
                local head = plr.Character.Head
                local pos, onScreen = Camera:WorldToViewportPoint(head.Position)
                if onScreen then
                    local screenDist = (Vector2.new(pos.X, pos.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                    if screenDist < dist and screenDist < Settings.Fov then
                        closest = head
                        dist = screenDist
                    end
                end
            end
        end
        if closest then
            Camera.CFrame = CFrame.lookAt(Camera.CFrame.Position, closest.Position)
        end
    end
    
    -- MOVIMENTOS + SPINBOT
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") and char:FindFirstChild("HumanoidRootPart") then
        local hum = char.Humanoid
        local root = char.HumanoidRootPart
        
        hum.WalkSpeed = Settings.Speed and 120 or 16
        hum.JumpPower = Settings.Jump and 120 or 50
        if Settings.NoSit then hum.Sit = false end
        if Settings.Spinbot then
            root.CFrame = root.CFrame * CFrame.Angles(0, math.rad(20), 0)
        end
    end
    
    -- ESP CORRIGIDO (NOVO SISTEMA)
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Head") then
            local highlight = plr.Character:FindFirstChild("BruzucaESP")
            if Settings.Esp then
                if not highlight then
                    highlight = Instance.new("Highlight")
                    highlight.Name = "BruzucaESP"
                    highlight.FillColor = Color3.fromRGB(255, 0, 0)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.FillTransparency = 0.5
                    highlight.Parent = plr.Character
                end
            elseif highlight then
                highlight:Destroy()
            end
            
            -- Hitbox
            local head = plr.Character.Head
            if head and Settings.Hitbox then
                head.Size = Vector3.new(8, 8, 8)
                head.Transparency = 0.7
                head.CanCollide = false
            end
        end
    end
    
    -- Inf Jump
    if Settings.InfJump and UserInputService:IsKeyDown(Enum.KeyCode.Space) then
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

RunService.Stepped:Connect(function()
    if Settings.Noclip and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

print("✅ 🇧🇷 BRUZUCA HUB V6.4 CARREGADO! ESP + SCRIPT CORRIGIDOS!")
