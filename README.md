-- CONFIG
local RUN_SPEED = 70

-- PLAYER
local p = game.Players.LocalPlayer
local h = (p.Character or p.CharacterAdded:Wait()):WaitForChild("Humanoid")
local normal = h.WalkSpeed

-- GUI
local gui = Instance.new("ScreenGui", p.PlayerGui)
gui.ResetOnSpawn = false

local b = Instance.new("TextButton", gui)
b.Size = UDim2.new(0, 80, 0, 80)
b.Position = UDim2.new(0.5, -40, 0.5, -40) -- meio da tela
b.Text = "RUN"
b.TextScaled = true
b.AutoButtonColor = false

Instance.new("UICorner", b).CornerRadius = UDim.new(1, 0)
local s = Instance.new("UIStroke", b)
s.Thickness = 2

-- CORRER
b.MouseButton1Down:Connect(function()
	h.WalkSpeed = RUN_SPEED
end)

local stop = function()
	h.WalkSpeed = normal
end
b.MouseButton1Up:Connect(stop)
b.TouchEnded:Connect(stop)

-- 🖱️📱 ARRASTAR BOTÃO
do
	local UIS = game:GetService("UserInputService")
	local drag, startPos, startInput

	b.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then
			drag = true
			startInput = input.Position
			startPos = b.Position
		end
	end)

	b.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then
			drag = false
		end
	end)

	UIS.InputChanged:Connect(function(input)
		if drag and (input.UserInputType == Enum.UserInputType.MouseMovement
		or input.UserInputType == Enum.UserInputType.Touch) then
			local delta = input.Position - startInput
			b.Position = UDim2.new(
				startPos.X.Scale, startPos.X.Offset + delta.X,
				startPos.Y.Scale, startPos.Y.Offset + delta.Y
			)
		end
	end)
end

-- 🌈 RGB SUAVE
task.spawn(function()
	local i = 0
	while b.Parent do
		i = (i + 0.01) % 1
		local c = Color3.fromHSV(i, 1, 1)
		b.BackgroundColor3 = c
		s.Color = c
		task.wait()
	end
end)
