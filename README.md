local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local MarketplaceService = game:GetService("MarketplaceService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local cam = workspace.CurrentCamera

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CustomMenuGui"
screenGui.IgnoreGuiInset = true
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local function getStartSize()
    if cam and cam.ViewportSize.X < 900 then
        return UDim2.new(0.85, 0, 0.75, 0)
    else
        return UDim2.new(0.42, 0, 0.58, 0)
    end
end

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.Size = getStartSize()
mainFrame.Position = UDim2.new(0.5, 0, -0.6, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
mainFrame.BackgroundTransparency = 0
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.ClipsDescendants = true
mainFrame.Parent = screenGui

local gradient = Instance.new("UIGradient", mainFrame)
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0.0, Color3.fromRGB(132, 0, 255)),
    ColorSequenceKeypoint.new(1.0, Color3.fromRGB(0, 183, 255))
}
gradient.Rotation = 270

local frameCorner = Instance.new("UICorner", mainFrame)
frameCorner.CornerRadius = UDim.new(0, 12)

local frameStroke = Instance.new("UIStroke", mainFrame)
frameStroke.Thickness = 2
frameStroke.Color = Color3.fromRGB(0, 0, 0)
frameStroke.Transparency = 0.5
frameStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local titleBar = Instance.new("Frame")
titleBar.Name = "TitleBar"
titleBar.BackgroundTransparency = 1
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.Position = UDim2.new(0, 0, 0, 0)
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

local logoHolder = Instance.new("Frame")
logoHolder.BackgroundTransparency = 1
logoHolder.Size = UDim2.new(0, 190, 1, 0)
logoHolder.Position = UDim2.new(0, 8, 0, 0)
logoHolder.Parent = titleBar

local logoImage = Instance.new("ImageLabel")
logoImage.BackgroundTransparency = 1
logoImage.Size = UDim2.new(0, 28, 0, 28)
logoImage.Position = UDim2.new(0, 0, 0.5, -14)
logoImage.Image = ""
logoImage.Parent = logoHolder

local logoText = Instance.new("TextLabel")
logoText.BackgroundTransparency = 1
logoText.Size = UDim2.new(1, -34, 1, 0)
logoText.Position = UDim2.new(0, 34, 0, 0)
logoText.Font = Enum.Font.SourceSansBold
logoText.TextColor3 = Color3.fromRGB(255, 255, 255)
logoText.TextXAlignment = Enum.TextXAlignment.Left
logoText.Text = "RA HUB"
logoText.TextSize = 18
logoText.Parent = logoHolder

local btnSize = 24
local btnSpacing = 6

local function createControlButton(name, symbol)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Text = symbol
    btn.TextSize = 18
    btn.Font = Enum.Font.SourceSansBold
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.BackgroundTransparency = 0.3
    btn.BorderSizePixel = 0
    btn.Size = UDim2.new(0, btnSize, 0, btnSize)
    btn.AutoButtonColor = false
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(1, 0)
    local stroke = Instance.new("UIStroke", btn)
    stroke.Thickness = 1
    stroke.Color = Color3.fromRGB(255, 255, 255)
    stroke.Transparency = 0.4
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    btn.Parent = titleBar
    return btn
end

local closeButton = createControlButton("CloseButton", "✕")
local minimizeButton = createControlButton("MinimizeButton", "－")
local maximizeButton = createControlButton("MaximizeButton", "☐")

closeButton.AnchorPoint = Vector2.new(1, 0)
closeButton.Position = UDim2.new(1, -8, 0, 8)
maximizeButton.AnchorPoint = Vector2.new(1, 0)
maximizeButton.Position = UDim2.new(1, -(8 + btnSize + btnSpacing), 0, 8)
minimizeButton.AnchorPoint = Vector2.new(1, 0)
minimizeButton.Position = UDim2.new(1, -(8 + 2*(btnSize + btnSpacing)), 0, 8)

local contentFrame = Instance.new("Frame")
contentFrame.Name = "ContentFrame"
contentFrame.BackgroundTransparency = 1
contentFrame.Size = UDim2.new(1, 0, 1, -40)
contentFrame.Position = UDim2.new(0, 0, 0, 40)
contentFrame.BorderSizePixel = 0
contentFrame.Parent = mainFrame

local padding = Instance.new("UIPadding", contentFrame)
padding.PaddingTop = UDim.new(0, 8)
padding.PaddingBottom = UDim.new(0, 8)
padding.PaddingLeft = UDim.new(0, 8)
padding.PaddingRight = UDim.new(0, 8)

local leftPanel = Instance.new("Frame")
leftPanel.Name = "LeftPanel"
leftPanel.BackgroundTransparency = 0.1
leftPanel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
leftPanel.Size = UDim2.new(0.36, 0, 1, 0)
leftPanel.Position = UDim2.new(0, 0, 0, 0)
leftPanel.Parent = contentFrame
local leftCorner = Instance.new("UICorner", leftPanel)
leftCorner.CornerRadius = UDim.new(0, 10)

local rightPanel = Instance.new("Frame")
rightPanel.Name = "RightPanel"
rightPanel.BackgroundTransparency = 0.2
rightPanel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
rightPanel.Size = UDim2.new(0.62, -2, 1, 0)
rightPanel.Position = UDim2.new(0.38, 2, 0, 0)
rightPanel.Parent = contentFrame
local rightCorner = Instance.new("UICorner", rightPanel)
rightCorner.CornerRadius = UDim.new(0, 10)

local searchBox = Instance.new("TextBox")
searchBox.Name = "SearchBox"
searchBox.Parent = leftPanel
searchBox.Size = UDim2.new(1, -10, 0, 28)
searchBox.Position = UDim2.new(0, 5, 0, 5)
searchBox.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
searchBox.TextColor3 = Color3.fromRGB(255, 255, 255)
searchBox.PlaceholderColor3 = Color3.fromRGB(200, 200, 200)
searchBox.PlaceholderText = "поиск / search..."
searchBox.Font = Enum.Font.SourceSans
searchBox.TextSize = 14
searchBox.ClearTextOnFocus = false
local sbCorner = Instance.new("UICorner", searchBox)
sbCorner.CornerRadius = UDim.new(0, 6)

local tabsScroll = Instance.new("ScrollingFrame")
tabsScroll.Name = "TabsScroll"
tabsScroll.Parent = leftPanel
tabsScroll.BackgroundTransparency = 1
tabsScroll.Position = UDim2.new(0, 5, 0, 38)
tabsScroll.Size = UDim2.new(1, -10, 1, -43)
tabsScroll.ScrollBarThickness = 3
tabsScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
local tabsLayout = Instance.new("UIListLayout", tabsScroll)
tabsLayout.FillDirection = Enum.FillDirection.Vertical
tabsLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
tabsLayout.SortOrder = Enum.SortOrder.LayoutOrder
tabsLayout.Padding = UDim.new(0, 6)

local tabTitle = Instance.new("TextLabel")
tabTitle.BackgroundTransparency = 1
tabTitle.Size = UDim2.new(1, -10, 0, 28)
tabTitle.Position = UDim2.new(0, 5, 0, 0)
tabTitle.Font = Enum.Font.SourceSansBold
tabTitle.TextSize = 18
tabTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
tabTitle.TextXAlignment = Enum.TextXAlignment.Left
tabTitle.Text = ""
tabTitle.Parent = rightPanel

local buttonsScroll = Instance.new("ScrollingFrame")
buttonsScroll.Name = "ButtonsScroll"
buttonsScroll.BackgroundTransparency = 1
buttonsScroll.Size = UDim2.new(1, -10, 1, -34)
buttonsScroll.Position = UDim2.new(0, 5, 0, 32)
buttonsScroll.ScrollBarThickness = 4
buttonsScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
buttonsScroll.Parent = rightPanel

local buttonsLayout = Instance.new("UIListLayout", buttonsScroll)
buttonsLayout.FillDirection = Enum.FillDirection.Vertical
buttonsLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left
buttonsLayout.VerticalAlignment = Enum.VerticalAlignment.Top
buttonsLayout.Padding = UDim.new(0, 6)

local toolsFrame = Instance.new("Frame")
toolsFrame.Name = "ToolsFrame"
toolsFrame.Size = UDim2.new(0, 255, 0, 225)
toolsFrame.AnchorPoint = Vector2.new(0.5, 0.5)
toolsFrame.Position = UDim2.new(0.62, 0, 0.5, 0)
toolsFrame.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
toolsFrame.BackgroundTransparency = 0.05
toolsFrame.Visible = false
toolsFrame.Active = true
toolsFrame.ZIndex = 60
toolsFrame.Parent = screenGui

local toolsCorner = Instance.new("UICorner", toolsFrame)
toolsCorner.CornerRadius = UDim.new(0, 12)

local toolsStroke = Instance.new("UIStroke", toolsFrame)
toolsStroke.Thickness = 1
toolsStroke.Color = Color3.fromRGB(255, 255, 255)
toolsStroke.Transparency = 0.4

local toolsGlow = Instance.new("ImageLabel")
toolsGlow.BackgroundTransparency = 1
toolsGlow.Image = "rbxassetid://4996891970"
toolsGlow.ImageColor3 = Color3.fromRGB(130, 0, 255)
toolsGlow.ScaleType = Enum.ScaleType.Slice
toolsGlow.SliceCenter = Rect.new(24, 24, 278, 278)
toolsGlow.Size = UDim2.new(1, 30, 1, 30)
toolsGlow.Position = UDim2.new(0, -15, 0, -15)
toolsGlow.ImageTransparency = 0.35
toolsGlow.ZIndex = 59
toolsGlow.Parent = toolsFrame

local toolsGrad = Instance.new("UIGradient", toolsFrame)
toolsGrad.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(37, 0, 71)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 175, 255))
}
toolsGrad.Rotation = 90
toolsGrad.Transparency = NumberSequence.new{
    NumberSequenceKeypoint.new(0, 0.05),
    NumberSequenceKeypoint.new(1, 0.15)
}

local toolsTitle = Instance.new("Frame")
toolsTitle.BackgroundTransparency = 1
toolsTitle.Size = UDim2.new(1, -16, 0, 38)
toolsTitle.Position = UDim2.new(0, 8, 0, 4)
toolsTitle.ZIndex = 61
toolsTitle.Parent = toolsFrame

local toolsIcon = Instance.new("ImageLabel")
toolsIcon.BackgroundTransparency = 1
toolsIcon.Size = UDim2.new(0, 22, 0, 22)
toolsIcon.Position = UDim2.new(0, 0, 0.5, -11)
toolsIcon.Image = "rbxassetid://7733964716"
toolsIcon.ImageColor3 = Color3.fromRGB(255, 255, 255)
toolsIcon.ZIndex = 61
toolsIcon.Parent = toolsTitle

local toolsLabel = Instance.new("TextLabel")
toolsLabel.BackgroundTransparency = 1
toolsLabel.Size = UDim2.new(1, -72, 1, 0)
toolsLabel.Position = UDim2.new(0, 28, 0, 0)
toolsLabel.Font = Enum.Font.SourceSansBold
toolsLabel.TextSize = 16
toolsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
toolsLabel.TextXAlignment = Enum.TextXAlignment.Left
toolsLabel.Text = "Инструменты"
toolsLabel.ZIndex = 61
toolsLabel.Parent = toolsTitle

local toolsClose = Instance.new("TextButton")
toolsClose.Text = "✕"
toolsClose.Font = Enum.Font.SourceSansBold
toolsClose.TextSize = 16
toolsClose.TextColor3 = Color3.fromRGB(255, 255, 255)
toolsClose.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toolsClose.BackgroundTransparency = 0.05
toolsClose.Size = UDim2.new(0, 26, 0, 26)
toolsClose.Position = UDim2.new(1, -26, 0.5, -13)
toolsClose.AutoButtonColor = false
toolsClose.ZIndex = 62
toolsClose.Parent = toolsTitle
local toolsCloseCorner = Instance.new("UICorner", toolsClose)
toolsCloseCorner.CornerRadius = UDim.new(1, 0)

local toolsBody = Instance.new("Frame")
toolsBody.BackgroundTransparency = 1
toolsBody.Size = UDim2.new(1, -16, 1, -48)
toolsBody.Position = UDim2.new(0, 8, 0, 45)
toolsBody.ZIndex = 61
toolsBody.Parent = toolsFrame

local function createToolButton(txt, orderX, orderY)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(0.5, -6, 0, 42)
    b.Position = UDim2.new((orderX-1)*0.5, (orderX==1) and 0 or 6, 0, (orderY-1)*47)
    b.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    b.BackgroundTransparency = 0.1
    b.Text = txt
    b.TextColor3 = Color3.fromRGB(255, 255, 255)
    b.Font = Enum.Font.SourceSansBold
    b.TextSize = 14
    b.AutoButtonColor = false
    b.ZIndex = 62
    b.Parent = toolsBody
    local c = Instance.new("UICorner", b)
    c.CornerRadius = UDim.new(0, 8)
    local s = Instance.new("UIStroke", b)
    s.Thickness = 1
    s.Color = Color3.fromRGB(255, 255, 255)
    s.Transparency = 0.6
    b.MouseEnter:Connect(function()
        TweenService:Create(b, TweenInfo.new(0.18, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            BackgroundTransparency = 0.02
        }):Play()
    end)
    b.MouseLeave:Connect(function()
        TweenService:Create(b, TweenInfo.new(0.18, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            BackgroundTransparency = 0.1
        }):Play()
    end)
    b.MouseButton1Down:Connect(function()
        TweenService:Create(b, TweenInfo.new(0.1), {Size = UDim2.new(0.5, -6, 0, 39)}):Play()
    end)
    b.MouseButton1Up:Connect(function()
        TweenService:Create(b, TweenInfo.new(0.1), {Size = UDim2.new(0.5, -6, 0, 42)}):Play()
    end)
    return b
end

local toolBtn1 = createToolButton("Fly GUI V3", 1, 1)
local toolBtn2 = createToolButton("Infinite Yield", 2, 1)
local toolBtn3 = createToolButton("Hydrogen DEX", 1, 2)
local toolBtn4 = createToolButton("Телепорт", 2, 2)

toolBtn1.Activated:Connect(function()
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
    end)
end)

toolBtn2.Activated:Connect(function()
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
    end)
end)

toolBtn3.Activated:Connect(function()
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/DannyH3103/scripts/main/Hydrogen_DEXV4"))()
    end)
end)

toolBtn4.Activated:Connect(function()
    pcall(function()
        loadstring(game:HttpGet("https://pastebin.com/raw/h8t8cA0F"))()
    end)
end)

local toolsDragging = false
local toolsDragStart, toolsStartPos, toolsDragInput
local function updateToolsDrag(input)
    local delta = input.Position - toolsDragStart
    toolsFrame.Position = UDim2.new(
        toolsStartPos.X.Scale,
        toolsStartPos.X.Offset + delta.X,
        toolsStartPos.Y.Scale,
        toolsStartPos.Y.Offset + delta.Y
    )
end

toolsTitle.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        toolsDragging = true
        toolsDragStart = input.Position
        toolsStartPos = toolsFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                toolsDragging = false
            end
        end)
    end
end)

toolsTitle.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        toolsDragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if toolsDragging and input == toolsDragInput then
        updateToolsDrag(input)
    end
end)

toolsClose.Activated:Connect(function()
    TweenService:Create(toolsFrame, TweenInfo.new(0.22, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
        BackgroundTransparency = 1,
        Size = UDim2.new(0, 255, 0, 0)
    }):Play()
    task.delay(0.2, function()
        toolsFrame.Visible = false
        toolsFrame.Size = UDim2.new(0, 255, 0, 225)
        toolsFrame.BackgroundTransparency = 0.05
    end)
end)

local function openTools()
    toolsFrame.Visible = true
    toolsFrame.Size = UDim2.new(0, 0, 0, 0)
    toolsFrame.BackgroundTransparency = 1
    TweenService:Create(toolsFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, 255, 0, 225),
        BackgroundTransparency = 0.05
    }):Play()
end

-- база вкладок
local TabsBase = {
    {
        Key = "S",
        PlaceId = 0,
        GameId = 0,
        RuName = "Настройки / Поиск",
        EnName = "Settings / Search",
        Buttons = {
            {RuName = "Найти игру по имени", EnName = "Find game name", Link = "", Code = "__FIND_BY_NAME__"},
            {RuName = "Найти игру по ID", EnName = "Find game ID", Link = "", Code = "__FIND_BY_ID__"},
            {RuName = "Открыть текущее место", EnName = "Open current place", Link = "", Code = "__OPEN_THIS__"},
            {RuName = "Канал Delta", EnName = "Delta Channel", Link = "", Code = "__COPY_DELTA__"},
            {RuName = "Инструменты", EnName = "Tools", Link = "", Code = "__TOOLS__"},
        }
    },

    {
        Key = "BROOKHAVEN",
        PlaceId = 4924922222,
        GameId = 4924922222,
        RuName = "Brookhaven RP",
        EnName = "Brookhaven RP",
        Buttons = {
            {RuName = "Панель", EnName = "Panel", Link = "https://raw.githubusercontent.com/kigredns/testUIDK/refs/heads/main/panel.lua", Code = ""},
            {RuName = "Brookhaven Hub", EnName = "Brookhaven Hub", Link = "https://raw.githubusercontent.com/Daivd977/Deivd999/refs/heads/main/pessal", Code = ""},
            {RuName = "пусто", EnName = "empty", Link = "", Code = ""},
        }
    },

    {
        Key = "BLOXFRUITS",
        PlaceId = 2753915549,
        GameId = 2753915549,
        RuName = "Blox Fruits",
        EnName = "Blox Fruits",
        Buttons = {
            {RuName = "Volcano New", EnName = "Volcano New", Link = "https://raw.githubusercontent.com/wpisstestfprg/Volcano/refs/heads/main/VolcanoNewUpdated.luau", Code = ""},
            {RuName = "BlueX Hub", EnName = "BlueX Hub", Link = "https://raw.githubusercontent.com/Dev-BlueX/BlueX-Hub/refs/heads/main/Main.lua", Code = ""},
            {RuName = "NTT Hub", EnName = "NTT Hub", Link = "https://ntt-hub.xyz/api/repo?id1=main&id2=lua", Code = ""},
        }
    },

    {
        Key = "PLANTSVSBRAINROTS",
        PlaceId = 127742093697776,
        GameId = 8316902627,
        RuName = "Plants Vs Brainrots 🌻",
        EnName = "Plants Vs Brainrots 🌻",
        Buttons = {
            {RuName = "Prostone Hub", EnName = "Prostone Hub", Link = "https://raw.githubusercontent.com/Kernelecho1/prostone-Hub-script/refs/heads/main/plantsVSbrainrots.lua", Code = ""},
            {RuName = "Anonymous Hub", EnName = "Anonymous Hub", Link = "https://raw.githubusercontent.com/HitAMassy/AnonymousHub/refs/heads/main/loader.lua", Code = ""},
            {RuName = "пусто", EnName = "empty", Link = "", Code = ""},
        }
    },

    {
        Key = "NIGHTS99",
        PlaceId = 79546208627805,
        GameId = 7326934954,
        RuName = "99 Nights in the Forest",
        EnName = "99 Nights in the Forest",
        Buttons = {
            {RuName = "Raygull 1.0", EnName = "Raygull 1.0", Link = "https://raw.githubusercontent.com/raygull3d/99-Nights-in-the-Forest-Script/refs/heads/main/99%20Days%20Scirpt%20By%20Raygull%20Beta%201.0.lua", Code = ""},
            {RuName = "CpsHub", EnName = "CpsHub", Link = "https://raw.githubusercontent.com/Rx1m/CpsHub/refs/heads/main/Hub", Code = ""},
            {RuName = "H4x Loader", EnName = "H4x Loader", Link = "https://raw.githubusercontent.com/H4xScripts/Loader/refs/heads/main/loader.lua", Code = ""},
        }
    },

    {
        Key = "MM2",
        PlaceId = 142823291,
        GameId = 142823291,
        RuName = "Murder Mystery 2",
        EnName = "Murder Mystery 2",
        Buttons = {
            {RuName = "Primordial", EnName = "Primordial", Link = "https://raw.githubusercontent.com/BludnyHolandan/MM2/refs/heads/main/primordial/script/amg.lua", Code = ""},
            {RuName = "Croxy Hub", EnName = "Croxy Hub", Link = "https://raw.githubusercontent.com/croxy1337/hub/refs/heads/main/main.lua", Code = ""},
            {RuName = "пусто", EnName = "empty", Link = "", Code = ""},
        }
    },

    {
        Key = "3008",
        PlaceId = 2768379856,
        GameId = 1000233041,
        RuName = "3008 (IKEA)",
        EnName = "3008 (IKEA)",
        Buttons = {
            {RuName = "ZZINS Hub", EnName = "ZZINS Hub", Link = "https://raw.githubusercontent.com/ZZINS077/3008/refs/heads/main/ZZINSHUB", Code = ""},
            {RuName = "SCP3008.py", EnName = "SCP3008.py", Link = "https://raw.githubusercontent.com/Yumiara/Python/refs/heads/main/SCP3008.py", Code = ""},
            {RuName = "пусто", EnName = "empty", Link = "", Code = ""},
        }
    },

    {
        Key = "STEALBRAINROT",
        PlaceId = 109983668079237,
        GameId = 7709344486,
        RuName = "Украдите Brainrot",
        EnName = "Steal a Brainrot",
        Buttons = {
            {RuName = "Vulkan", EnName = "Vulkan", Link = "https://raw.githubusercontent.com/ily123950/Vulkan/refs/heads/main/Tr", Code = ""},
            {RuName = "Ravion", EnName = "Ravion", Link = "https://raw.githubusercontent.com/egor2078f/sab/main/ravion.lua", Code = ""},
            {RuName = "пусто", EnName = "empty", Link = "", Code = ""},
        }
    },

    {
        Key = "BUILDABOAT",
        PlaceId = 537413528,
        GameId = 210851291,
        RuName = "Построй лодку за сокровищем",
        EnName = "Build A Boat For Treasure",
        Buttons = {
            {RuName = "BABFT Script", EnName = "BABFT Script", Link = "https://raw.githubusercontent.com/example-prog/Build-A-Boat-For-Treasure/refs/heads/main/BABFT", Code = ""},
            {RuName = "SyniX Hub", EnName = "SyniX Hub", Link = "https://raw.githubusercontent.com/ZorSonMods/SyniX-HUB-BUILD-BOAT-FOR-TREASURE/main/script", Code = ""},
            {RuName = "Minh Hub", EnName = "Minh Hub", Link = "https://raw.githubusercontent.com/Minh-code0807/Main-Minh-Hub-ROBLOX/refs/heads/main/Minh%20Hub%20Aotu%20farm%20Build%20a%20Boat%20for%20Treasure.lua", Code = ""},
        }
    },

    {
        Key = "FISCH",
        PlaceId = 16732694052,
        GameId = 5750914919,
        RuName = "Fisch (рыбалка)",
        EnName = "Fisch",
        Buttons = {
            {RuName = "Lunor Loader", EnName = "Lunor Loader", Link = "https://lunor.dev/loader", Code = ""},
            {RuName = "Junkie Dev", EnName = "Junkie Dev", Link = "https://api.junkie-development.de/api/v1/luascripts/public/35eabcb2c03e2c3e0767386f9518fb187022b321e69988192299145ea725c819/download", Code = ""},
            {RuName = "пусто", EnName = "empty", Link = "", Code = ""},
        }
    },

    {
        Key = "GROWAGARDEN",
        PlaceId = 126884695634066,
        GameId = 7436755782,
        RuName = "Вырасти сад",
        EnName = "Grow a Garden",
        Buttons = {
            {RuName = "Speed Hub X", EnName = "Speed Hub X", Link = "https://raw.githubusercontent.com/AhmadV99/Speed-Hub-X/main/Speed%20Hub%20X.lua", Code = ""},
            {RuName = "H4x Loader 2", EnName = "H4x Loader 2", Link = "https://raw.githubusercontent.com/H4xScripts/Loader/refs/heads/main/loader2.lua", Code = ""},
            {RuName = "пусто", EnName = "empty", Link = "", Code = ""},
        }
    },
}

local currentLanguage = "ru"
local currentTab = nil
local tabButtons = {}

local function applyButtonAnimations(button, onClickFunction)
    button.MouseEnter:Connect(function()
        TweenService:Create(
            button,
            TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
            {BackgroundTransparency = 0.1}
        ):Play()
    end)
    button.MouseLeave:Connect(function()
        TweenService:Create(
            button,
            TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
            {BackgroundTransparency = 0.3}
        ):Play()
        local scale = button:FindFirstChildWhichIsA("UIScale")
        if scale then
            scale:Destroy()
        end
    end)
    button.MouseButton1Down:Connect(function()
        local scaleObj = Instance.new("UIScale", button)
        scaleObj.Scale = 1.0
        TweenService:Create(
            scaleObj,
            TweenInfo.new(0.1, Enum.EasingStyle.Back, Enum.EasingDirection.In),
            {Scale = 0.95}
        ):Play()
    end)
    button.MouseButton1Up:Connect(function()
        local scaleObj = button:FindFirstChildWhichIsA("UIScale")
        if scaleObj then
            TweenService:Create(
                scaleObj,
                TweenInfo.new(0.1, Enum.EasingStyle.Back, Enum.EasingDirection.Out),
                {Scale = 1.0}
            ):Play()
            task.delay(0.12, function()
                if scaleObj.Parent then
                    scaleObj:Destroy()
                end
            end)
        end
    end)
    button.Activated:Connect(function()
        onClickFunction()
    end)
end

local function clearButtons()
    for _,v in ipairs(buttonsScroll:GetChildren()) do
        if v:IsA("TextButton") then
            v:Destroy()
        end
    end
end

local function refreshButtonsScrollSize()
    task.wait()
    local sz = buttonsLayout.AbsoluteContentSize
    buttonsScroll.CanvasSize = UDim2.new(0, 0, 0, sz.Y + 8)
end

local function openTab(tabData)
    currentTab = tabData
    local nameToShow = currentLanguage == "ru" and (tabData.RuName or tabData.EnName) or (tabData.EnName or tabData.RuName)
    tabTitle.Text = nameToShow or ""
    clearButtons()
    for i,btnInfo in ipairs(tabData.Buttons or {}) do
        local b = Instance.new("TextButton")
        b.Name = "TabButton"..i
        b.Text = currentLanguage == "ru" and (btnInfo.RuName or btnInfo.EnName) or (btnInfo.EnName or btnInfo.RuName)
        b.Font = Enum.Font.SourceSansBold
        b.TextColor3 = Color3.fromRGB(255, 255, 255)
        b.TextScaled = true
        b.Size = UDim2.new(1, 0, 0, 50)
        b.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        b.BackgroundTransparency = 0.3
        b.BorderSizePixel = 0
        b.AutoButtonColor = false
        local corner = Instance.new("UICorner", b)
        corner.CornerRadius = UDim.new(0, 8)
        b.Parent = buttonsScroll
        applyButtonAnimations(b, function()
            if btnInfo.Link and btnInfo.Link ~= "" then
                pcall(function()
                    loadstring(game:HttpGet(btnInfo.Link))()
                end)
            else
                if btnInfo.Code and btnInfo.Code ~= "" then
                    if btnInfo.Code == "__FIND_BY_NAME__" then
                        local ok, data = pcall(function()
                            return MarketplaceService:GetProductInfo(game.PlaceId)
                        end)
                        if ok and data and data.Name then
                            local low = string.lower(data.Name)
                            local found = nil
                            for _,tb in ipairs(TabsBase) do
                                local nm = currentLanguage == "ru" and (tb.RuName or tb.EnName) or (tb.EnName or tb.RuName)
                                if nm and string.find(string.lower(nm), low, 1, true) then
                                    found = tb
                                    break
                                end
                            end
                            if not found then
                                for _,tb in ipairs(TabsBase) do
                                    if (tb.PlaceId and tb.PlaceId == game.PlaceId) or (tb.GameId and tb.GameId == game.GameId) then
                                        found = tb
                                        break
                                    end
                                end
                            end
                            if found then
                                openTab(found)
                            end
                        end
                    elseif btnInfo.Code == "__FIND_BY_ID__" then
                        local found = nil
                        for _,tb in ipairs(TabsBase) do
                            if (tb.PlaceId and tb.PlaceId == game.PlaceId) or (tb.GameId and tb.GameId == game.GameId) then
                                found = tb
                                break
                            end
                        end
                        if found then
                            openTab(found)
                        end
                    elseif btnInfo.Code == "__OPEN_THIS__" then
                        local found = nil
                        for _,tb in ipairs(TabsBase) do
                            if (tb.PlaceId and tb.PlaceId == game.PlaceId) or (tb.GameId and tb.GameId == game.GameId) then
                                found = tb
                                break
                            end
                        end
                        if found then
                            openTab(found)
                        end
                    elseif btnInfo.Code == "__COPY_DELTA__" then
                        pcall(function()
                            if setclipboard then
                                setclipboard("https://t.me/deltascriptt")
                            elseif toclipboard then
                                toclipboard("https://t.me/deltascriptt")
                            end
                        end)
                    elseif btnInfo.Code == "__TOOLS__" then
                        openTools()
                    else
                        pcall(function()
                            loadstring(btnInfo.Code)()
                        end)
                    end
                end
            end
        end)
    end
    refreshButtonsScrollSize()
end

local function rebuildTabs()
    for _,child in ipairs(tabsScroll:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end
    tabButtons = {}
    for i,tab in ipairs(TabsBase) do
        local t = Instance.new("TextButton")
        t.Name = "MenuTab"..i
        t.Text = currentLanguage == "ru" and (tab.RuName or tab.EnName) or (tab.EnName or tab.RuName)
        t.Font = Enum.Font.SourceSansBold
        t.TextColor3 = Color3.fromRGB(255, 255, 255)
        t.TextScaled = true
        t.Size = UDim2.new(1, -4, 0, 26)
        t.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        t.BackgroundTransparency = 0.3
        t.BorderSizePixel = 0
        t.AutoButtonColor = false
        local corner = Instance.new("UICorner", t)
        corner.CornerRadius = UDim.new(0, 6)
        t.Parent = tabsScroll
        applyButtonAnimations(t, function()
            openTab(tab)
        end)
        tabButtons[#tabButtons+1] = t
    end
    task.wait()
    local abs = tabsLayout.AbsoluteContentSize
    tabsScroll.CanvasSize = UDim2.new(0, 0, 0, abs.Y + 10)
end

local function searchAndOpen(text)
    if text == "" then return end
    local low = string.lower(text)
    local found = nil
    for _,tb in ipairs(TabsBase) do
        local nm = currentLanguage == "ru" and (tb.RuName or tb.EnName) or (tb.EnName or tb.RuName)
        if nm and string.find(string.lower(nm), low, 1, true) then
            found = tb
            break
        end
    end
    if not found then
        local id = tonumber(text)
        if id then
            for _,tb in ipairs(TabsBase) do
                if (tb.PlaceId and tb.PlaceId == id) or (tb.GameId and tb.GameId == id) then
                    found = tb
                    break
                end
            end
        end
    end
    if found then
        openTab(found)
    end
end

searchBox.FocusLost:Connect(function(enter)
    if enter then
        searchAndOpen(searchBox.Text)
    end
end)

titleBar.ZIndex = 2
closeButton.ZIndex = 3
maximizeButton.ZIndex = 3
minimizeButton.ZIndex = 3
toolsFrame.ZIndex = 60

local roundButton = Instance.new("TextButton")
roundButton.Name = "RoundToggleButton"
roundButton.Text = ""
roundButton.Size = UDim2.new(0, 50, 0, 50)
roundButton.AnchorPoint = Vector2.new(0.5, 0.5)
roundButton.Position = UDim2.new(0.5, 0, 0.5, 0)
roundButton.BackgroundTransparency = 0
roundButton.Visible = false
roundButton.Parent = screenGui

local roundGradient = Instance.new("UIGradient", roundButton)
roundGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0.0, Color3.fromRGB(132, 0, 255)),
    ColorSequenceKeypoint.new(1.0, Color3.fromRGB(0, 183, 255))
}
roundGradient.Rotation = 270

local roundCorner = Instance.new("UICorner", roundButton)
roundCorner.CornerRadius = UDim.new(1, 0)

local roundStroke = Instance.new("UIStroke", roundButton)
roundStroke.Thickness = 2
roundStroke.Color = Color3.fromRGB(0, 0, 0)
roundStroke.Transparency = 0.5
roundStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local function openMenu()
    mainFrame.Visible = true
    mainFrame.Position = UDim2.new(0.5, 0, -0.6, 0)
    local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quint, Enum.EasingDirection.Out)
    local goal = {Position = UDim2.new(0.5, 0, 0.5, 0)}
    local tween = TweenService:Create(mainFrame, tweenInfo, goal)
    tween:Play()
    mainFrame.BackgroundTransparency = 1
    frameStroke.Transparency = 1
    TweenService:Create(
        mainFrame,
        TweenInfo.new(0.5, Enum.EasingStyle.Linear, Enum.EasingDirection.Out),
        {BackgroundTransparency = 0}
    ):Play()
    TweenService:Create(
        frameStroke,
        TweenInfo.new(0.5, Enum.EasingStyle.Linear, Enum.EasingDirection.Out),
        {Transparency = 0.5}
    ):Play()
end

local function closeMenu()
    if not mainFrame.Visible then return end
    local tweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quint, Enum.EasingDirection.In)
    local goal = {Position = UDim2.new(0.5, 0, -0.5, 0)}
    local tween = TweenService:Create(mainFrame, tweenInfo, goal)
    tween:Play()
    TweenService:Create(
        mainFrame,
        TweenInfo.new(0.3, Enum.EasingStyle.Linear, Enum.EasingDirection.In),
        {BackgroundTransparency = 1}
    ):Play()
    TweenService:Create(
        frameStroke,
        TweenInfo.new(0.3, Enum.EasingStyle.Linear, Enum.EasingDirection.In),
        {Transparency = 1}
    ):Play()
    tween.Completed:Connect(function()
        mainFrame.Visible = false
    end)
end

local function minimizeMenu()
    if not mainFrame.Visible then return end
    local tweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quint, Enum.EasingDirection.In)
    local goal = {Position = UDim2.new(0.5, 0, 1.5, 0)}
    local tween = TweenService:Create(mainFrame, tweenInfo, goal)
    tween:Play()
    TweenService:Create(
        mainFrame,
        TweenInfo.new(0.3, Enum.EasingStyle.Linear, Enum.EasingDirection.In),
        {BackgroundTransparency = 1}
    ):Play()
    TweenService:Create(
        frameStroke,
        TweenInfo.new(0.3, Enum.EasingStyle.Linear, Enum.EasingDirection.In),
        {Transparency = 1}
    ):Play()
    tween.Completed:Connect(function()
        mainFrame.Visible = false
        roundButton.Visible = true
    end)
end

local originalSize = mainFrame.Size
local originalPos = mainFrame.Position
local isMaximized = false

local function toggleMaximize()
    if not isMaximized then
        originalSize = mainFrame.Size
        originalPos = mainFrame.Position
        isMaximized = true
        local targetSize = (cam and cam.ViewportSize.X < 900) and UDim2.new(0.95, 0, 0.9, 0) or UDim2.new(0.95, 0, 0.85, 0)
        local targetPos = UDim2.new(0.5, 0, 0.5, 0)
        local tweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quint, Enum.EasingDirection.Out)
        TweenService:Create(mainFrame, tweenInfo, {
            Size = targetSize,
            Position = targetPos
        }):Play()
    else
        isMaximized = false
        local tweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quint, Enum.EasingDirection.Out)
        TweenService:Create(mainFrame, tweenInfo, {
            Size = originalSize,
            Position = originalPos
        }):Play()
    end
end

closeButton.MouseEnter:Connect(function()
    TweenService:Create(closeButton, TweenInfo.new(0.2), {BackgroundTransparency = 0.1}):Play()
end)
closeButton.MouseLeave:Connect(function()
    TweenService:Create(closeButton, TweenInfo.new(0.2), {BackgroundTransparency = 0.3}):Play()
end)
closeButton.Activated:Connect(function()
    closeMenu()
end)

minimizeButton.MouseEnter:Connect(function()
    TweenService:Create(minimizeButton, TweenInfo.new(0.2), {BackgroundTransparency = 0.1}):Play()
end)
minimizeButton.MouseLeave:Connect(function()
    TweenService:Create(minimizeButton, TweenInfo.new(0.2), {BackgroundTransparency = 0.3}):Play()
end)
minimizeButton.Activated:Connect(function()
    minimizeMenu()
end)

maximizeButton.MouseEnter:Connect(function()
    TweenService:Create(maximizeButton, TweenInfo.new(0.2), {BackgroundTransparency = 0.1}):Play()
end)
maximizeButton.MouseLeave:Connect(function()
    TweenService:Create(maximizeButton, TweenInfo.new(0.2), {BackgroundTransparency = 0.3}):Play()
end)
maximizeButton.Activated:Connect(function()
    toggleMaximize()
end)

local dragging = false
local dragStart, startPos
local dragInput

local function updateDrag(input)
    local delta = input.Position - dragStart
    local newPos = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,
        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
    mainFrame.Position = newPos
end

titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 
       or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement 
       or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        updateDrag(input)
    end
end)

local roundDragging = false
local roundHasMoved = false
local roundDragStart, roundStartPos
local roundDragInput
local MOVE_ROUND_THRESHOLD = 5

local function updateRoundDrag(input)
    local delta = input.Position - roundDragStart
    if delta.Magnitude > MOVE_ROUND_THRESHOLD then
        roundHasMoved = true
    end
    roundButton.Position = UDim2.new(
        roundStartPos.X.Scale, roundStartPos.X.Offset + delta.X,
        roundStartPos.Y.Scale, roundStartPos.Y.Offset + delta.Y
    )
end

roundButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 
       or input.UserInputType == Enum.UserInputType.Touch then
        roundDragging = true
        roundHasMoved = false
        roundDragStart = input.Position
        roundStartPos = roundButton.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                roundDragging = false
            end
        end)
    end
end)

roundButton.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement 
       or input.UserInputType == Enum.UserInputType.Touch then
        roundDragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if roundDragging and input == roundDragInput then
        updateRoundDrag(input)
    end
end)

roundButton.Activated:Connect(function()
    if not roundHasMoved then
        roundButton.Visible = false
        openMenu()
    end
end)

local langFrame = Instance.new("Frame")
langFrame.Size = UDim2.new(0, 250, 0, 130)
langFrame.AnchorPoint = Vector2.new(0.5, 0.5)
langFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
langFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
langFrame.BackgroundTransparency = 0.2
langFrame.Parent = screenGui
local langCorner = Instance.new("UICorner", langFrame)
langCorner.CornerRadius = UDim.new(0, 10)

local langTitle = Instance.new("TextLabel")
langTitle.BackgroundTransparency = 1
langTitle.Size = UDim2.new(1, 0, 0, 28)
langTitle.Position = UDim2.new(0, 0, 0, 6)
langTitle.Font = Enum.Font.SourceSansBold
langTitle.TextSize = 16
langTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
langTitle.Text = "Выбери язык / Choose language"
langTitle.Parent = langFrame

local ruBtn = Instance.new("TextButton")
ruBtn.Size = UDim2.new(0.45, -5, 0, 36)
ruBtn.Position = UDim2.new(0.05, 0, 0, 50)
ruBtn.Text = "Русский"
ruBtn.Font = Enum.Font.SourceSansBold
ruBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ruBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
ruBtn.AutoButtonColor = false
ruBtn.Parent = langFrame
local ruCorner = Instance.new("UICorner", ruBtn)
ruCorner.CornerRadius = UDim.new(0, 6)

local enBtn = Instance.new("TextButton")
enBtn.Size = UDim2.new(0.45, -5, 0, 36)
enBtn.Position = UDim2.new(0.5, 5, 0, 50)
enBtn.Text = "English"
enBtn.Font = Enum.Font.SourceSansBold
enBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
enBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
enBtn.AutoButtonColor = false
enBtn.Parent = langFrame
local enCorner = Instance.new("UICorner", enBtn)
enCorner.CornerRadius = UDim.new(0, 6)

local function pickLanguage(lang)
    currentLanguage = lang
    rebuildTabs()
    openMenu()
    langFrame:Destroy()
    if TabsBase[1] then
        openTab(TabsBase[1])
    end
end

ruBtn.Activated:Connect(function()
    pickLanguage("ru")
end)

enBtn.Activated:Connect(function()
    pickLanguage("en")
end)

if cam then
    cam:GetPropertyChangedSignal("ViewportSize"):Connect(function()
        if not isMaximized then
            mainFrame.Size = getStartSize()
        end
    end)
end
