local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local MarketplaceService = game:GetService("MarketplaceService")
local TeleportService = game:GetService("TeleportService")

-- SISTEMA DONO
local DONO_DO_SCRIPT = "gggtrt9o"
local function isDono()
    return LocalPlayer.Name == DONO_DO_SCRIPT
end

-- VARIÁVEIS GLOBAIS
local UsuariosComScript = {}
local KickButton, InputKick, ConfirmKick
local PlaceID = game.PlaceId
local ServerID = game.JobId

-- REGISTRA USUÁRIO
local function RegistrarUsuario(playerName)
    if not table.find(UsuariosComScript, playerName) then
        table.insert(UsuariosComScript, playerName)
    end
end
RegistrarUsuario(LocalPlayer.Name)

-- *** SISTEMA DE NOTIFICAÇÕES ***
local NotificationGui = Instance.new("ScreenGui")
NotificationGui.Name = "BruzucaNotifications"
NotificationGui.Parent = game.CoreGui
NotificationGui.ResetOnSpawn = false

local NotificationsFrame = Instance.new("Frame")
NotificationsFrame.Name = "Notifications"
NotificationsFrame.Parent = NotificationGui
NotificationsFrame.Size = UDim2.new(0, 300, 1, 0)
NotificationsFrame.Position = UDim2.new(1, -320, 0, 20)
NotificationsFrame.BackgroundTransparency = 1

local NotifListLayout = Instance.new("UIListLayout")
NotifListLayout.Parent = NotificationsFrame
NotifListLayout.Padding = UDim.new(0, 5)
NotifListLayout.SortOrder = Enum.SortOrder.LayoutOrder

local function CreateNotification(PlayerName, DisplayName, Message)
    local Notif = Instance.new("Frame")
    Notif.Size = UDim2.new(1, -20, 0, 60)
    Notif.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
    Notif.Parent = NotificationsFrame
    
    local NotifCorner = Instance.new("UICorner")
    NotifCorner.CornerRadius = UDim.new(0, 12)
    NotifCorner.Parent = Notif
    
    local NotifStroke = Instance.new("UIStroke")
    NotifStroke.Color = Color3.fromRGB(0, 255, 150)
    NotifStroke.Thickness = 2
    NotifStroke.Parent = Notif
    
    local NotifHeader = Instance.new("TextLabel")
    NotifHeader.Size = UDim2.new(1, 0, 0.5, 0)
    NotifHeader.Position = UDim2.new(0, 15, 0, 8)
    NotifHeader.BackgroundTransparency = 1
    NotifHeader.Font = Enum.Font.GothamBold
    NotifHeader.Text = "👑🇧🇷 " .. DisplayName
    NotifHeader.TextColor3 = Color3.fromRGB(0, 255, 150)
    NotifHeader.TextSize = 15
    NotifHeader.TextXAlignment = Enum.TextXAlignment.Left
    NotifHeader.Parent = Notif
    
    local NotifText = Instance.new("TextLabel")
    NotifText.Size = UDim2.new(1, -20, 0.5, 0)
    NotifText.Position = UDim2.new(0, 15, 0.5, 2)
    NotifText.BackgroundTransparency = 1
    NotifText.Font = Enum.Font.Gotham
    NotifText.Text = Message
    NotifText.TextColor3 = Color3.new(1,1,1)
    NotifText.TextSize = 13
    NotifText.TextXAlignment = Enum.TextXAlignment.Left
    NotifText.TextWrapped = true
    NotifText.Parent = Notif
    
    Notif:TweenSize(UDim2.new(1, -20, 0, 60), "Out", "Quad", 0.3)
    game:GetService("Debris"):AddItem(Notif, 5)
    Notif:TweenSize(UDim2.new(0, 0, 0, 0), "In", "Quad", 0.3, false, 4.7)
end

-- GUI PRINCIPAL
local ChatGui = Instance.new("ScreenGui")
ChatGui.Name = "BruzucaChatVIP"
ChatGui.Parent = game.CoreGui
ChatGui.ResetOnSpawn = false

local ChatFrame = Instance.new("Frame")
ChatFrame.Name = "ChatFrame"
ChatFrame.Parent = ChatGui
ChatFrame.Size = UDim2.new(0, 500, 0, 320)
ChatFrame.Position = UDim2.new(0, 20, 0.2, 0)
ChatFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 25)
ChatFrame.BackgroundTransparency = 0.05
ChatFrame.BorderSizePixel = 0
ChatFrame.Active = true
ChatFrame.Draggable = true
ChatFrame.Visible = true

local ChatCorner = Instance.new("UICorner")
ChatCorner.CornerRadius = UDim.new(0, 16)
ChatCorner.Parent = ChatFrame

local ChatStroke = Instance.new("UIStroke")
ChatStroke.Color = Color3.fromRGB(0, 255, 150)
ChatStroke.Thickness = 3
ChatStroke.Parent = ChatFrame

-- TÍTULO
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Parent = ChatFrame
TitleLabel.Size = UDim2.new(1, -70, 0, 40)
TitleLabel.Position = UDim2.new(0, 20, 0, 10)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "💬 BRUZUCA CHAT VIP" .. (isDono() and " [👑DONO]" or "") .. " | Brookhaven"
TitleLabel.TextColor3 = isDono() and Color3.fromRGB(255, 215, 0) or Color3.fromRGB(0, 255, 150)
TitleLabel.TextSize = 18
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- BOTÃO FECHAR
local CloseBtn = Instance.new("TextButton")
CloseBtn.Parent = ChatFrame
CloseBtn.Size = UDim2.new(0, 35, 0, 35)
CloseBtn.Position = UDim2.new(1, -45, 0, 10)
CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 70, 70)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.new(1,1,1)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20
local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 10)
CloseCorner.Parent = CloseBtn

-- *** BOTÕES DONO (SÓ ELE VÊ) ***
local DeleteLastBtn, ClearAllBtn
local MESSAGES_START_Y = 95

if isDono() then
    -- 🗑️ 1
    DeleteLastBtn = Instance.new("TextButton")
    DeleteLastBtn.Name = "DeleteLast"
    DeleteLastBtn.Parent = ChatFrame
    DeleteLastBtn.Size = UDim2.new(0, 60, 0, 30)
    DeleteLastBtn.Position = UDim2.new(0, 20, 0, 55)
    DeleteLastBtn.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
    DeleteLastBtn.Text = "🗑️ 1"
    DeleteLastBtn.TextColor3 = Color3.new(1,1,1)
    DeleteLastBtn.Font = Enum.Font.GothamBold
    DeleteLastBtn.TextSize = 14
    local DeleteCorner = Instance.new("UICorner")
    DeleteCorner.CornerRadius = UDim.new(0, 8)
    DeleteCorner.Parent = DeleteLastBtn
    
    -- 🗑️ TODOS
    ClearAllBtn = Instance.new("TextButton")
    ClearAllBtn.Name = "ClearAll"
    ClearAllBtn.Parent = ChatFrame
    ClearAllBtn.Size = UDim2.new(0, 80, 0, 30)
    ClearAllBtn.Position = UDim2.new(0, 90, 0, 55)
    ClearAllBtn.BackgroundColor3 = Color3.fromRGB(255, 150, 0)
    ClearAllBtn.Text = "🗑️ TODOS"
    ClearAllBtn.TextColor3 = Color3.new(1,1,1)
    ClearAllBtn.Font = Enum.Font.GothamBold
    ClearAllBtn.TextSize = 14
    local ClearCorner = Instance.new("UICorner")
    ClearCorner.CornerRadius = UDim.new(0, 8)
    ClearCorner.Parent = ClearAllBtn
    
    -- 🚫 KICK POR NICK (CORRIGIDO COM REJOIN)
    KickButton = Instance.new("TextButton")
    KickButton.Name = "KickPlayer"
    KickButton.Parent = ChatFrame
    KickButton.Size = UDim2.new(0, 70, 0, 30)
    KickButton.Position = UDim2.new(0, 180, 0, 55)
    KickButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    KickButton.Text = "🚫 KICK"
    KickButton.TextColor3 = Color3.new(1,1,1)
    KickButton.Font = Enum.Font.GothamBold
    KickButton.TextSize = 12
    local KickCorner = Instance.new("UICorner")
    KickCorner.CornerRadius = UDim.new(0, 8)
    KickCorner.Parent = KickButton
else
    MESSAGES_START_Y = 55
end

-- MENSAGENS FRAME
local MessagesFrame = Instance.new("ScrollingFrame")
MessagesFrame.Parent = ChatFrame
MessagesFrame.Position = UDim2.new(0, 20, 0, MESSAGES_START_Y)
MessagesFrame.Size = UDim2.new(1, -40, 1, -90)
MessagesFrame.BackgroundTransparency = 1
MessagesFrame.ScrollBarThickness = 6
MessagesFrame.ScrollBarImageColor3 = Color3.fromRGB(0, 255, 150)
MessagesFrame.CanvasSize = UDim2.new(0, 0, 0, 0)

local ListLayout = Instance.new("UIListLayout")
ListLayout.Parent = MessagesFrame
ListLayout.Padding = UDim.new(0, 6)
ListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- INPUT
local InputFrame = Instance.new("Frame")
InputFrame.Parent = ChatFrame
InputFrame.Size = UDim2.new(1, -40, 0, 35)
InputFrame.Position = UDim2.new(0, 20, 1, -45)
InputFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
local InputCorner = Instance.new("UICorner")
InputCorner.CornerRadius = UDim.new(0, 12)
InputCorner.Parent = InputFrame

local InputBox = Instance.new("TextBox")
InputBox.Parent = InputFrame
InputBox.Size = UDim2.new(1, -20, 1, 0)
InputBox.Position = UDim2.new(0, 12, 0, 0)
InputBox.BackgroundTransparency = 1
InputBox.Font = Enum.Font.Gotham
InputBox.Text = ""
InputBox.PlaceholderText = "Digite sua mensagem... (ENTER)"
InputBox.TextColor3 = Color3.new(1,1,1)
InputBox.TextSize = 15
InputBox.TextXAlignment = Enum.TextXAlignment.Left

-- SISTEMA MENSAGENS + VIP
local MessagesList = {}
local MaxMessages = 30
local VIP_GAMEPASS_ID = 80342504

local function IsVipPlayer(player)
    local success, hasPass = pcall(function()
        return MarketplaceService:UserOwnsGamePassAsync(player.UserId, VIP_GAMEPASS_ID)
    end)
    return success and hasPass
end

local function AddMessage(PlayerName, DisplayName, TextMsg, IsMe)
    local playerObj = Players:FindFirstChild(PlayerName) or LocalPlayer
    local isVip = IsVipPlayer(playerObj)
    
    if not IsMe then
        CreateNotification(PlayerName, DisplayName or PlayerName, TextMsg)
    end
    
    local nomeColor = IsMe and Color3.new(1,1,1) or 
                     (isVip and Color3.fromRGB(255, 215, 0)) or 
                     Color3.fromRGB(170, 170, 255)
    
    local NewMsg = Instance.new("Frame")
    NewMsg.Size = UDim2.new(1, 0, 0, 42)
    NewMsg.BackgroundTransparency = 1
    NewMsg.Parent = MessagesFrame
    
    local MsgBg = Instance.new("Frame")
    MsgBg.Size = UDim2.new(1, 0, 1, 0)
    MsgBg.BackgroundColor3 = IsMe and Color3.fromRGB(0, 220, 130) or 
                           (isVip and Color3.fromRGB(255, 215, 0) or Color3.fromRGB(70, 70, 90))
    MsgBg.BackgroundTransparency = 0.3
    MsgBg.Parent = NewMsg
    
    local MsgCorner = Instance.new("UICorner")
    MsgCorner.CornerRadius = UDim.new(0, 10)
    MsgCorner.Parent = MsgBg
    
    local PlayerNameLabel = Instance.new("TextLabel")
    PlayerNameLabel.Size = UDim2.new(0, 280, 0.55, 0)
    PlayerNameLabel.Position = UDim2.new(0, 15, 0, 4)
    PlayerNameLabel.BackgroundTransparency = 1
    PlayerNameLabel.Font = Enum.Font.GothamBold
    PlayerNameLabel.Text = (IsMe and "👑🇧🇷 " or (isVip and "👑 " or "⚪ ")) .. (DisplayName or PlayerName)
    PlayerNameLabel.TextColor3 = nomeColor
    PlayerNameLabel.TextSize = 14
    PlayerNameLabel.TextXAlignment = Enum.TextXAlignment.Left
    PlayerNameLabel.Parent = MsgBg
    
    local MessageLabel = Instance.new("TextLabel")
    MessageLabel.Size = UDim2.new(1, -30, 0.45, 0)
    MessageLabel.Position = UDim2.new(0, 15, 0.55, 0)
    MessageLabel.BackgroundTransparency = 1
    MessageLabel.Font = Enum.Font.Gotham
    MessageLabel.Text = TextMsg
    MessageLabel.TextColor3 = Color3.new(1,1,1)
    MessageLabel.TextSize = 13
    MessageLabel.TextXAlignment = Enum.TextXAlignment.Left
    MessageLabel.TextWrapped = true
    MessageLabel.Parent = MsgBg
    
    table.insert(MessagesList, 1, NewMsg)
    if #MessagesList > MaxMessages then 
        MessagesList[#MessagesList]:Destroy()
        table.remove(MessagesList)
    end
    
    MessagesFrame.CanvasSize = UDim2.new(0, 0, 0, ListLayout.AbsoluteContentSize.Y + 20)
    MessagesFrame.CanvasPosition = Vector2.new(0, MessagesFrame.AbsoluteCanvasSize.Y)
end

-- *** EVENTOS DONO ***
if isDono() then
    -- DELETE LAST
    DeleteLastBtn.MouseButton1Click:Connect(function()
        if #MessagesList > 0 then
            MessagesList[#MessagesList]:Destroy()
            table.remove(MessagesList)
            MessagesFrame.CanvasSize = UDim2.new(0, 0, 0, ListLayout.AbsoluteContentSize.Y + 20)
        end
    end)
    
    -- CLEAR ALL
    ClearAllBtn.MouseButton1Click:Connect(function()
        for _, msg in pairs(MessagesFrame:GetChildren()) do
            if msg:IsA("Frame") and msg.Name ~= "ListLayout" then
                msg:Destroy()
            end
        end
        MessagesList = {}
    end)
    
    -- *** KICK POR NICK CORRIGIDO COM REJOIN AUTOMÁTICO ***
    KickButton.MouseButton1Click:Connect(function()
        -- REMOVE INPUTS ANTIGOS
        if InputKick then InputKick:Destroy() end
        if ConfirmKick then ConfirmKick:Destroy() end
        
        -- INPUT NICK
        InputKick = Instance.new("TextBox")
        InputKick.Parent = ChatFrame
        InputKick.Name = "KickInput"
        InputKick.Size = UDim2.new(0, 120, 0, 30)
        InputKick.Position = UDim2.new(0, 260, 0, 55)
        InputKick.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
        InputKick.Text = ""
        InputKick.PlaceholderText = "Nick do player"
        InputKick.TextColor3 = Color3.new(1,1,1)
        InputKick.Font = Enum.Font.Gotham
        InputKick.TextSize = 13
        local InputKickCorner = Instance.new("UICorner")
        InputKickCorner.CornerRadius = UDim.new(0, 8)
        InputKickCorner.Parent = InputKick
        
        -- BOTÃO OK
        ConfirmKick = Instance.new("TextButton")
        ConfirmKick.Parent = ChatFrame
        ConfirmKick.Size = UDim2.new(0, 40, 0, 30)
        ConfirmKick.Position = UDim2.new(0, 385, 0, 55)
        ConfirmKick.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        ConfirmKick.Text = "OK"
        ConfirmKick.TextColor3 = Color3.new(1,1,1)
        ConfirmKick.Font = Enum.Font.GothamBold
        ConfirmKick.TextSize = 14
        local ConfirmCorner = Instance.new("UICorner")
        ConfirmCorner.CornerRadius = UDim.new(0, 8)
        ConfirmCorner.Parent = ConfirmKick
        
        ConfirmKick.MouseButton1Click:Connect(function()
            local targetNick = InputKick.Text
            
            -- PROCURA PELO NICK (DISPLAYNAME OU USERNAME)
            local targetPlayer = nil
            for _, player in pairs(Players:GetPlayers()) do
                if player.DisplayName:lower():find(targetNick:lower()) or 
                   player.Name:lower():find(targetNick:lower()) then
                    targetPlayer = player
                    break
                end
            end
            
            if targetPlayer and targetPlayer ~= LocalPlayer then
                -- *** KICK REAL COM REJOIN AUTOMÁTICO ***
                AddMessage("BRUZUCA HUB", "ADMIN", "🚫 " .. targetPlayer.DisplayName .. " (" .. targetPlayer.Name .. ") foi KICKADO com REJOIN!", false)
                CreateNotification("BRUZUCA HUB", "👑 " .. LocalPlayer.DisplayName, "🚫 " .. targetPlayer.DisplayName .. " REJOIN automático ativado!", false)
                
                -- REMOVE DA LISTA LOCAL
                for i, v in pairs(UsuariosComScript) do
                    if v == targetPlayer.Name then
                        table.remove(UsuariosComScript, i)
                        break
                    end
                end
                
                -- *** REJOIN AUTOMÁTICO DO TARGET PLAYER ***
                pcall(function()
                    TeleportService:TeleportToPlaceInstance(PlaceID, ServerID, {targetPlayer})
                end)
                
            elseif targetPlayer == LocalPlayer then
                CreateNotification("ERRO", "KICK", "❌ Você não pode se kickar!", false)
            else
                CreateNotification("ERRO", "KICK", "❌ Player não encontrado ou sem script!", false)
            end
            
            -- LIMPA INPUTS
            InputKick:Destroy()
            ConfirmKick:Destroy()
        end)
    end)
end

-- INPUT PRINCIPAL
InputBox.FocusLost:Connect(function(EnterPressed)
    if EnterPressed and InputBox.Text ~= "" and InputBox.Text:len() > 1 then
        local MsgText = InputBox.Text
        InputBox.Text = ""
        AddMessage(LocalPlayer.Name, LocalPlayer.DisplayName, MsgText, true)
        print("💬 [" .. LocalPlayer.DisplayName .. "]: " .. MsgText)
    end
end)

-- CONTROLES
UserInputService.InputBegan:Connect(function(Input)
    if Input.KeyCode == Enum.KeyCode.T then
        ChatFrame.Visible = not ChatFrame.Visible
        if ChatFrame.Visible then InputBox:CaptureFocus() end
    end
end)

CloseBtn.MouseButton1Click:Connect(function()
    ChatFrame.Visible = false
end)

-- TESTE
spawn(function()
    wait(1)
    local myVip = IsVipPlayer(LocalPlayer)
    AddMessage("BRUZUCA HUB", "BRUZUCA HUB", "🔔 NOTIFICAÇÕES ATIVAS! VIP: " .. (myVip and "SIM 👑" or "NÃO"), false)
    if isDono() then
        AddMessage("BRUZUCA HUB", "BRUZUCA HUB", "👑 DONO ggtrt9o - KICK COM REJOIN ATIVO! ✅", false)
    end
    AddMessage(LocalPlayer.Name, LocalPlayer.DisplayName, "✅ Chat + Notificações OK! 👑🇧🇷", true)
end)

print("🔔 CHAT BRUZUCA V6 - KICK COM REJOIN AUTOMÁTICO + DONO ggtrt9o ATIVO!")
