--============================================================
-- KAIOX HUB + TELA DE CARREGAMENTO (10s)
--============================================================

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local Run = game:GetService("RunService")

local LP = Players.LocalPlayer
local Mouse = LP:GetMouse()

--============================================================
-- *** TELA DE CARREGAMENTO ***
--============================================================

local LoadingGui = Instance.new("ScreenGui", LP:WaitForChild("PlayerGui"))
LoadingGui.ResetOnSpawn = false
LoadingGui.IgnoreGuiInset = true
LoadingGui.ZIndexBehavior = Enum.ZIndexBehavior.Global

-- Fundo preto
local Black = Instance.new("Frame", LoadingGui)
Black.Size = UDim2.new(1,0,1,0)
Black.BackgroundColor3 = Color3.fromRGB(0,0,0)
Black.ZIndex = 10

-- Foto
local Photo = Instance.new("ImageLabel", Black)
Photo.Size = UDim2.new(0,300,0,300)
Photo.Position = UDim2.new(0.5,-150,0.28,-150)
Photo.BackgroundTransparency = 1
Photo.ZIndex = 11
Photo.Image = "rbxassetid://77189072242089"

-- Texto CARREGANDO:
local LoadingTxt = Instance.new("TextLabel", Black)
LoadingTxt.Size = UDim2.new(0,300,0,60)
LoadingTxt.Position = UDim2.new(0.5,-150,0.57,0)
LoadingTxt.BackgroundTransparency = 1
LoadingTxt.TextColor3 = Color3.fromRGB(255,255,255)
LoadingTxt.Font = Enum.Font.GothamBold
LoadingTxt.TextScaled = true
LoadingTxt.ZIndex = 11
LoadingTxt.Text = "CARREGANDO:"

-- Mensagens de 5 em 5s
local msgs = {
	"Iniciando scripts...",
	"Carregando interface...",
	"Otimizando performance...",
	"Preparando ambiente...",
	"Quase lá..."
}

task.spawn(function()
	while Black.Parent do
		for _,m in ipairs(msgs) do
			LoadingTxt.Text = "CARREGANDO: " .. m
			task.wait(5)
		end
	end
end)

-- Estrelas infinitas
task.spawn(function()
	while Black.Parent do
		local star = Instance.new("Frame", Black)
		star.Size = UDim2.new(0,5,0,5)
		star.BackgroundColor3 = Color3.fromRGB(255,255,255)
		star.Position = UDim2.new(0,-10,math.random(),0)
		star.ZIndex = 11
		Instance.new("UICorner", star).CornerRadius = UDim.new(1,0)

		task.spawn(function()
			for i = 0,1,0.01 do
				star.Position = UDim2.new(i, -10, star.Position.Y.Scale, 0)
				task.wait(0.02)
			end
			star:Destroy()
		end)

		task.wait(0.3)
	end
end)

-- Barra base
local BarBack = Instance.new("Frame", Black)
BarBack.Size = UDim2.new(0,500,0,40)
BarBack.Position = UDim2.new(0.5,-250,0.70,0)
BarBack.BackgroundColor3 = Color3.fromRGB(40,40,40)
BarBack.ZIndex = 10
Instance.new("UICorner", BarBack).CornerRadius = UDim.new(0,10)

-- Barra de progresso
local BarFill = Instance.new("Frame", BarBack)
BarFill.Size = UDim2.new(0,0,1,0)
BarFill.BackgroundColor3 = Color3.fromRGB(120,0,255)
BarFill.ZIndex = 11
Instance.new("UICorner", BarFill).CornerRadius = UDim.new(0,10)

-- Texto %
local Percent = Instance.new("TextLabel", Black)
Percent.Size = UDim2.new(0,200,0,50)
Percent.Position = UDim2.new(0.5,-100,0.63,0)
Percent.BackgroundTransparency = 1
Percent.TextColor3 = Color3.fromRGB(255,255,255)
Percent.TextScaled = true
Percent.Font = Enum.Font.GothamBold
Percent.ZIndex = 11
Percent.Text = "0%"

-- Animação do loading (10s)
task.spawn(function()
	for i = 1, 100 do
		BarFill.Size = UDim2.new(i/100,0,1,0)
		Percent.Text = i.."%"
		task.wait(0.10)
	end
	LoadingGui:Destroy()
end)

--============================================================
-- KAIOX HUB (MESMO DE ANTES)
--============================================================

local gui = Instance.new("ScreenGui", LP:WaitForChild("PlayerGui"))
gui.ResetOnSpawn = false

--============================================================
-- BOTÃO REDONDO (AGORA 12s)
--============================================================

local OpenBtn = Instance.new("ImageButton")
OpenBtn.Size = UDim2.new(0, 70, 0, 70)
OpenBtn.Position = UDim2.new(0.05, 0, 0.5, 0)
OpenBtn.BackgroundColor3 = Color3.fromRGB(20,20,20)
OpenBtn.AutoButtonColor = true
OpenBtn.Image = ""
OpenBtn.Visible = false -- começa invisível
OpenBtn.Parent = gui

-- aparece depois de 12s
task.delay(12, function()
	OpenBtn.Visible = true
end)

local circle = Instance.new("UICorner", OpenBtn)
circle.CornerRadius = UDim.new(1,0)

local Icon = Instance.new("ImageLabel", OpenBtn)
Icon.Size = UDim2.new(0.6,0,0.6,0)
Icon.Position = UDim2.new(0.2,0,0.2,0)
Icon.BackgroundTransparency = 1
Icon.Image = "rbxassetid://3926305904"
Icon.ImageRectOffset = Vector2.new(884,204)
Icon.ImageRectSize = Vector2.new(36,36)

--========== DRAG DO BOTÃO ==========
local dragging = false
local dragStart, startPos

OpenBtn.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = OpenBtn.Position
	end
end)

OpenBtn.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging then
		local delta = input.Position - dragStart
		OpenBtn.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

--============================================================
-- BARRA SUPERIOR
--============================================================

local Bar = Instance.new("Frame", gui)
Bar.Size = UDim2.new(0, 450, 0, 55)
Bar.Position = UDim2.new(0.3, 0, 0.15, 0)
Bar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Bar.BackgroundTransparency = 0.15
Bar.Visible = false

Instance.new("UICorner", Bar).CornerRadius = UDim.new(0, 12)

local B1 = Instance.new("UIStroke", Bar)
B1.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
B1.Color = Color3.fromRGB(120,0,255)
B1.Thickness = 3

--========== BOTÃO MOVER ==========
local MoveBar = Instance.new("TextButton", Bar)
MoveBar.Size = UDim2.new(0, 45, 1, 0)
MoveBar.Text = "+"
MoveBar.TextColor3 = Color3.fromRGB(255,255,255)
MoveBar.TextScaled = true
MoveBar.BackgroundColor3 = Color3.fromRGB(40,40,40)
Instance.new("UICorner", MoveBar).CornerRadius = UDim.new(0,12)

local draggingBar = false
local dragBarStart, barStartPos

MoveBar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 
	or input.UserInputType == Enum.UserInputType.Touch then
		draggingBar = true
		dragBarStart = input.Position
		barStartPos = Bar.Position
	end
end)

MoveBar.InputEnded:Connect(function()
	draggingBar = false
end)

UIS.InputChanged:Connect(function(input)
	if draggingBar then
		local delta = input.Position - dragBarStart
		Bar.Position = UDim2.new(barStartPos.X.Scale, barStartPos.X.Offset + delta.X, barStartPos.Y.Scale, barStartPos.Y.Offset + delta.Y)
	end
end)

--========== TÍTULO KAIOX ==========
local Title = Instance.new("TextLabel", Bar)
Title.Size = UDim2.new(0.65, 0, 1, 0)
Title.Position = UDim2.new(0.15, 0, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "KAIOX HUB"
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold

task.spawn(function()
	while true do
		for h=0,1,0.01 do
			Title.TextColor3 = Color3.fromHSV(h,1,1)
			task.wait(0.03)
		end
	end
end)

--========== + / - ==========
local PlusBtn = Instance.new("TextButton", Bar)
PlusBtn.Size = UDim2.new(0, 45, 1, 0)
PlusBtn.Position = UDim2.new(0.80, 0, 0, 0)
PlusBtn.Text = "+"
PlusBtn.TextScaled = true
PlusBtn.TextColor3 = Color3.fromRGB(255,255,255)
PlusBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
Instance.new("UICorner", PlusBtn).CornerRadius = UDim.new(0,12)

--========== X ==========
local CloseBtn = Instance.new("TextButton", Bar)
CloseBtn.Size = UDim2.new(0, 45, 1, 0)
CloseBtn.Position = UDim2.new(0.90,0,0,0)
CloseBtn.Text = "X"
CloseBtn.TextScaled = true
CloseBtn.TextColor3 = Color3.fromRGB(255,255,255)
CloseBtn.BackgroundColor3 = Color3.fromRGB(70,0,0)
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0,12)

--============================================================
-- MENU
--============================================================

local Menu = Instance.new("Frame", gui)
Menu.Size = UDim2.new(0, 540, 0, 420)
Menu.Position = UDim2.new(0.3,0,0.25,0)
Menu.BackgroundColor3 = Color3.fromRGB(20,20,20)
Menu.BackgroundTransparency = 0.15
Menu.Visible = false

local MenuCorner = Instance.new("UICorner", Menu)
MenuCorner.CornerRadius = UDim.new(0,14)

local MenuStroke = Instance.new("UIStroke", Menu)
MenuStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
MenuStroke.Color = Color3.fromRGB(120,0,255)
MenuStroke.Thickness = 3

-- MENU DRAG
local menuDragging = false
local menuStartPos, menuMouseStart

Menu.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		menuDragging = true
		menuStartPos = Menu.Position
		menuMouseStart = input.Position
	end
end)

Menu.InputEnded:Connect(function()
	menuDragging = false
end)

UIS.InputChanged:Connect(function(input)
	if menuDragging then
		local delta = input.Position - menuMouseStart
		Menu.Position = UDim2.new(menuStartPos.X.Scale, menuStartPos.X.Offset + delta.X, menuStartPos.Y.Scale, menuStartPos.Y.Offset + delta.Y)
	end
end)

--============================================================
-- CATEGORIAS
--============================================================

local Left = Instance.new("Frame", Menu)
Left.Size = UDim2.new(0.30,0,1,0)
Left.BackgroundColor3 = Color3.fromRGB(25,25,25)

local LeftCorner = Instance.new("UICorner", Left)
LeftCorner.CornerRadius = UDim.new(0,14)

local function CreateCategory(txt, y)
	local b = Instance.new("TextButton", Left)
	b.Size = UDim2.new(1,0,0,55)
	b.Position = UDim2.new(0,0,0,y)
	b.BackgroundColor3 = Color3.fromRGB(40,40,40)
	b.TextColor3 = Color3.fromRGB(255,255,255)
	b.TextScaled = true
	b.Font = Enum.Font.GothamBold

	local bc = Instance.new("UICorner", b)
	bc.CornerRadius = UDim.new(0,14)

	local bs = Instance.new("UIStroke", b)
	bs.Color = Color3.fromRGB(120,0,255)
	bs.Thickness = 2

	b.Text = txt
	return b
end

local UniversalBtn = CreateCategory("UNIVERSAL",0)
local RpBtn = CreateCategory("RP",55)
local CredBtn = CreateCategory("CREDITO",110)

--============================================================
-- DIREITA / CONTEÚDO
--============================================================

local Right = Instance.new("Frame", Menu)
Right.Size = UDim2.new(0.70,0,1,0)
Right.Position = UDim2.new(0.30,0,0,0)
Right.BackgroundColor3 = Color3.fromRGB(22,22,22)

local RightCorner = Instance.new("UICorner", Right)
RightCorner.CornerRadius = UDim.new(0,14)

-- UNIVERSAL PAGE
local UniversalPage = Instance.new("Frame", Right)
UniversalPage.Size = UDim2.new(1,0,1,0)
UniversalPage.BackgroundTransparency = 1

local function CreateOption(txt, y)
	local b = Instance.new("TextButton", UniversalPage)
	b.Size = UDim2.new(1,-20,0,55)
	b.Position = UDim2.new(0,10,0,y)
	b.BackgroundColor3 = Color3.fromRGB(40,40,40)
	b.TextColor3 = Color3.fromRGB(255,255,255)
	b.TextScaled = true
	b.Font = Enum.Font.GothamBold

	local bc = Instance.new("UICorner", b)
	bc.CornerRadius = UDim.new(0,14)

	local bs = Instance.new("UIStroke", b)
	bs.Color = Color3.fromRGB(120,0,255)
	bs.Thickness = 2

	b.Text = txt
	return b
end

local InfiniteBtn = CreateOption("InfiniteJump", 10)
local NoclipBtn = CreateOption("Noclip", 70)

local SpeedBox = Instance.new("TextBox", UniversalPage)
SpeedBox.Size = UDim2.new(1,-20,0,50)
SpeedBox.Position = UDim2.new(0,10,0,130)
SpeedBox.Text = "WalkSpeed"
SpeedBox.TextScaled = true
SpeedBox.TextColor3 = Color3.fromRGB(255,255,255)
SpeedBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
local s1 = Instance.new("UICorner", SpeedBox)
s1.CornerRadius = UDim.new(0,14)
local s2 = Instance.new("UIStroke", SpeedBox)
s2.Color = Color3.fromRGB(120,0,255)
s2.Thickness = 2

local JumpBox = Instance.new("TextBox", UniversalPage)
JumpBox.Size = UDim2.new(1,-20,0,50)
JumpBox.Position = UDim2.new(0,10,0,190)
JumpBox.Text = "JumpPower"
JumpBox.TextScaled = true
JumpBox.TextColor3 = Color3.fromRGB(255,255,255)
JumpBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
local jp1 = Instance.new("UICorner", JumpBox)
jp1.CornerRadius = UDim.new(0,14)
local jp2 = Instance.new("UIStroke", JumpBox)
jp2.Color = Color3.fromRGB(120,0,255)
jp2.Thickness = 2

-- RP PAGE
local RpPage = Instance.new("Frame", Right)
RpPage.Size = UDim2.new(1,0,1,0)
RpPage.BackgroundTransparency = 1
RpPage.Visible = false

local Soon = Instance.new("TextLabel", RpPage)
Soon.Size = UDim2.new(1,0,1,0)
Soon.BackgroundTransparency = 1
Soon.Text = "EM BREVE"
Soon.TextColor3 = Color3.fromRGB(255,255,255)
Soon.TextScaled = true

-- CREDITO PAGE
local CredPage = Instance.new("Frame", Right)
CredPage.Size = UDim2.new(1,0,1,0)
CredPage.BackgroundTransparency = 1
CredPage.Visible = false

local creditTitle = Instance.new("TextLabel", CredPage)
creditTitle.Size = UDim2.new(1,0,0,50)
creditTitle.Position = UDim2.new(0,0,0,20)
creditTitle.BackgroundTransparency = 1
creditTitle.Text = "Feito por: KAIOX"
creditTitle.TextColor3 = Color3.fromRGB(255,255,255)
creditTitle.TextScaled = true
creditTitle.Font = Enum.Font.GothamBold

local creditYT = Instance.new("TextLabel", CredPage)
creditYT.Size = UDim2.new(1,0,0,50)
creditYT.Position = UDim2.new(0,0,0,90)
creditYT.BackgroundTransparency = 1
creditYT.Text = "YOUTUBE: KAIOXKX"
creditYT.TextColor3 = Color3.fromRGB(255,255,255)
creditYT.TextScaled = true
creditYT.Font = Enum.Font.GothamBold

--============================================================
-- FUNÇÕES DO HUB
--============================================================

local toggleState = false

OpenBtn.MouseButton1Click:Connect(function()
	toggleState = not toggleState
	if toggleState then
		Bar.Visible = true
	else
		Bar.Visible = false
		Menu.Visible = false
		PlusBtn.Text = "+"
		menuOpen = false
	end
end)

CloseBtn.MouseButton1Click:Connect(function()
	Bar.Visible = false
	Menu.Visible = false
	PlusBtn.Text = "+"
	menuOpen = false
end)

local menuOpen = false
PlusBtn.MouseButton1Click:Connect(function()
	menuOpen = not menuOpen
	if menuOpen then
		PlusBtn.Text = "-"
		Menu.Visible = true
	else
		PlusBtn.Text = "+"
		Menu.Visible = false
	end
end)

UniversalBtn.MouseButton1Click:Connect(function()
	UniversalPage.Visible = true
	RpPage.Visible = false
	CredPage.Visible = false
end)

RpBtn.MouseButton1Click:Connect(function()
	UniversalPage.Visible = false
	RpPage.Visible = true
	CredPage.Visible = false
end)

CredBtn.MouseButton1Click:Connect(function()
	UniversalPage.Visible = false
	RpPage.Visible = false
	CredPage.Visible = true
end)

--============================================================
-- UNIVERSAL FUNÇÕES
--============================================================

local infinite = false
InfiniteBtn.MouseButton1Click:Connect(function()
	infinite = not infinite
	InfiniteBtn.Text = infinite and "InfiniteJump: ON" or "InfiniteJump"
end)

UIS.JumpRequest:Connect(function()
	if infinite and LP.Character then
		LP.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
	end
end)

local noclip = false
NoclipBtn.MouseButton1Click:Connect(function()
	noclip = not noclip
	NoclipBtn.Text = noclip and "Noclip: ON" or "Noclip"
end)

Run.Stepped:Connect(function()
	if noclip and LP.Character then
		for _,v in pairs(LP.Character:GetDescendants()) do
			if v:IsA("BasePart") then
				v.CanCollide = false
			end
		end
	end
end)

-- WalkSpeed
SpeedBox.FocusLost:Connect(function()
	local n = tonumber(SpeedBox.Text)
	if n and LP.Character then
		LP.Character.Humanoid.WalkSpeed = n
	end
end)

-- JumpPower
JumpBox.FocusLost:Connect(function()
	local n = tonumber(JumpBox.Text)
	if n and LP.Character then
		LP.Character.Humanoid.JumpPower = n
	end
end)
