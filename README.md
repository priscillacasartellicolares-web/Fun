-- Fun V4 Hub Script with Open/Close, Actions, Draggable Feature
local Player = game.Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = Player:WaitForChild("PlayerGui")

-- Create the Hub Menu Frame
local hubFrame = Instance.new("Frame")
hubFrame.Size = UDim2.new(0.5, 0, 0.5, 0)
hubFrame.Position = UDim2.new(0.25, 0, 0.25, 0)
hubFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
hubFrame.BackgroundTransparency = 0.5
hubFrame.Visible = false  -- Start with the hub hidden
hubFrame.Parent = ScreenGui

-- Create title text
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0.2, 0)
titleLabel.Text = "NoobG Hub"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 24
titleLabel.BackgroundTransparency = 1
titleLabel.Parent = hubFrame

-- Button: Speed
local speedButton = Instance.new("TextButton")
speedButton.Size = UDim2.new(0.8, 0, 0.1, 0)
speedButton.Position = UDim2.new(0.1, 0, 0.3, 0)
speedButton.Text = "Speed Boost"
speedButton.BackgroundColor3 = Color3.fromRGB(0, 128, 0)
speedButton.TextColor3 = Color3.fromRGB(255, 255, 255)
speedButton.Parent = hubFrame

-- Button: Fly
local flyButton = Instance.new("TextButton")
flyButton.Size = UDim2.new(0.8, 0, 0.1, 0)
flyButton.Position = UDim2.new(0.1, 0, 0.45, 0)
flyButton.Text = "Fly"
flyButton.BackgroundColor3 = Color3.fromRGB(0, 0, 128)
flyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
flyButton.Parent = hubFrame

-- Button: Reset
local resetButton = Instance.new("TextButton")
resetButton.Size = UDim2.new(0.8, 0, 0.1, 0)
resetButton.Position = UDim2.new(0.1, 0, 0.6, 0)
resetButton.Text = "Reset Character"
resetButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
resetButton.TextColor3 = Color3.fromRGB(255, 255, 255)
resetButton.Parent = hubFrame

-- Button: Jump High
local jumpButton = Instance.new("TextButton")
jumpButton.Size = UDim2.new(0.8, 0, 0.1, 0)
jumpButton.Position = UDim2.new(0.1, 0, 0.75, 0)
jumpButton.Text = "Jump High"
jumpButton.BackgroundColor3 = Color3.fromRGB(255, 165, 0)
jumpButton.TextColor3 = Color3.fromRGB(255, 255, 255)
jumpButton.Parent = hubFrame

-- Button: Get Tools
local toolsButton = Instance.new("TextButton")
toolsButton.Size = UDim2.new(0.8, 0, 0.1, 0)
toolsButton.Position = UDim2.new(0.1, 0, 0.9, 0)
toolsButton.Text = "Get Tools"
toolsButton.BackgroundColor3 = Color3.fromRGB(128, 0, 128)
toolsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toolsButton.Parent = hubFrame

-- Action for Speed Boost
speedButton.MouseButton1Click:Connect(function()
    local character = Player.Character
    if character and character:FindFirstChild("Humanoid") then
        local humanoid = character.Humanoid
        humanoid.WalkSpeed = 100  -- Set speed to 100 (default is 16)
    end
end)

-- Action for Fly
flyButton.MouseButton1Click:Connect(function()
    local character = Player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        -- Enable flying
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
        bodyVelocity.Velocity = Vector3.new(0, 50, 0)  -- Upward force
        bodyVelocity.Parent = character.HumanoidRootPart
    end
end)

-- Action for Reset Character
resetButton.MouseButton1Click:Connect(function()
    local character = Player.Character
    if character then
        -- Reset the player's character
        character:BreakJoints()
    end
end)

-- Action for Jump High
jumpButton.MouseButton1Click:Connect(function()
    local character = Player.Character
    if character and character:FindFirstChild("Humanoid") then
        local humanoid = character.Humanoid
        humanoid.JumpPower = 200  -- Set jump power to 200 (default is 50)
    end
end)

-- Action for Get Tools
toolsButton.MouseButton1Click:Connect(function()
    local tools = {"Tool1", "Tool2", "Tool3"}  -- List of tools to give
    for _, toolName in ipairs(tools) do
        local tool = game.ServerStorage:FindFirstChild(toolName)
        if tool then
            local clone = tool:Clone()
            clone.Parent = Player.Backpack
        end
    end
end)

-- Create a button to open and close the hub menu
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0.2, 0, 0.1, 0)
toggleButton.Position = UDim2.new(0.1, 0, 0.9, 0)
toggleButton.Text = "Open Hub"
toggleButton.BackgroundColor3 = Color3.fromRGB(255, 165, 0)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Parent = ScreenGui

-- Action to open or close the hub when clicked
toggleButton.MouseButton1Click:Connect(function()
    if hubFrame.Visible then
        hubFrame.Visible = false
        toggleButton.Text = "Open Hub"  -- Change the button text to "Open Hub"
    else
        hubFrame.Visible = true
        toggleButton.Text = "Close Hub"  -- Change the button text to "Close Hub"
    end
end)

-- Make the Hub Menu draggable
local dragging = false
local dragStart = nil
local startPos = nil

hubFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = hubFrame.Position
    end
end)

hubFrame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        hubFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

hubFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
