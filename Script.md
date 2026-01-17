--==================================================
-- SCRIPT BY MR RED BLACK
--==================================================

-- NOTIFICAÇÃO + CLIPBOARD
pcall(function()
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "HUB:",
        Text = "SCRIPT BY MR RED BLACK",
        Duration = 6
    })
    if setclipboard then
        setclipboard("https://discord.gg/ffuFGauPdS")
    end
end)

-- SERVICES
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local SoundService = game:GetService("SoundService")

local player = Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.ResetOnSpawn = false
gui.Name = "RBH_CLOCK"

-- MAIN FRAME
local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 420, 0, 280)
main.Position = UDim2.new(0.5, 0, 0.5, 0)
main.AnchorPoint = Vector2.new(0.5,0.5)
main.BackgroundColor3 = Color3.fromRGB(10,10,10)
main.BorderSizePixel = 0
main.Active = true

Instance.new("UICorner", main).CornerRadius = UDim.new(0,14)

-- RGB BORDA
local stroke = Instance.new("UIStroke", main)
stroke.Thickness = 2

task.spawn(function()
	while gui.Parent do
		for h = 0,1,0.01 do
			stroke.Color = Color3.fromHSV(h,1,1)
			task.wait(0.03)
		end
	end
end)



-- TITLE
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,40)
title.BackgroundTransparency = 1
title.Text = "RED BLACK CLOCK"
title.Font = Enum.Font.Code
title.TextSize = 22
title.TextColor3 = Color3.new(1,0,0)

-- TAB BUTTONS
local function tabButton(txt, x)
	local b = Instance.new("TextButton", main)
	b.Size = UDim2.new(0,120,0,30)
	b.Position = UDim2.new(0, x, 0, 45)
	b.Text = txt
	b.Font = Enum.Font.Code
	b.TextSize = 16
	b.BackgroundColor3 = Color3.fromRGB(20,20,20)
	b.TextColor3 = Color3.new(1,1,1)
	Instance.new("UICorner", b)
	return b
end

local btnClock = tabButton("RELÓGIO", 10)
local btnCrono = tabButton("CRONÔMETRO", 145)
local btnTimer = tabButton("TIMER", 280)

-- CONTENT FRAMES
local function contentFrame()
	local f = Instance.new("Frame", main)
	f.Size = UDim2.new(1,-20,1,-100)
	f.Position = UDim2.new(0,10,0,85)
	f.BackgroundTransparency = 1
	f.Visible = false
	return f
end

local clockFrame = contentFrame()
local cronoFrame = contentFrame()
local timerFrame = contentFrame()

-- CLOCK
local clockLabel = Instance.new("TextLabel", clockFrame)
clockLabel.Size = UDim2.new(1,0,1,0)
clockLabel.BackgroundTransparency = 1
clockLabel.Font = Enum.Font.Code
clockLabel.TextSize = 36
clockLabel.TextColor3 = Color3.new(0,1,0)

task.spawn(function()
	while gui.Parent do
		if clockFrame.Visible then
			clockLabel.Text = os.date("%H:%M:%S")
		end
		task.wait(1)
	end
end)

-- CRONÔMETRO
local cLabel = Instance.new("TextLabel", cronoFrame)
cLabel.Size = UDim2.new(1,0,0,60)
cLabel.BackgroundTransparency = 1
cLabel.Font = Enum.Font.Code
cLabel.TextSize = 32
cLabel.Text = "00:00"
cLabel.TextColor3 = Color3.new(1,1,1)

local cRunning = false
local cTime = 0

local function cButton(txt, y)
	local b = Instance.new("TextButton", cronoFrame)
	b.Size = UDim2.new(0,120,0,30)
	b.Position = UDim2.new(0.5,-60,0,y)
	b.Text = txt
	b.Font = Enum.Font.Code
	b.TextSize = 16
	b.BackgroundColor3 = Color3.fromRGB(30,30,30)
	b.TextColor3 = Color3.new(1,1,1)
	Instance.new("UICorner", b)
	return b
end

cButton("START", 70).MouseButton1Click:Connect(function() cRunning = true end)
cButton("STOP", 110).MouseButton1Click:Connect(function() cRunning = false end)
cButton("RESET", 150).MouseButton1Click:Connect(function()
	cRunning = false
	cTime = 0
	cLabel.Text = "00:00"
end)

task.spawn(function()
	while gui.Parent do
		if cRunning then
			cTime += 1
			cLabel.Text = string.format("%02d:%02d", math.floor(cTime/60), cTime%60)
		end
		task.wait(1)
	end
end)

-- TIMER
local tLabel = Instance.new("TextLabel", timerFrame)
tLabel.Size = UDim2.new(1,0,0,40)
tLabel.BackgroundTransparency = 1
tLabel.Font = Enum.Font.Code
tLabel.TextSize = 30
tLabel.Text = "00:00"
tLabel.TextColor3 = Color3.new(1,0,0)

local input = Instance.new("TextBox", timerFrame)
input.Size = UDim2.new(0,200,0,30)
input.Position = UDim2.new(0.5,-100,0,50)
input.PlaceholderText = "Tempo em segundos"
input.Font = Enum.Font.Code
input.TextSize = 16
input.BackgroundColor3 = Color3.fromRGB(25,25,25)
input.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", input)

local tRunning = false
local tTime = 0

local alarm = Instance.new("Sound", SoundService)
alarm.SoundId = "rbxassetid://9118828564"
alarm.Volume = 1

local function tButton(txt, y, cb)
	local b = Instance.new("TextButton", timerFrame)
	b.Size = UDim2.new(0,120,0,30)
	b.Position = UDim2.new(0.5,-60,0,y)
	b.Text = txt
	b.Font = Enum.Font.Code
	b.TextSize = 16
	b.BackgroundColor3 = Color3.fromRGB(30,30,30)
	b.TextColor3 = Color3.new(1,1,1)
	Instance.new("UICorner", b)
	b.MouseButton1Click:Connect(cb)
end

tButton("START", 95, function()
	local v = tonumber(input.Text)
	if v then
		tTime = v
		tRunning = true
	end
end)

tButton("STOP", 135, function() tRunning = false end)
tButton("RESET", 175, function()
	tRunning = false
	tLabel.Text = "00:00"
end)

task.spawn(function()
	while gui.Parent do
		if tRunning and tTime > 0 then
			tTime -= 1
			tLabel.Text = string.format("%02d:%02d", math.floor(tTime/60), tTime%60)
			if tTime <= 0 then
				tRunning = false
				alarm:Play()
			end
		end
		task.wait(1)
	end
end)

-- TAB SWITCH
local function show(f)
	clockFrame.Visible = false
	cronoFrame.Visible = false
	timerFrame.Visible = false
	f.Visible = true
end

btnClock.MouseButton1Click:Connect(function() show(clockFrame) end)
btnCrono.MouseButton1Click:Connect(function() show(cronoFrame) end)
btnTimer.MouseButton1Click:Connect(function() show(timerFrame) end)

show(clockFrame)
