-- HIBLOX HUB V2.2 - FIXED & OPTIMIZED
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

-- Limpar UIs antigas para não bugar
local oldUI = LocalPlayer.PlayerGui:FindFirstChild("HibloxSystem")
if oldUI then oldUI:Destroy() end

-- Configurações
_G.Aimbot = false
_G.Hitbox = false
_G.Esp = false
_G.Noclip = false
_G.Spinbot = false
_G.Speed = false
_G.Jump = false
_G.Fov = 150

-- FOV Circle
local fovc = Drawing.new("Circle")
fovc.Thickness = 1
fovc.Color = Color3.fromRGB(0, 170, 255)
fovc.Visible = false
fovc.Transparency = 1

-- UI PRINCIPAL
local sg = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
sg.Name = "HibloxSystem"
sg.ResetOnSpawn = false

local frame = Instance.new("Frame", sg)
frame.Name = "Main"
frame.Size = UDim2.new(0, 170, 0, 320)
frame.Position = UDim2.new(0.05, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
Instance.new("UICorner", frame)

-- Bolinha (Minimizado)
local ball = Instance.new("TextButton", sg)
ball.Size = UDim2.new(0, 45, 0, 45)
ball.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
ball.Text = "H"
ball.TextColor3 = Color3.new(1, 1, 1)
ball.TextSize = 20
ball.Visible = false
ball.Draggable = true
Instance.new("UICorner", ball).CornerRadius = UDim.new(1, 0)

-- Botão Minimizar
local close = Instance.new("TextButton", frame)
close.Size = UDim2.new(0, 30, 0, 30)
close.Position = UDim2.new(0.8, 0, 0, 2)
close.Text = "X"
close.TextColor3 = Color3.new(1, 0, 0)
close.BackgroundTransparency = 1

close.MouseButton1Click:Connect(function()
    frame.Visible = false
    ball.Visible = true
    ball.Position = frame.Position
end)

ball.MouseButton1Click:Connect(function()
    frame.Visible = true
    ball.Visible = false
    frame.Position = ball.Position
end)

-- Lista de Opções
local scroll = Instance.new("ScrollingFrame", frame)
scroll.Size = UDim2.new(1, -10, 1, -40)
scroll.Position = UDim2.new(0, 5, 0, 35)
scroll.BackgroundTransparency = 1
scroll.CanvasSize = UDim2.new(0, 0, 0, 450)
scroll.ScrollBarThickness = 3

local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0, 5)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Criar Botões
local function addBtn(name, var, call)
    local b = Instance.new("TextButton", scroll)
    b.Size = UDim2.new(0.9, 0, 0, 30)
    b.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    b.Text = name .. ": OFF"
    b.TextColor3 = Color3.new(1, 1, 1)
    b.Font = Enum.Font.SourceSansBold
    Instance.new("UICorner", b)
    
    b.MouseButton1Click:Connect(function()
        if var then            _G[var] = not _G[var]
            b.Text = name .. ": " .. (_G[var] and "ON" or "OFF")
            b.BackgroundColor3 = _G[var] and Color3.fromRGB(0, 120, 0) or Color3.fromRGB(40, 40, 40)
            if var == "Aimbot" then fovc.Visible = _G[var] end
        end
        if call then call() end
    end)
end

-- Adicionando as Funções
addBtn("AIMBOT (M2)", "Aimbot")
addBtn("HITBOX", "Hitbox")
addBtn("ESP", "Esp")
addBtn("NOCLIP", "Noclip")
addBtn("SPINBOT", "Spinbot")
addBtn("SPEED (100)", "Speed")
addBtn("JUMP (100)", "Jump")
addBtn("FLY", nil, function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Universal-fly-script-28708"))() end)

-- Brookhaven Radio ID Input
local idBox = Instance.new("TextBox", scroll)
idBox.Size = UDim2.new(0.9, 0, 0, 30)
idBox.PlaceholderText = "ID do Som"
idBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
idBox.TextColor3 = Color3.new(1, 1, 1)Instance.new("UICorner", idBox)

addBtn("TOCAR RADIO", nil, function()
    local re = game:GetService("ReplicatedStorage"):FindFirstChild("RE")
    local radio = re and re:FindFirstChild("1Play1Radio1")
    if radio then radio:FireServer(idBox.Text) end
end)

-- LOGICA DOS CHEATS
local function getClosest()
    local t, d = nil, _G.Fov
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
            local p, vis = Camera:WorldToViewportPoint(v.Character.Head.Position)
            if vis then
                local m = (Vector2.new(p.X, p.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                if m < d then t, d = v.Character.Head, m end
            end
        end
    end
    return t
end

RunService.RenderStepped:Connect(function()
    fovc.Position = Vector2.new(Mouse.X, Mouse.Y + 36)
    fovc.Radius = _G.Fov
    
    if _G.Aimbot and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
        local target = getClosest()
        if target then Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position) end
    end

    local char = LocalPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        local hum = char:FindFirstChildOfClass("Humanoid")
        hum.WalkSpeed = _G.Speed and 100 or 16
        hum.JumpPower = _G.Jump and 100 or 50
        if _G.Spinbot then char.HumanoidRootPart.CFrame *= CFrame.Angles(0, math.rad(50), 0) end
    end

    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
            local h = v.Character.Head
            h.Size = _G.Hitbox and Vector3.new(15, 15, 15) or Vector3.new(2, 1, 1)            h.Transparency = _G.Hitbox and 0.7 or 0
            h.CanCollide = not _G.Hitbox
            
            local hl = v.Character:FindFirstChild("ESPHL")
            if _G.Esp then
                if not hl then hl = Instance.new("Highlight", v.Character) hl.Name = "ESPHL" end
            elseif hl then hl:Destroy() end        end
    end
end)

RunService.Stepped:Connect(function()
    if _G.Noclip and LocalPlayer.Character then
        for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end
end)
