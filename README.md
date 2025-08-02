local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Cria GUI
local screenGui = Instance.new("ScreenGui", playerGui)
local toggleButton = Instance.new("TextButton", screenGui)
toggleButton.Size = UDim2.new(0, 60, 0, 60)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.TextScaled = true
toggleButton.Text = "➡️"

-- Texto animado
local fixedMessage = Instance.new("TextLabel", screenGui)
fixedMessage.Size = UDim2.new(0.4, 0, 0.1, 0)
fixedMessage.Position = UDim2.new(0.3, 0, 0.45, 0)
fixedMessage.BackgroundTransparency = 0.5
fixedMessage.BackgroundColor3 = Color3.new(0, 0, 0)
fixedMessage.TextColor3 = Color3.new(1, 1, 1)
fixedMessage.TextScaled = true
fixedMessage.Visible = false

local function showAnimatedMessage(text)
	fixedMessage.Text = text
	fixedMessage.Visible = true
	fixedMessage.TextTransparency = 1

	TweenService:Create(fixedMessage, TweenInfo.new(0.5), {TextTransparency = 0}):Play()
	task.wait(1)
	TweenService:Create(fixedMessage, TweenInfo.new(0.5), {TextTransparency = 1}):Play()
	task.wait(0.5)

	fixedMessage.Visible = false
end

-- Movimento
local movedForward = false
local previousPosition = nil

toggleButton.MouseButton1Click:Connect(function()
	local character = player.Character or player.CharacterAdded:Wait()
	local root = character:WaitForChild("HumanoidRootPart")

	if not movedForward then
		previousPosition = root.CFrame
		root.CFrame = root.CFrame + (root.CFrame.LookVector * 100)
		showAnimatedMessage("Saindo da base...")
		toggleButton.Text = "⬅️"
		movedForward = true
	else
		if previousPosition then
			root.CFrame = previousPosition
			showAnimatedMessage("Voltando...")
		end
		toggleButton.Text = "➡️"
		movedForward = false
	end
end)
