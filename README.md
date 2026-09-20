--// =========================================================
--// ITALLO HUB - MM2
--// FULL MOBILE VERSION
--//
--// KEY
--// RGB
--// ESP GERAL
--// ESP ASSASSINO
--// ESP SHERIFF
--// ESP NOMES
--// AUTO FARM
--// ANTI-QUEDA
--// NOCLIP / SPEED / JUMP / FLY
--// TP POR LISTA
--// STATUS
--// =========================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Stats = game:GetService("Stats")

local LocalPlayer = Players.LocalPlayer

local CONFIG = {
    Key = "ITALLO-GOSTOSO-KKK",

    ESP = false,
    MurdererESP = true,
    SheriffESP = true,
    NameESP = false,

    CoinFarm = false,
    CoinFarmSpeed = 0.15, -- intervalo entre moedas
    CoinFarmWalkSpeed = 5, -- deslocamento lento; pode ser aumentado no campo do FARM

    Noclip = false,
    Speed = false,
    SpeedValue = 24,

    Jump = false,
    JumpValue = 60,

    Fly = false,
}

local Character
local Humanoid
local Root
local LastSafeCFrame
local CoinFlightTarget

--==========================================================
-- CHARACTER
--==========================================================

local function UpdateCharacter()

    Character = LocalPlayer.Character
        or LocalPlayer.CharacterAdded:Wait()

    Humanoid = Character:WaitForChild("Humanoid")
    Root = Character:WaitForChild("HumanoidRootPart")

    LastSafeCFrame = Root.CFrame
end

UpdateCharacter()

LocalPlayer.CharacterAdded:Connect(function()

    task.wait(1)

    pcall(UpdateCharacter)

end)

--==========================================================
-- RGB
--==========================================================

local RGBObjects = {}

local function AddRGB(Object)
    table.insert(RGBObjects, Object)
end

task.spawn(function()

    local Hue = 0

    while task.wait(0.03) do

        Hue = (Hue + 0.005) % 1

        local Color =
            Color3.fromHSV(Hue, 1, 1)

        for _,Object in ipairs(RGBObjects) do

            if Object and Object.Parent then

                pcall(function()
                    Object.Color = Color
                end)

            end

        end

    end

end)

--==========================================================
-- GUI
--==========================================================

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ITALLO_HUB"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = game:GetService("CoreGui")

local function Corner(Object, Radius)

    local C = Instance.new("UICorner")
    C.CornerRadius = UDim.new(0, Radius or 8)
    C.Parent = Object

end

local function MakeDraggable(Frame, Handle)

    local Dragging = false
    local DragStart
    local StartPosition

    Handle.InputBegan:Connect(function(Input)

        if Input.UserInputType ==
            Enum.UserInputType.MouseButton1
            or
            Input.UserInputType ==
            Enum.UserInputType.Touch then

            Dragging = true
            DragStart = Input.Position
            StartPosition = Frame.Position

            Input.Changed:Connect(function()

                if Input.UserInputState ==
                    Enum.UserInputState.End then

                    Dragging = false

                end

            end)

        end

    end)

    UserInputService.InputChanged:Connect(function(Input)

        if not Dragging then
            return
        end

        if Input.UserInputType ==
            Enum.UserInputType.MouseMovement
            or
            Input.UserInputType ==
            Enum.UserInputType.Touch then

            local Delta =
                Input.Position - DragStart

            Frame.Position =
                UDim2.new(
                    StartPosition.X.Scale,
                    StartPosition.X.Offset + Delta.X,
                    StartPosition.Y.Scale,
                    StartPosition.Y.Offset + Delta.Y
                )

        end

    end)

end

--==========================================================
-- LOGIN
--==========================================================

local KeyFrame = Instance.new("Frame")

KeyFrame.Size =
    UDim2.new(0,350,0,195)

KeyFrame.Position =
    UDim2.new(0.5,-175,0.5,-97)

KeyFrame.BackgroundColor3 =
    Color3.fromRGB(13,13,18)

KeyFrame.BorderSizePixel = 0
KeyFrame.Parent = ScreenGui

Corner(KeyFrame,14)

local KeyStroke = Instance.new("UIStroke")
KeyStroke.Thickness = 2
KeyStroke.Parent = KeyFrame

AddRGB(KeyStroke)

local KeyTitle = Instance.new("TextLabel")

KeyTitle.Size =
    UDim2.new(1,-20,0,42)

KeyTitle.Position =
    UDim2.new(0,10,0,5)

KeyTitle.BackgroundTransparency = 1
KeyTitle.Text = "🔥 ITALLO HUB"
KeyTitle.TextColor3 = Color3.new(1,1,1)
KeyTitle.TextSize = 24
KeyTitle.Font = Enum.Font.GothamBold
KeyTitle.Parent = KeyFrame

local KeyInfo = Instance.new("TextLabel")

KeyInfo.Size =
    UDim2.new(1,-20,0,25)

KeyInfo.Position =
    UDim2.new(0,10,0,45)

KeyInfo.BackgroundTransparency = 1
KeyInfo.Text = "Digite sua Key"
KeyInfo.TextColor3 =
    Color3.fromRGB(180,180,190)

KeyInfo.TextSize = 13
KeyInfo.Font = Enum.Font.Gotham
KeyInfo.Parent = KeyFrame

local KeyBox = Instance.new("TextBox")

KeyBox.Size =
    UDim2.new(1,-30,0,40)

KeyBox.Position =
    UDim2.new(0,15,0,75)

KeyBox.BackgroundColor3 =
    Color3.fromRGB(25,25,32)

KeyBox.PlaceholderText = "Key..."
KeyBox.Text = ""
KeyBox.TextColor3 = Color3.new(1,1,1)
KeyBox.TextSize = 14
KeyBox.Font = Enum.Font.Gotham
KeyBox.Parent = KeyFrame

Corner(KeyBox,8)

local Enter = Instance.new("TextButton")

Enter.Size =
    UDim2.new(0.46,0,0,38)

Enter.Position =
    UDim2.new(0.03,0,0,140)

Enter.BackgroundColor3 =
    Color3.fromRGB(30,30,38)

Enter.Text = "ENTRAR"
Enter.TextColor3 = Color3.new(1,1,1)
Enter.TextSize = 13
Enter.Font = Enum.Font.GothamBold
Enter.Parent = KeyFrame

Corner(Enter,8)

local Generate = Instance.new("TextButton")

Generate.Size =
    UDim2.new(0.46,0,0,38)

Generate.Position =
    UDim2.new(0.51,0,0,140)

Generate.BackgroundColor3 =
    Color3.fromRGB(30,30,38)

Generate.Text = "GERAR KEY"
Generate.TextColor3 = Color3.new(1,1,1)
Generate.TextSize = 13
Generate.Font = Enum.Font.GothamBold
Generate.Parent = KeyFrame

Corner(Generate,8)

MakeDraggable(KeyFrame,KeyTitle)

--==========================================================
-- MAIN
--==========================================================

local Main = Instance.new("Frame")

Main.Size =
    UDim2.new(0,440,0,330)

Main.Position =
    UDim2.new(0.5,-220,0.5,-165)

Main.BackgroundColor3 =
    Color3.fromRGB(11,11,16)

Main.BorderSizePixel = 0
Main.Visible = false
Main.Parent = ScreenGui

Corner(Main,14)

local MainStroke = Instance.new("UIStroke")
MainStroke.Thickness = 2
MainStroke.Parent = Main

AddRGB(MainStroke)

local Header = Instance.new("Frame")

Header.Size =
    UDim2.new(1,0,0,52)

Header.BackgroundColor3 =
    Color3.fromRGB(18,18,25)

Header.BorderSizePixel = 0
Header.Parent = Main

Corner(Header,14)

local Title = Instance.new("TextLabel")

Title.Size =
    UDim2.new(1,-100,1,0)

Title.Position =
    UDim2.new(0,15,0,0)

Title.BackgroundTransparency = 1
Title.Text = "🔥 ITALLO HUB | MM2"
Title.TextColor3 = Color3.new(1,1,1)
Title.TextSize = 18
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment =
    Enum.TextXAlignment.Left

Title.Parent = Header

local Minimize = Instance.new("TextButton")

Minimize.Size =
    UDim2.new(0,42,0,36)

Minimize.Position =
    UDim2.new(1,-48,0,8)

Minimize.BackgroundColor3 =
    Color3.fromRGB(30,30,38)

Minimize.Text = "—"
Minimize.TextColor3 = Color3.new(1,1,1)
Minimize.TextSize = 22
Minimize.Font = Enum.Font.GothamBold
Minimize.Parent = Header

Corner(Minimize,8)

MakeDraggable(Main,Header)

--==========================================================
-- SIDEBAR
--==========================================================

local Sidebar = Instance.new("Frame")

Sidebar.Size =
    UDim2.new(0,105,1,-62)

Sidebar.Position =
    UDim2.new(0,8,0,58)

Sidebar.BackgroundColor3 =
    Color3.fromRGB(18,18,24)

Sidebar.BorderSizePixel = 0
Sidebar.Parent = Main

Corner(Sidebar,10)

local SideLayout =
    Instance.new("UIListLayout")

SideLayout.Padding =
    UDim.new(0,5)

SideLayout.HorizontalAlignment =
    Enum.HorizontalAlignment.Center

SideLayout.Parent = Sidebar

local Content = Instance.new("Frame")

Content.Size =
    UDim2.new(1,-125,1,-62)

Content.Position =
    UDim2.new(0,117,0,58)

Content.BackgroundColor3 =
    Color3.fromRGB(18,18,24)

Content.BorderSizePixel = 0
Content.Parent = Main

Corner(Content,10)

--==========================================================
-- TABS
--==========================================================

local Tabs = {}

local function CreateTab(Name)

    local Button = Instance.new("TextButton")

    Button.Size =
        UDim2.new(1,-10,0,35)

    Button.BackgroundColor3 =
        Color3.fromRGB(28,28,36)

    Button.Text = Name
    Button.TextColor3 =
        Color3.fromRGB(220,220,225)

    Button.TextSize = 12
    Button.Font = Enum.Font.GothamBold
    Button.Parent = Sidebar

    Corner(Button,7)

    local Page = Instance.new("ScrollingFrame")

    Page.Size =
        UDim2.new(1,-16,1,-16)

    Page.Position =
        UDim2.new(0,8,0,8)

    Page.BackgroundTransparency = 1
    Page.BorderSizePixel = 0
    Page.ScrollBarThickness = 3
    Page.Visible = false
    Page.Parent = Content

    local Layout =
        Instance.new("UIListLayout")

    Layout.Padding =
        UDim.new(0,7)

    Layout.Parent = Page

    Layout:GetPropertyChangedSignal(
        "AbsoluteContentSize"
    ):Connect(function()

        Page.CanvasSize =
            UDim2.new(
                0,
                0,
                0,
                Layout.AbsoluteContentSize.Y + 10
            )

    end)

    Button.MouseButton1Click:Connect(function()

        for _,Data in pairs(Tabs) do
            Data.Page.Visible = false
        end

        Page.Visible = true

    end)

    Tabs[Name] = {
        Button = Button,
        Page = Page
    }

    return Page
end

local ESPPage =
    CreateTab("ESP")

local FarmPage =
    CreateTab("FARM")

local MovePage =
    CreateTab("MOVE")

local TPPage =
    CreateTab("TP")

local StatusPage =
    CreateTab("STATUS")

--==========================================================
-- CONTROLS
--==========================================================

local function CreateToggle(Page,Text,Default,Callback)

    local Button = Instance.new("TextButton")

    Button.Size =
        UDim2.new(1,-4,0,38)

    Button.BackgroundColor3 =
        Color3.fromRGB(27,27,35)

    Button.TextColor3 =
        Color3.new(1,1,1)

    Button.TextSize = 13
    Button.Font = Enum.Font.GothamBold
    Button.Parent = Page

    Corner(Button,8)

    local Enabled = Default

    local function Update()

        Button.Text =
            Text ..
            " : " ..
            (
                Enabled
                and "ON"
                or "OFF"
            )

    end

    Button.MouseButton1Click:Connect(function()

        Enabled = not Enabled

        Update()

        Callback(Enabled)

    end)

    Update()

    Callback(Default)

    return Button
end

local function CreateBox(Page,Placeholder,Default,Callback)

    local Box = Instance.new("TextBox")

    Box.Size =
        UDim2.new(1,-4,0,38)

    Box.BackgroundColor3 =
        Color3.fromRGB(27,27,35)

    Box.PlaceholderText = Placeholder
    Box.Text = tostring(Default)

    Box.TextColor3 =
        Color3.new(1,1,1)

    Box.TextSize = 13
    Box.Font = Enum.Font.Gotham
    Box.Parent = Page

    Corner(Box,8)

    Box.FocusLost:Connect(function()

        local Value =
            tonumber(Box.Text)

        if Value then
            Callback(Value)
        end

    end)

    return Box
end

local function CreateLabel(Page,Text)

    local Label = Instance.new("TextLabel")

    Label.Size =
        UDim2.new(1,-4,0,42)

    Label.BackgroundTransparency = 1
    Label.Text = Text

    Label.TextColor3 =
        Color3.fromRGB(200,200,205)

    Label.TextSize = 13
    Label.Font = Enum.Font.GothamBold
    Label.TextWrapped = true
    Label.Parent = Page

    return Label
end

--==========================================================
-- ESP
--==========================================================

CreateToggle(
    ESPPage,
    "👁️ ESP GERAL",
    false,
    function(Value)
        CONFIG.ESP = Value
    end
)

CreateToggle(
    ESPPage,
    "🔴 ESP ASSASSINO",
    true,
    function(Value)
        CONFIG.MurdererESP = Value
    end
)

CreateToggle(
    ESPPage,
    "🔵 ESP SHERIFF",
    true,
    function(Value)
        CONFIG.SheriffESP = Value
    end
)

CreateToggle(
    ESPPage,
    "🏷️ ESP NOMES",
    false,
    function(Value)
        CONFIG.NameESP = Value
    end
)

CreateLabel(
    ESPPage,
    "🔴 Assassino = vermelho\n🔵 Sheriff = azul\n🟢 Outros = verde"
)

--==========================================================
-- ROLE DETECTION
--==========================================================

local function GetRole(Player)

    local Role =
        Player:GetAttribute("Role")

    if typeof(Role) == "string" then
        return Role
    end

    local RoleValue =
        Player:FindFirstChild("Role")

    if RoleValue
        and RoleValue:IsA("StringValue") then

        return RoleValue.Value

    end

    local Char =
        Player.Character

    if Char then

        if Char:FindFirstChild("Knife")
            or Char:FindFirstChild("DefaultKnife") then

            return "Murderer"

        end

        if Char:FindFirstChild("Gun")
            or Char:FindFirstChild("Revolver") then

            return "Sheriff"

        end

    end

    return "Unknown"
end

--==========================================================
-- ESP SYSTEM
--==========================================================

local ESPObjects = {}

local function RemoveESP(Player)

    local Data =
        ESPObjects[Player]

    if not Data then
        return
    end

    if Data.Highlight then
        Data.Highlight:Destroy()
    end

    if Data.Billboard then
        Data.Billboard:Destroy()
    end

    ESPObjects[Player] = nil
end

local function CreateESP(Player)

    if Player == LocalPlayer then
        return
    end

    RemoveESP(Player)

    local Highlight =
        Instance.new("Highlight")

    Highlight.Name =
        "ITALLO_ROLE_ESP"

    Highlight.FillTransparency = 0.45
    Highlight.OutlineTransparency = 0
    Highlight.DepthMode =
        Enum.HighlightDepthMode.AlwaysOnTop

    Highlight.Enabled = false
    Highlight.Parent =
        game:GetService("CoreGui")

    local Billboard =
        Instance.new("BillboardGui")

    Billboard.Name =
        "ITALLO_NAME_ESP"

    Billboard.Size =
        UDim2.new(0,180,0,45)

    Billboard.StudsOffset =
        Vector3.new(0,3,0)

    Billboard.AlwaysOnTop = true
    Billboard.Enabled = false
    Billboard.Parent =
        game:GetService("CoreGui")

    local Label =
        Instance.new("TextLabel")

    Label.Size =
        UDim2.new(1,0,1,0)

    Label.BackgroundTransparency = 1
    Label.TextStrokeTransparency = 0
    Label.TextSize = 13
    Label.Font = Enum.Font.GothamBold
    Label.Parent = Billboard

    ESPObjects[Player] = {
        Highlight = Highlight,
        Billboard = Billboard,
        Label = Label
    }

    task.spawn(function()

        while ESPObjects[Player] do

            task.wait(0.1)

            if not Player.Parent then
                break
            end

            local Data =
                ESPObjects[Player]

            local Char =
                Player.Character

            if not Char then

                Highlight.Enabled = false
                Billboard.Enabled = false

                continue
            end

            local HRP =
                Char:FindFirstChild(
                    "HumanoidRootPart"
                )

            if not HRP then
                continue
            end

            local Role =
                GetRole(Player)

            local RoleColor =
                Color3.fromRGB(
                    0,
                    255,
                    80
                )

            local RoleText =
                Player.Name

            local RoleVisible = false

            if Role == "Murderer" then

                RoleColor =
                    Color3.fromRGB(
                        255,
                        0,
                        0
                    )

                RoleText =
                    "🔴 ASSASSINO\n" ..
                    Player.Name

                RoleVisible =
                    CONFIG.MurdererESP

            elseif Role == "Sheriff" then

                RoleColor =
                    Color3.fromRGB(
                        0,
                        110,
                        255
                    )

                RoleText =
                    "🔵 SHERIFF\n" ..
                    Player.Name

                RoleVisible =
                    CONFIG.SheriffESP

            else

                RoleColor =
                    Color3.fromRGB(
                        0,
                        255,
                        80
                    )

                RoleText =
                    "🟢 " ..
                    Player.Name

                RoleVisible = true

            end

            Highlight.Adornee =
                Char

            Highlight.FillColor =
                RoleColor

            Highlight.OutlineColor =
                Color3.new(1,1,1)

            Label.TextColor3 =
                RoleColor

            Label.Text =
                RoleText

            Billboard.Adornee =
                HRP

            -- ESP visual
            Highlight.Enabled =
                CONFIG.ESP
                and RoleVisible

            -- NOMES independente do Highlight
            Billboard.Enabled =
                CONFIG.NameESP

        end

        RemoveESP(Player)

    end)
end

for _,Player in ipairs(
    Players:GetPlayers()
) do

    CreateESP(Player)

end

Players.PlayerAdded:Connect(function(Player)

    task.wait(0.5)

    CreateESP(Player)

end)

Players.PlayerRemoving:Connect(function(Player)

    RemoveESP(Player)

end)

--==========================================================
-- AUTO FARM
--==========================================================

local FarmStatus =
    CreateLabel(
        FarmPage,
        "🪙 Auto Farm desligado"
    )

CreateToggle(
    FarmPage,
    "🪙 AUTO FARM",
    false,
    function(Value)

        CONFIG.CoinFarm = Value

    end
)

CreateBox(
    FarmPage,
    "Intervalo entre moedas (0.03-2)",
    0.15,
    function(Value)

        CONFIG.CoinFarmSpeed =
            math.clamp(
                Value,
                0.03,
                2
            )

    end
)

CreateBox(
    FarmPage,
    "Velocidade do Auto Farm (1-100)",
    5,
    function(Value)

        CONFIG.CoinFarmWalkSpeed =
            math.clamp(
                Value,
                1,
                100
            )

    end
)

CreateLabel(
    FarmPage,
    "🛡️ Anti-queda ativo.\nMoedas fora do mapa são ignoradas."
)

--==========================================================
-- COIN POSITION
--==========================================================

local function GetCoinPosition(Object)

    if not Object
        or not Object.Parent then

        return nil

    end

    if Object:IsA("BasePart") then
        return Object.Position
    end

    if Object:IsA("Model") then

        local Part =
            Object.PrimaryPart
            or Object:FindFirstChildWhichIsA(
                "BasePart",
                true
            )

        if Part then
            return Part.Position
        end

    end

    return nil
end

--==========================================================
-- GROUND CHECK
--==========================================================

local function HasGround(Position)

    local Params =
        RaycastParams.new()

    Params.FilterType =
        Enum.RaycastFilterType.Exclude

    Params.FilterDescendantsInstances = {
        Character
    }

    local Origin =
        Position +
        Vector3.new(0,4,0)

    local Direction =
        Vector3.new(0,-15,0)

    local Result =
        workspace:Raycast(
            Origin,
            Direction,
            Params
        )

    return Result ~= nil
end

local function IsSafePosition(Position)

    if not Position then
        return false
    end

    -- Nunca vai para posições muito abaixo
    if Position.Y < -10 then
        return false
    end

    if not HasGround(Position) then
        return false
    end

    return true
end

--==========================================================
-- FIND COINS
--==========================================================

local function FindCoins()

    local Coins = {}

    for _,Object in ipairs(
        workspace:GetDescendants()
    ) do

        local Name =
            string.lower(
                Object.Name
            )

        if
            (
                Object:IsA("BasePart")
                or
                Object:IsA("Model")
            )
            and
            (
                string.find(Name,"coin")
                or
                string.find(Name,"money")
                or
                string.find(Name,"token")
            )
        then

            local Position =
                GetCoinPosition(Object)

            if Position
                and IsSafePosition(Position) then

                table.insert(
                    Coins,
                    {
                        Object = Object,
                        Position = Position
                    }
                )

            end

        end

    end

    return Coins
end

--==========================================================
-- SAFE POSITION / RECOVERY
--==========================================================

local function SaveSafePosition()

    if not Root then
        return
    end

    if
        Root.Position.Y > 0
        and HasGround(Root.Position)
    then

        LastSafeCFrame =
            Root.CFrame

    end
end

local function RecoverFromFall()

    if not Root then
        return
    end

    if Root.Position.Y < -8 then

        if LastSafeCFrame then

            Root.CFrame =
                LastSafeCFrame

        else

            Root.CFrame =
                CFrame.new(
                    0,
                    10,
                    0
                )

        end

        task.wait(0.2)

    end
end

--==========================================================
-- SAFE MOVEMENT
--==========================================================

local function MoveSafelyTo(Position)

    if
        not Humanoid
        or not Root
        or not Position
    then

        return false
    end

    if not IsSafePosition(Position) then
        return false
    end

    CoinFlightTarget = Position

    local Start =
        Root.Position

    Humanoid:MoveTo(Position)

    local StartTime =
        os.clock()

    while CONFIG.CoinFarm do

        task.wait(0.04)

        if not Root or not Humanoid then
            CoinFlightTarget = nil
            return false
        end

        RecoverFromFall()

        if Root.Position.Y < -8 then
            CoinFlightTarget = nil
            return false
        end

        local Distance =
            (
                Root.Position -
                Position
            ).Magnitude

        if Distance <= 4.5 then

            CoinFlightTarget = nil
            SaveSafePosition()

            return true

        end

        -- Não fica preso indefinidamente
        if
            os.clock() -
            StartTime > 10
        then

            CoinFlightTarget = nil
            return false

        end

        -- Detecta travamento
        if
            (
                Root.Position -
                Start
            ).Magnitude < 1
            and
            os.clock() -
            StartTime > 1
        then

            CoinFlightTarget = nil
            return false

        end

    end

    CoinFlightTarget = nil
    return false
end

--==========================================================
-- AUTO FARM LOOP
--==========================================================

task.spawn(function()

    while task.wait(0.1) do

        if not CONFIG.CoinFarm then
            continue
        end

        if not Root
            or not Humanoid then

            pcall(UpdateCharacter)

            continue
        end

        RecoverFromFall()

        SaveSafePosition()

        local Coins =
            FindCoins()

        if #Coins == 0 then

            FarmStatus.Text =
                "🪙 Procurando moedas..."

            task.wait(0.5)

            continue
        end

        local Collected = 0

        for Index,Data in ipairs(Coins) do

            if not CONFIG.CoinFarm then
                break
            end

            local Coin =
                Data.Object

            if Coin
                and Coin.Parent then

                -- Atualiza novamente a posição
                local Position =
                    GetCoinPosition(Coin)

                if
                    Position
                    and
                    IsSafePosition(Position)
                then

                    FarmStatus.Text =
                        "🪙 Coletando " ..
                        Index ..
                        "/" ..
                        #Coins

                    if MoveSafelyTo(Position) then
                        Collected += 1
                    end

                    task.wait(
                        CONFIG.CoinFarmSpeed
                    )

                end

            end

        end

        FarmStatus.Text =
            "✅ Ciclo: " ..
            Collected ..
            "/" ..
            #Coins

        task.wait(0.25)

    end

end)

--==========================================================
-- MOVEMENT
--==========================================================

CreateToggle(
    MovePage,
    "🚫 Noclip",
    false,
    function(Value)

        CONFIG.Noclip = Value

    end
)

CreateToggle(
    MovePage,
    "🏃 Speed",
    false,
    function(Value)

        CONFIG.Speed = Value

    end
)

CreateBox(
    MovePage,
    "Velocidade",
    24,
    function(Value)

        CONFIG.SpeedValue = Value

    end
)

CreateToggle(
    MovePage,
    "🦘 Jump",
    false,
    function(Value)

        CONFIG.Jump = Value

    end
)

CreateBox(
    MovePage,
    "Jump Power",
    60,
    function(Value)

        CONFIG.JumpValue = Value

    end
)

CreateToggle(
    MovePage,
    "🪽 Fly",
    false,
    function(Value)

        CONFIG.Fly = Value

    end
)

--==========================================================
-- NOCLIP
--==========================================================

RunService.Stepped:Connect(function()

    if
        not (CONFIG.Noclip or CONFIG.CoinFarm)
        or not Character
    then

        return

    end

    for _,Part in ipairs(
        Character:GetDescendants()
    ) do

        if Part:IsA("BasePart") then
            Part.CanCollide = false
        end

    end

end)

--==========================================================
-- SPEED / JUMP
--==========================================================

RunService.Heartbeat:Connect(function()

    if not Humanoid then
        return
    end

    -- Durante o Auto Farm, o personagem voa; não corre pelo chão.
    if CONFIG.CoinFarm then

        Humanoid.WalkSpeed = 0

    elseif CONFIG.Speed then

        Humanoid.WalkSpeed =
            CONFIG.SpeedValue

    else

        Humanoid.WalkSpeed = 16

    end

    if CONFIG.Jump then

        Humanoid.JumpPower =
            CONFIG.JumpValue

    else

        Humanoid.JumpPower = 50

    end

end)

--==========================================================
-- FLY
--==========================================================

local FlyVelocity

RunService.RenderStepped:Connect(function()

    if not Root
        or not Humanoid then

        return
    end

    local ShouldFly =
        CONFIG.Fly
        or CONFIG.CoinFarm
        or CoinFlightTarget ~= nil

    if ShouldFly then

        if not FlyVelocity then

            FlyVelocity =
                Instance.new(
                    "BodyVelocity"
                )

            FlyVelocity.MaxForce =
                Vector3.new(
                    math.huge,
                    math.huge,
                    math.huge
                )

            FlyVelocity.Velocity =
                Vector3.zero

            FlyVelocity.Parent =
                Root

        end

        if CoinFlightTarget then

            local Offset =
                CoinFlightTarget - Root.Position

            local Distance =
                Offset.Magnitude

            if Distance > 0.5 then

                local Speed =
                    math.clamp(
                        CONFIG.CoinFarmWalkSpeed * 2,
                        8,
                        30
                    )

                FlyVelocity.Velocity =
                    Offset.Unit * Speed

            else

                FlyVelocity.Velocity =
                    Vector3.zero

            end

        else

            FlyVelocity.Velocity =
                Humanoid.MoveDirection * 60

        end

    elseif FlyVelocity then

        FlyVelocity:Destroy()

        FlyVelocity = nil

    end

end)

--==========================================================
-- TP LIST
--==========================================================

CreateLabel(
    TPPage,
    "👆 Toque no jogador para teleportar"
)

local PlayerList =
    Instance.new("Frame")

PlayerList.Size =
    UDim2.new(1,-4,0,220)

PlayerList.BackgroundTransparency = 1
PlayerList.Parent = TPPage

local PlayerLayout =
    Instance.new("UIListLayout")

PlayerLayout.Padding =
    UDim.new(0,5)

PlayerLayout.Parent =
    PlayerList

local function TeleportToPlayer(Target)

    if
        not Target
        or Target == LocalPlayer
    then

        return
    end

    local TargetCharacter =
        Target.Character

    if not TargetCharacter then
        return
    end

    local TargetRoot =
        TargetCharacter:FindFirstChild(
            "HumanoidRootPart"
        )

    if not TargetRoot then
        return
    end

    if not Root then
        pcall(UpdateCharacter)
    end

    if Root then

        Root.CFrame =
            TargetRoot.CFrame *
            CFrame.new(0,0,3)

    end
end

local function RefreshPlayerList()

    for _,Object in ipairs(
        PlayerList:GetChildren()
    ) do

        if Object:IsA("TextButton") then
            Object:Destroy()
        end

    end

    for _,Player in ipairs(
        Players:GetPlayers()
    ) do

        if Player ~= LocalPlayer then

            local Button =
                Instance.new("TextButton")

            Button.Size =
                UDim2.new(1,0,0,38)

            Button.BackgroundColor3 =
                Color3.fromRGB(
                    27,27,35
                )

            Button.Text =
                "👤 " ..
                Player.DisplayName ..
                "  @" ..
                Player.Name

            Button.TextColor3 =
                Color3.new(1,1,1)

            Button.TextSize = 12
            Button.Font =
                Enum.Font.GothamBold

            Button.Parent =
                PlayerList

            Corner(Button,8)

            Button.MouseButton1Click:Connect(
                function()

                    TeleportToPlayer(
                        Player
                    )

                    Button.Text =
                        "✅ TP → " ..
                        Player.Name

                    task.delay(
                        1,
                        function()

                            if Button
                                and Button.Parent then

                                Button.Text =
                                    "👤 " ..
                                    Player.DisplayName ..
                                    "  @" ..
                                    Player.Name

                            end

                        end
                    )

                end
            )

        end

    end

end

RefreshPlayerList()

Players.PlayerAdded:Connect(function()

    task.wait(0.5)

    RefreshPlayerList()

end)

Players.PlayerRemoving:Connect(function()

    task.wait(0.1)

    RefreshPlayerList()

end)

--==========================================================
-- STATUS
--==========================================================

local FPSLabel =
    CreateLabel(
        StatusPage,
        "FPS: ..."
    )

local PingLabel =
    CreateLabel(
        StatusPage,
        "Ping: ..."
    )

local PlayersLabel =
    CreateLabel(
        StatusPage,
        "Players: ..."
    )

local RoleLabel =
    CreateLabel(
        StatusPage,
        "Sua Role: ..."
    )

local Frames = 0
local LastFPS = os.clock()

RunService.RenderStepped:Connect(function()

    Frames += 1

    if
        os.clock() -
        LastFPS >= 1
    then

        FPSLabel.Text =
            "FPS: " ..
            Frames

        Frames = 0

        LastFPS =
            os.clock()

    end

end)

task.spawn(function()

    while task.wait(1) do

        local Ping = "?"

        pcall(function()

            Ping =
                math.floor(
                    Stats.Network
                    .ServerStatsItem[
                        "Data Ping"
                    ]:GetValue()
                )

        end)

        PingLabel.Text =
            "Ping: " ..
            Ping ..
            " ms"

        PlayersLabel.Text =
            "Players: " ..
            #Players:GetPlayers()

        RoleLabel.Text =
            "Sua Role: " ..
            GetRole(LocalPlayer)

    end

end)

--==========================================================
-- MINIMIZE
--==========================================================

local Minimized = false

Minimize.MouseButton1Click:Connect(function()

    Minimized =
        not Minimized

    if Minimized then

        Sidebar.Visible = false
        Content.Visible = false

        Main.Size =
            UDim2.new(
                0,
                440,
                0,
                52
            )

        Minimize.Text = "+"

    else

        Main.Size =
            UDim2.new(
                0,
                440,
                0,
                330
            )

        Sidebar.Visible = true
        Content.Visible = true

        Minimize.Text = "—"

    end

end)

--==========================================================
-- LOGIN
--==========================================================

Generate.MouseButton1Click:Connect(function()

    KeyBox.Text =
        CONFIG.Key

    KeyInfo.Text =
        "✅ Key preenchida!"

end)

Enter.MouseButton1Click:Connect(function()

    if KeyBox.Text ==
        CONFIG.Key then

        KeyInfo.Text =
            "✅ Login realizado!"

        task.wait(0.3)

        KeyFrame.Visible = false
        Main.Visible = true

        Tabs["ESP"].Page.Visible = true

    else

        KeyInfo.Text =
            "❌ Key inválida!"

        KeyBox.Text = ""

    end

end)

print("🔥 ITALLO HUB carregado!")
print("🔴 ESP Assassino")
print("🔵 ESP Sheriff")
print("🏷️ ESP Nomes separado")
print("🪙 Auto Farm seguro")
