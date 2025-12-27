-- KAIOX HUB 

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- ================= GUI BASE =================
local gui = Instance.new("ScreenGui")
gui.IgnoreGuiInset = true
gui.ResetOnSpawn = false
gui.Parent = player.PlayerGui

local fundo = Instance.new("Frame")
fundo.Size = UDim2.fromScale(1,1)
fundo.BackgroundColor3 = Color3.new(0,0,0)
fundo.Parent = gui

-- ================= MÚSICA =================
local musica = Instance.new("Sound")
musica.SoundId = "rbxassetid://79148752172118"
musica.Volume = 1
musica.Looped = true
musica.Parent = gui
musica:Play()

-- ================= ESTRELAS =================
local estrelas = {}
local STAR_COUNT = 25
local STAR_SPEED = 120
local estrelasAtivas = true

for i = 1, STAR_COUNT do
	local s = Instance.new("Frame")
	s.Size = UDim2.fromOffset(2,2)
	s.BackgroundColor3 = Color3.new(1,1,1)
	s.BorderSizePixel = 0
	s.Position = UDim2.fromOffset(
		math.random(0, camera.ViewportSize.X),
		math.random(0, camera.ViewportSize.Y)
	)
	s.Parent = fundo
	table.insert(estrelas, s)
end

RunService.RenderStepped:Connect(function(dt)
	if not estrelasAtivas then return end
	for _, s in ipairs(estrelas) do
		s.Position += UDim2.fromOffset(STAR_SPEED * dt, 0)
		if s.Position.X.Offset > camera.ViewportSize.X then
			s.Position = UDim2.fromOffset(-5, math.random(0, camera.ViewportSize.Y))
		end
	end
end)

-- ================= BLOCO CENTRAL =================
local bloco = Instance.new("Frame")
bloco.Size = UDim2.fromOffset(400,200)
bloco.Position = UDim2.fromScale(0.5,0.5)
bloco.AnchorPoint = Vector2.new(0.5,0.5)
bloco.BackgroundColor3 = Color3.fromRGB(106,43,217)
bloco.BorderColor3 = Color3.new(1,1,1)
bloco.BorderSizePixel = 2
bloco.Parent = fundo

local corner = Instance.new("UICorner", bloco)
corner.CornerRadius = UDim.new(0,50)

-- ================= TÍTULO =================
local titulo = Instance.new("TextLabel")
titulo.BackgroundTransparency = 1
titulo.Size = UDim2.new(1,0,0,50)
titulo.Position = UDim2.fromOffset(0,5)
titulo.Text = "KAIOX HUB"
titulo.Font = Enum.Font.FredokaOne
titulo.TextSize = 42
titulo.Parent = bloco

task.spawn(function()
	local h = 0
	while titulo.Parent do
		h = (h + 0.004) % 1
		titulo.TextColor3 = Color3.fromHSV(h,1,1)
		task.wait(0.03)
	end
end)

-- Linha branca
local linha = Instance.new("Frame")
linha.Size = UDim2.fromOffset(200,2)
linha.Position = UDim2.fromOffset(100,55)
linha.BackgroundColor3 = Color3.new(1,1,1)
linha.BorderSizePixel = 0
linha.Parent = bloco

-- ================= TEXTOS =================
local function criarTexto(txt, y)
	local t = Instance.new("TextLabel")
	t.BackgroundTransparency = 1
	t.Size = UDim2.new(1,0,0,20)
	t.Position = UDim2.fromOffset(0,y)
	t.Text = txt
	t.Font = Enum.Font.FredokaOne
	t.TextSize = 14
	t.TextColor3 = Color3.new(1,1,1)
	t.Parent = bloco
	return t
end

local carregando = criarTexto("CARREGANDO:", 65)
local status = criarTexto("CARREGANDO SCRIPTS", 85)

local mensagens = {
	"CARREGANDO SCRIPTS",
	"CARREGANDO MÓDULOS",
	"ESPERE SÓ MAIS UM POUCO!",
	"OTIMIZANDO SCRIPTS",
	"CARREGANDO INTERFACE"
}

task.spawn(function()
	while status.Parent do
		status.Text = mensagens[math.random(#mensagens)]
		task.wait(5)
	end
end)

-- ================= BARRA =================
local barraFundo = Instance.new("Frame")
barraFundo.Size = UDim2.fromOffset(300,12)
barraFundo.Position = UDim2.fromOffset(50,130)
barraFundo.BackgroundColor3 = Color3.new(0,0,0)
barraFundo.Parent = bloco

local barra = Instance.new("Frame")
barra.Size = UDim2.fromScale(0,1)
barra.Parent = barraFundo

task.spawn(function()
	local h = 0
	while barra.Parent do
		h = (h + 0.01) % 1
		barra.BackgroundColor3 = Color3.fromHSV(h,1,1)
		task.wait()
	end
end)

local porcento = Instance.new("TextLabel")
porcento.BackgroundTransparency = 1
porcento.Size = UDim2.fromScale(1,1)
porcento.Text = "0%"
porcento.Font = Enum.Font.FredokaOne
porcento.TextSize = 14
porcento.TextColor3 = Color3.new(1,1,1)
porcento.Parent = barraFundo

-- ================= PROGRESSO =================
for i = 1,100 do
	barra.Size = UDim2.fromScale(i/100,1)
	porcento.Text = i.."%"
	task.wait(0.045)
end

barraFundo.Visible = false
local exec = criarTexto("EXECUTANDO",130)
exec.TextColor3 = Color3.fromRGB(0,255,0)

task.wait(2)

-- ================= FADE SINCRONIZADO =================
estrelasAtivas = false
local tweenInfo = TweenInfo.new(2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

TweenService:Create(fundo, tweenInfo, {BackgroundTransparency = 1}):Play()
TweenService:Create(bloco, tweenInfo, {BackgroundTransparency = 1}):Play()
TweenService:Create(musica, tweenInfo, {Volume = 0}):Play()

for _, obj in ipairs(bloco:GetDescendants()) do
	if obj:IsA("TextLabel") then
		TweenService:Create(obj, tweenInfo, {TextTransparency = 1}):Play()
	elseif obj:IsA("Frame") then
		TweenService:Create(obj, tweenInfo, {BackgroundTransparency = 1}):Play()
	end
end

for _, s in ipairs(estrelas) do
	TweenService:Create(s, tweenInfo, {BackgroundTransparency = 1}):Play()
end

task.wait(2)

musica:Stop()
gui:Destroy()

-- ================= MENSAGEM FINAL =================
local finalGui = Instance.new("ScreenGui", player.PlayerGui)

local txt = Instance.new("TextLabel", finalGui)
txt.Size = UDim2.fromScale(1,1)
txt.BackgroundTransparency = 1
txt.TextWrapped = true
txt.Font = Enum.Font.FredokaOne
txt.TextSize = 24
txt.TextColor3 = Color3.new(1,1,1)
txt.TextTransparency = 1
txt.Text =
	"BEM VINDO "..player.Name..
	"\n\nESPERO QUE VOCÊ GOSTE DO KAIOX HUB!\n\nOBRIGADO POR EXECUTAR :)"

TweenService:Create(txt, TweenInfo.new(1), {TextTransparency = 0}):Play()
task.wait(5)
TweenService:Create(txt, TweenInfo.new(2), {TextTransparency = 1}):Play()
task.wait(2)

finalGui:Destroy()

-- ================= BOTÃO FLUTUANTE =================
local btnGui = Instance.new("ScreenGui", player.PlayerGui)
btnGui.ResetOnSpawn = false

local btn = Instance.new("ImageButton", btnGui)
btn.Size = UDim2.fromOffset(55,55)
btn.Position = UDim2.fromOffset(200,200)
btn.Image = "rbxassetid://128674111436582"
btn.BackgroundTransparency = 1

local c = Instance.new("UICorner", btn)
c.CornerRadius = UDim.new(1,0)

local dragging = false
local dragStart
local startPos

btn.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = btn.Position
	end
end)

btn.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging and (
		input.UserInputType == Enum.UserInputType.MouseMovement
		or input.UserInputType == Enum.UserInputType.Touch
	) then
		local delta = input.Position - dragStart
		btn.Position = UDim2.fromOffset(
			startPos.X.Offset + delta.X,
			startPos.Y.Offset + delta.Y
		)
	end
end)
-- ================= SERVIÇOS =================
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer

-- ================= GUI PRINCIPAL =================
local menuGui = Instance.new("ScreenGui", player.PlayerGui)
menuGui.Enabled = false
menuGui.ResetOnSpawn = false
menuGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- ================= BARRA =================
local menu = Instance.new("ImageLabel", menuGui)
menu.Size = UDim2.fromOffset(700,180)
menu.Position = UDim2.fromScale(0.5,0.5)
menu.AnchorPoint = Vector2.new(0.5,0.5)
menu.Image = "rbxassetid://121463511643141"
menu.ImageTransparency = 0.2
menu.BackgroundTransparency = 1
menu.Visible = false
menu.ZIndex = 50
menu.Active = true

-- ================= DRAG =================
local dragging = false
local dragStart, startPos

menu.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = menu.Position
	end
end)

menu.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging and (
		input.UserInputType == Enum.UserInputType.MouseMovement
		or input.UserInputType == Enum.UserInputType.Touch
	) then
		local delta = input.Position - dragStart
		menu.Position = UDim2.new(
			startPos.X.Scale, startPos.X.Offset + delta.X,
			startPos.Y.Scale, startPos.Y.Offset + delta.Y
		)
	end
end)

-- ================= TÍTULO =================
local menuTitle = Instance.new("TextLabel", menu)
menuTitle.Size = UDim2.fromScale(1,1)
menuTitle.BackgroundTransparency = 1
menuTitle.Position = UDim2.fromOffset(0,13)
menuTitle.Text = "KAIOX HUB"
menuTitle.Font = Enum.Font.FredokaOne
menuTitle.TextSize = 48
menuTitle.ZIndex = 51

task.spawn(function()
	local h = 0
	while menuTitle.Parent do
		h = (h + 0.005) % 1
		menuTitle.TextColor3 = Color3.fromHSV(h,1,1)
		task.wait(0.03)
	end
end)

-- ================= BOTÃO X =================
local closeBtn = Instance.new("TextButton", menu)
closeBtn.Size = UDim2.fromOffset(40,40)
closeBtn.Position = UDim2.new(1,-60,0,87) -- 10px pra trás
closeBtn.Text = "X"
closeBtn.Font = Enum.Font.FredokaOne
closeBtn.TextSize = 32
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.BackgroundTransparency = 1
closeBtn.ZIndex = 51

-- ================= BOTÃO + =================
local toggleBtn = Instance.new("TextButton", menu)
toggleBtn.Size = UDim2.fromOffset(40,40)
toggleBtn.Position = UDim2.fromOffset(20,87) -- 10px pra frente
toggleBtn.Text = "+"
toggleBtn.Font = Enum.Font.FredokaOne
toggleBtn.TextSize = 32
toggleBtn.TextColor3 = Color3.new(1,1,1)
toggleBtn.BackgroundTransparency = 1
toggleBtn.ZIndex = 51

-- ================= MENU EXTRA =================
local extraMenu = Instance.new("ImageLabel", menuGui)
extraMenu.Size = UDim2.fromOffset(1000,400)
extraMenu.Position = UDim2.fromScale(0.5,0.65)
extraMenu.AnchorPoint = Vector2.new(0.5,0.5)
extraMenu.Image = "rbxassetid://104698208097279"
extraMenu.BackgroundTransparency = 1
extraMenu.Visible = false
extraMenu.ZIndex = 10

local aberto = false
toggleBtn.MouseButton1Click:Connect(function()
	aberto = not aberto
	toggleBtn.Text = aberto and "-" or "+"
	extraMenu.Visible = aberto
end)

-- ================= CONFIRMAÇÃO =================
local confirmGui = Instance.new("ScreenGui", player.PlayerGui)
confirmGui.Enabled = false

local confirm = Instance.new("ImageLabel", confirmGui)
confirm.Size = UDim2.fromOffset(500,500)
confirm.Position = UDim2.fromScale(0.5,0.5)
confirm.AnchorPoint = Vector2.new(0.5,0.5)
confirm.Image = "rbxassetid://104698208097279"
confirm.BackgroundTransparency = 1

local txt1 = Instance.new("TextLabel", confirm)
txt1.BackgroundTransparency = 1
txt1.Size = UDim2.fromOffset(400,60)
txt1.Position = UDim2.fromOffset(50,40)
txt1.Text = "DESEJA SAIR DO"
txt1.Font = Enum.Font.FredokaOne
txt1.TextSize = 48
txt1.TextColor3 = Color3.new(1,1,1)

local txt2 = Instance.new("TextLabel", confirm)
txt2.BackgroundTransparency = 1
txt2.Size = UDim2.fromOffset(400,80)
txt2.Position = UDim2.fromOffset(50,110)
txt2.Text = "KAIOX HUB?"
txt2.Font = Enum.Font.FredokaOne
txt2.TextSize = 60

task.spawn(function()
	local h = 0
	while txt2.Parent do
		h = (h + 0.005) % 1
		txt2.TextColor3 = Color3.fromHSV(h,1,1)
		task.wait(0.03)
	end
end)

local sim = Instance.new("TextButton", confirm)
sim.Size = UDim2.fromOffset(150,60)
sim.Position = UDim2.fromOffset(60,220)
sim.Text = "SIM"
sim.Font = Enum.Font.FredokaOne
sim.TextSize = 32
sim.TextColor3 = Color3.new(1,1,1)
sim.BackgroundTransparency = 1

local nao = Instance.new("TextButton", confirm)
nao.Size = UDim2.fromOffset(150,60)
nao.Position = UDim2.fromOffset(290,220)
nao.Text = "NÃO"
nao.Font = Enum.Font.FredokaOne
nao.TextSize = 32
nao.TextColor3 = Color3.new(1,1,1)
nao.BackgroundTransparency = 1

-- ================= EVENTOS =================
btn.MouseButton1Click:Connect(function()
	menuGui.Enabled = true
	menu.Visible = true
end)

closeBtn.MouseButton1Click:Connect(function()
	confirmGui.Enabled = true
end)

nao.MouseButton1Click:Connect(function()
	confirmGui.Enabled = false
end)

sim.MouseButton1Click:Connect(function()
	menuGui:Destroy()
	btn:Destroy()
	confirmGui:Destroy()

	task.wait(1)

	local dcGui = Instance.new("ScreenGui", player.PlayerGui)
	local dc = Instance.new("TextLabel", dcGui)
	dc.Size = UDim2.fromScale(1,1)
	dc.BackgroundTransparency = 1
	dc.Font = Enum.Font.FredokaOne
	dc.TextSize = 28
	dc.TextColor3 = Color3.new(1,1,1)
	dc.TextTransparency = 1

	local created = os.date("%d/%m/%Y", player.AccountAge * -86400 + os.time())

	TweenService:Create(dc, TweenInfo.new(1), {TextTransparency = 0}):Play()

	-- ANIMAÇÃO DOS PONTOS
	task.spawn(function()
		local base = "DESCONECTANDO"
		local dots = { "", ".", "..", "...", "..", "." }
		while dc.Parent do
			for _,d in ipairs(dots) do
				dc.Text =
					base..d.."\n\n"..
					"Nome: "..player.Name.."\n"..
					"Data de entrada no Roblox: "..created.."\n"..
					"Situação: on"
				task.wait(0.4)
			end
		end
	end)

	task.wait(5)
	TweenService:Create(dc, TweenInfo.new(2), {TextTransparency = 1}):Play()
end)
-- ================= SISTEMA DE PÁGINAS (AJUSTE FINAL DEFINITIVO) =================
-- Usa o extraMenu (menu do +)

-- Título KAIOX HUB
local pagesTitle = Instance.new("TextLabel", extraMenu)
pagesTitle.Size = UDim2.fromOffset(400,70)
pagesTitle.Position = UDim2.new(0.5, -200, 0, 19)
pagesTitle.BackgroundTransparency = 1
pagesTitle.Text = "KAIOX HUB"
pagesTitle.Font = Enum.Font.FredokaOne
pagesTitle.TextSize = 60
pagesTitle.ZIndex = 20

task.spawn(function()
	local h = 0
	while pagesTitle.Parent do
		h = (h + 0.004) % 1
		pagesTitle.TextColor3 = Color3.fromHSV(h,1,1)
		task.wait(0.03)
	end
end)

-- Linha abaixo do título (SUBIU e ficou logo embaixo)
local titleLine = Instance.new("Frame", extraMenu)
titleLine.Size = UDim2.fromOffset(350,3)
titleLine.Position = UDim2.fromOffset(325,82) -- antes 95, agora mais pra cima
titleLine.BackgroundColor3 = Color3.fromRGB(106,43,217)
titleLine.BorderSizePixel = 0
titleLine.ZIndex = 20

-- Container das páginas
local pagesHolder = Instance.new("Frame", extraMenu)
pagesHolder.Size = UDim2.fromOffset(260,250)
pagesHolder.Position = UDim2.fromOffset(105,120)
pagesHolder.BackgroundTransparency = 1
pagesHolder.ZIndex = 20

-- Linha vertical separando páginas das opções
local sideLine = Instance.new("Frame", extraMenu)
sideLine.Size = UDim2.fromOffset(3,225)
sideLine.Position = UDim2.fromOffset(375,95)
sideLine.BackgroundColor3 = Color3.fromRGB(106,43,217)
sideLine.BorderSizePixel = 0
sideLine.ZIndex = 20

-- Função pra criar botões de página
local function createPageButton(text, yPos)
	local btn = Instance.new("TextButton", pagesHolder)
	btn.Size = UDim2.fromOffset(260,60)
	btn.Position = UDim2.fromOffset(0, yPos)
	btn.BackgroundColor3 = Color3.fromRGB(106,43,217)
	btn.BackgroundTransparency = 0.2
	btn.BorderSizePixel = 0
	btn.Text = text
	btn.Font = Enum.Font.FredokaOne
	btn.TextSize = 28
	btn.TextColor3 = Color3.new(1,1,1)
	btn.ZIndex = 21

	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0,12)

	return btn
end

-- Páginas
local pageUniversal = createPageButton("UNIVERSAL", 0)
local pageNexus = createPageButton("NEXUS", 70)
local pageCredits = createPageButton("CRÉDITOS", 140)

-- Página inicial
local currentPage = "UNIVERSAL"

local function selectPage(name)
	currentPage = name
end

pageUniversal.MouseButton1Click:Connect(function()
	selectPage("UNIVERSAL")
end)

pageNexus.MouseButton1Click:Connect(function()
	selectPage("NEXUS")
end)

pageCredits.MouseButton1Click:Connect(function()
	selectPage("CRÉDITOS")
end)

selectPage("UNIVERSAL")
-- ================= TOGGLE FINAL COM ESTADO LIMPO =================

local clickCount = 0
local hubVisivel = true
local animando = false

-- estado do menu extra
local extraWasOpen = false

-- pega todos os GuiObjects do HUB (menuGui + confirmGui)
local function getHubObjects()
	local list = {}

	local function scan(gui)
		for _,obj in ipairs(gui:GetDescendants()) do
			if obj:IsA("GuiObject") then
				table.insert(list, obj)
			end
		end
	end

	if menuGui then scan(menuGui) end
	if confirmGui then scan(confirmGui) end

	return list
end

local hubObjects = getHubObjects()

-- mostra / esconde tudo no mesmo frame
local function setHubVisible(show)
	if animando then return end
	animando = true

	-- se for esconder, guarda estado e fecha menu extra
	if not show and extraMenu then
		extraWasOpen = extraMenu.Visible
		extraMenu.Visible = false
		if toggleBtn then toggleBtn.Text = "+" end
	end

	for _,obj in ipairs(hubObjects) do
		if obj:IsA("TextLabel") or obj:IsA("TextButton") then
			obj.TextTransparency = show and 0 or 1
		end

		if obj:IsA("ImageLabel") or obj:IsA("ImageButton") then
			obj.ImageTransparency = show and 0 or 1
		end

		if obj:IsA("Frame") then
			obj.BackgroundTransparency = show and obj.BackgroundTransparency or 1
		end

		obj.Visible = show
	end

	-- quando voltar, NÃO reabre menu extra
	if show and extraMenu then
		extraMenu.Visible = false
		if toggleBtn then toggleBtn.Text = "+" end
	end

	hubVisivel = show
	animando = false
end

-- clique no botão flutuante
btn.MouseButton1Click:Connect(function()
	clickCount += 1

	-- 1º clique: só abre o menu principal (script antigo cuida)
	if clickCount == 1 then
		return
	end

	-- 2º clique em diante: toggle total
	setHubVisible(not hubVisivel)
end)
-- ================= CONTEÚDO DA PÁGINA UNIVERSAL =================

-- Holder do conteúdo da UNIVERSAL
local universalHolder = Instance.new("Frame", extraMenu)
universalHolder.Size = UDim2.fromOffset(380,260)
universalHolder.Position = UDim2.fromOffset(395,120)
universalHolder.BackgroundTransparency = 1
universalHolder.Visible = true
universalHolder.ZIndex = 20

-- Scroll
local scroll = Instance.new("ScrollingFrame", universalHolder)
scroll.Size = UDim2.fromScale(1,1)
scroll.CanvasSize = UDim2.fromOffset(0,520)
scroll.ScrollBarImageTransparency = 1
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel = 0

local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0,12)

-- Função base pra criar botões
local function createOption(text, bg)
	local btn = Instance.new("TextButton", scroll)
	btn.Size = UDim2.fromOffset(360,50)
	btn.BackgroundColor3 = bg or Color3.fromRGB(40,40,40)
	btn.Text = text
	btn.Font = Enum.Font.FredokaOne
	btn.TextSize = 22
	btn.TextColor3 = Color3.new(1,1,1)
	btn.BorderSizePixel = 0
	btn.ZIndex = 21

	local c = Instance.new("UICorner", btn)
	c.CornerRadius = UDim.new(0,10)

	return btn
end

-- INPUT VELOCIDADE
local speedBox = Instance.new("TextBox", scroll)
speedBox.Size = UDim2.fromOffset(360,50)
speedBox.BackgroundColor3 = Color3.fromRGB(0,0,0)
speedBox.PlaceholderText = "Velocidade"
speedBox.Text = ""
speedBox.Font = Enum.Font.FredokaOne
speedBox.TextSize = 22
speedBox.TextColor3 = Color3.new(1,1,1)
speedBox.BorderSizePixel = 0
speedBox.ClearTextOnFocus = false

Instance.new("UICorner", speedBox).CornerRadius = UDim.new(0,10)

-- INPUT PULO
local jumpBox = speedBox:Clone()
jumpBox.PlaceholderText = "Pulo"
jumpBox.Parent = scroll

-- BOTÕES OFF / ON
local function createToggle(text)
	local btn = createOption(text .. " : OFF")
	btn:SetAttribute("State", false)

	btn.MouseButton1Click:Connect(function()
		local state = not btn:GetAttribute("State")
		btn:SetAttribute("State", state)
		btn.Text = text .. (state and " : ON" or " : OFF")
	end)

	return btn
end

local espBtn = createToggle("ESP")
local infJumpBtn = createToggle("INFINITE JUMP")
local noclipBtn = createToggle("NOCLIP")

-- AIMBOT
local aimBtn = createToggle("AIMBOT")

-- ===== SERVIÇOS =====
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer

-- ===== VARIÁVEIS BASE =====
local wantedSpeed = 16
local wantedJump = 50

-- ================= VELOCIDADE & PULO =================

local function getHumanoid()
	if player.Character and player.Character:FindFirstChild("Humanoid") then
		return player.Character:FindFirstChild("Humanoid")
	end
end

speedBox.FocusLost:Connect(function()
	local val = tonumber(speedBox.Text)
	if val then
		wantedSpeed = math.clamp(val, 1, 300)
	end
	speedBox.Text = ""
	speedBox.PlaceholderText = "Velocidade (" .. wantedSpeed .. ")"
end)

jumpBox.FocusLost:Connect(function()
	local val = tonumber(jumpBox.Text)
	if val then
		wantedJump = math.clamp(val, 1, 300)
	end
	jumpBox.Text = ""
	jumpBox.PlaceholderText = "Pulo (" .. wantedJump .. ")"
end)

player.CharacterAdded:Connect(function()
	task.wait(1)
	local hum = getHumanoid()
	if hum then
		hum.WalkSpeed = wantedSpeed
		hum.JumpPower = wantedJump
	end
end)

RunService.Heartbeat:Connect(function()
	local hum = getHumanoid()
	if hum then
		if hum.WalkSpeed ~= wantedSpeed then
			hum.WalkSpeed = wantedSpeed
		end
		if hum.JumpPower ~= wantedJump then
			hum.JumpPower = wantedJump
		end
	end
end)

-- ================= INFINITE JUMP =================

UserInputService.JumpRequest:Connect(function()
	if infJumpBtn:GetAttribute("State") then
		local hum = getHumanoid()
		if hum then
			hum:ChangeState(Enum.HumanoidStateType.Jumping)
		end
	end
end)

-- ================= NOCLIP =================

RunService.Stepped:Connect(function()
	if noclipBtn:GetAttribute("State") then
		if player.Character then
			for _, part in pairs(player.Character:GetDescendants()) do
				if part:IsA("BasePart") then
					part.CanCollide = false
				end
			end
		end
	end
end)

-- ================= ESP COM CONTORNO =================

local espObjects = {}

local function removeESP(plr)
	if espObjects[plr] then
		espObjects[plr]:Destroy()
		espObjects[plr] = nil
	end
end

local function applyESP(plr)
	if plr == player then return end
	if not espBtn:GetAttribute("State") then return end
	if not plr.Character then return end

	removeESP(plr)

	local highlight = Instance.new("Highlight")
	highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	highlight.FillTransparency = 1
	highlight.OutlineTransparency = 0
	highlight.OutlineColor = plr.Team and plr.Team.TeamColor.Color or Color3.new(1,1,1)
	highlight.Adornee = plr.Character
	highlight.Parent = plr.Character

	espObjects[plr] = highlight

	plr:GetPropertyChangedSignal("Team"):Connect(function()
		if highlight then
			highlight.OutlineColor = plr.Team and plr.Team.TeamColor.Color or Color3.new(1,1,1)
		end
	end)
end

local function refreshESP()
	for _,plr in pairs(game.Players:GetPlayers()) do
		if espBtn:GetAttribute("State") then
			applyESP(plr)
		else
			removeESP(plr)
		end
	end
end

game.Players.PlayerAdded:Connect(function(plr)
	plr.CharacterAdded:Connect(function()
		task.wait(1)
		applyESP(plr)
	end)
end)

espBtn:GetAttributeChangedSignal("State"):Connect(function()
	refreshESP()
end)

game:GetService("RunService").Heartbeat:Connect(function()
	if espBtn:GetAttribute("State") then
		refreshESP()
	end
end)
-- ================= AIMBOT =================

local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

local aiming = false
local currentTarget = nil
local lastCamCF = Camera.CFrame

local function isAlive(plr)
	return plr.Character
	and plr.Character:FindFirstChild("Humanoid")
	and plr.Character.Humanoid.Health > 0
	and plr.Character:FindFirstChild("Head")
end

local function hasLineOfSight(targetHead)
	local params = RaycastParams.new()
	params.FilterType = Enum.RaycastFilterType.Blacklist
	params.FilterDescendantsInstances = {player.Character}

	local origin = Camera.CFrame.Position
	local dir = (targetHead.Position - origin)

	local result = workspace:Raycast(origin, dir, params)

	if result then
		return result.Instance:IsDescendantOf(targetHead.Parent)
	end

	return true
end

local function getClosestTarget()
	local closest, dist = nil, math.huge

	for _,plr in pairs(game.Players:GetPlayers()) do
		if plr ~= player and isAlive(plr) then
			local head = plr.Character.Head
			local d = (Camera.CFrame.Position - head.Position).Magnitude

			if d < dist and hasLineOfSight(head) then
				dist = d
				closest = plr
			end
		end
	end

	return closest
end

RunService.RenderStepped:Connect(function()
	if not aimBtn:GetAttribute("State") then
		currentTarget = nil
		return
	end

	-- detectou movimento forte da camera → solta o alvo
	if (Camera.CFrame.Position - lastCamCF.Position).Magnitude > 1 then
		currentTarget = nil
	end

	if not currentTarget or not isAlive(currentTarget) then
		currentTarget = getClosestTarget()
	end

	if currentTarget and isAlive(currentTarget) then
		local head = currentTarget.Character.Head
		if hasLineOfSight(head) then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position, head.Position)
		else
			currentTarget = nil
		end
	end

	lastCamCF = Camera.CFrame
end)
-- ================= CONTEÚDO DA PÁGINA CRÉDITOS (Informações de tudo) =================

local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")

local creditsHolder = Instance.new("Frame", extraMenu)
creditsHolder.Size = UDim2.fromOffset(380,260)
creditsHolder.Position = UDim2.fromOffset(370,65) -- 25 pra esquerda e 55 pra cima
creditsHolder.BackgroundTransparency = 1
creditsHolder.Visible = false
creditsHolder.ZIndex = 20

local layout = Instance.new("UIListLayout", creditsHolder)
layout.Padding = UDim.new(0,6)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Left

local function creditLabel(text, size)
	local lbl = Instance.new("TextLabel", creditsHolder)
	lbl.Size = UDim2.fromOffset(360,36)
	lbl.BackgroundTransparency = 1
	lbl.Text = text
	lbl.Font = Enum.Font.FredokaOne
	lbl.TextSize = size or 22
	lbl.TextColor3 = Color3.new(1,1,1)
	lbl.TextXAlignment = Enum.TextXAlignment.Left
	lbl.ZIndex = 21
	return lbl
end

-- TÍTULO RAINBOW
local title = creditLabel("KAIOX HUB", 32)

-- RAINBOW FRAME A FRAME
task.spawn(function()
	local h = 0
	while title.Parent do
		h = (h + 0.003) % 1
		title.TextColor3 = Color3.fromHSV(h,1,1)
		task.wait()
	end
end)

local creator = creditLabel("Criador: KAIOXKX")
local yt = creditLabel("YouTube: KAIOXKX")

-- RAINBOW NOS TEXTOS
for _,lbl in ipairs({creator, yt}) do
	task.spawn(function()
		local h = 0
		while lbl.Parent do
			h = (h + 0.002) % 1
			lbl.TextColor3 = Color3.fromHSV(h,1,1)
			task.wait()
		end
	end)
end

local fpsLabel = creditLabel("Fps: ...")
local gameName = MarketplaceService:GetProductInfo(game.PlaceId).Name
local gameLabel = creditLabel("Jogo: "..gameName)

-- FPS EM TEMPO REAL
local frames = 0
local last = tick()

RunService.RenderStepped:Connect(function()
	frames += 1
	if tick() - last >= 1 then
		fpsLabel.Text = "Fps: "..frames
		frames = 0
		last = tick()
	end
end)

-- PERFIL DO JOGADOR
local profileFrame = Instance.new("Frame", creditsHolder)
profileFrame.Size = UDim2.fromOffset(360,70)
profileFrame.BackgroundTransparency = 1

local avatar = Instance.new("ImageLabel", profileFrame)
avatar.Size = UDim2.fromOffset(55,55)
avatar.Position = UDim2.fromOffset(0,5)
avatar.BackgroundTransparency = 1
Instance.new("UICorner", avatar).CornerRadius = UDim.new(1,0)

local userId = player.UserId
local thumb = Players:GetUserThumbnailAsync(userId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size100x100)
avatar.Image = thumb

local nameLbl = Instance.new("TextLabel", profileFrame)
nameLbl.Size = UDim2.fromOffset(280,26)
nameLbl.Position = UDim2.fromOffset(65,6)
nameLbl.BackgroundTransparency = 1
nameLbl.Text = player.Name
nameLbl.Font = Enum.Font.FredokaOne
nameLbl.TextSize = 20
nameLbl.TextXAlignment = Enum.TextXAlignment.Left
nameLbl.TextColor3 = Color3.new(1,1,1)

local dateLbl = Instance.new("TextLabel", profileFrame)
dateLbl.Size = UDim2.fromOffset(280,26)
dateLbl.Position = UDim2.fromOffset(65,36)
dateLbl.BackgroundTransparency = 1
dateLbl.Font = Enum.Font.FredokaOne
dateLbl.TextSize = 18
dateLbl.TextXAlignment = Enum.TextXAlignment.Left
dateLbl.TextColor3 = Color3.new(1,1,1)

local created = os.date("%d/%m/%Y", os.time() - (player.AccountAge * 86400))
dateLbl.Text = "Data de entrada: "..created

-- ================= AJUSTE FINAL DAS PÁGINAS =================

local oldSelect = selectPage
function selectPage(name)
	currentPage = name
	universalHolder.Visible = (name == "UNIVERSAL")
	creditsHolder.Visible = (name == "CRÉDITOS")
end

selectPage("UNIVERSAL")
