local lp = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", lp.PlayerGui)
gui.Name = "BayPenguenPanel"
gui.ResetOnSpawn = false

local tgl = Instance.new("TextButton", gui)
tgl.Size = UDim2.new(0, 65, 0, 65)
tgl.Position = UDim2.new(0, 15, 0.5, -30)
tgl.Text = "🐧"
tgl.TextSize = 35
tgl.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
tgl.TextColor3 = Color3.fromRGB(255, 255, 255)
tgl.Active = true
tgl.Draggable = true
Instance.new("UICorner", tgl).CornerRadius = UDim.new(1, 0)
local stroke = Instance.new("UIStroke", tgl)
stroke.Color = Color3.fromRGB(255, 255, 255)
stroke.Thickness = 2

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 350, 0, 400)
main.Position = UDim2.new(0.5, -175, 0.5, -200)
main.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
main.Visible = false
main.Active = true
main.Draggable = true
Instance.new("UICorner", main)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 50)
title.Text = "BAY PENGUEN"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 50)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 24
Instance.new("UICorner", title)

local container = Instance.new("ScrollingFrame", main)
container.Size = UDim2.new(1, -20, 1, -70)
container.Position = UDim2.new(0, 10, 0, 60)
container.BackgroundTransparency = 1
container.CanvasSize = UDim2.new(0, 0, 3, 0)
container.ScrollBarThickness = 2
Instance.new("UIListLayout", container).Padding = UDim.new(0, 10)

local function createBtn(txt, color, func)
    local b = Instance.new("TextButton", container)
    b.Size = UDim2.new(1, 0, 0, 45)
    b.Text = txt
    b.BackgroundColor3 = color
    b.TextColor3 = Color3.fromRGB(255, 255, 255)
    b.Font = Enum.Font.SourceSansBold
    b.TextSize = 17
    Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(func)
    return b
end

task.spawn(function()
    while true do
        pcall(function()
            if lp.Character and lp.Character:FindFirstChild("Humanoid") then
                lp.Character.Humanoid.WalkSpeed = 300
            end
        end)
        task.wait(0.5)
    end
end)

local platActive = false
local plat = nil
createBtn("PLATFORM (ON/OFF)", Color3.fromRGB(0, 120, 200), function()
    platActive = not platActive
    if platActive then
        plat = Instance.new("Part", workspace)
        plat.Size = Vector3.new(15, 1, 15)
        plat.Anchored = true
        plat.Transparency = 0.6
        plat.BrickColor = BrickColor.new("Cyan")
        task.spawn(function()
            while platActive and task.wait() do
                if lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then
                    plat.CFrame = lp.Character.HumanoidRootPart.CFrame * CFrame.new(0, -3.5, 0)
                end
            end
            if plat then plat:Destroy() end
        end)
    else
        if plat then plat:Destroy() end
    end
end)

createBtn("SPEED 300", Color3.fromRGB(40, 150, 40), function() 
    if lp.Character and lp.Character:FindFirstChild("Humanoid") then
        lp.Character.Humanoid.WalkSpeed = 300 
    end
end)

createBtn("SUPER JUMP", Color3.fromRGB(40, 40, 150), function() 
    if lp.Character and lp.Character:FindFirstChild("Humanoid") then
        lp.Character.Humanoid.JumpPower = 150 
    end
end)

createBtn("PLAYER ESP", Color3.fromRGB(150, 40, 40), function()
    for _, p in pairs(game.Players:GetPlayers()) do
        if p ~= lp and p.Character and p.Character:FindFirstChild("Head") then
            if not p.Character.Head:FindFirstChild("BillboardGui") then
                local b = Instance.new("BillboardGui", p.Character.Head)
                b.AlwaysOnTop = true
                b.Size = UDim2.new(0, 100, 0, 50)
                local l = Instance.new("TextLabel", b)
                l.Size = UDim2.new(1, 0, 1, 0)
                l.Text = p.Name
                l.TextColor3 = Color3.new(1, 1, 1)
                l.BackgroundTransparency = 1
                l.TextStrokeTransparency = 0
            end
        end
    end
end)

local savedPos = nil
createBtn("SAVE POSITION", Color3.fromRGB(180, 130, 0), function() 
    if lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then
        savedPos = lp.Character.HumanoidRootPart.CFrame 
    end
end)

createBtn("TELEPORT TO SAVE", Color3.fromRGB(180, 50, 0), function() 
    if savedPos and lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then 
        lp.Character.HumanoidRootPart.CFrame = savedPos 
    end 
end)

tgl.MouseButton1Click:Connect(function() 
    main.Visible = not main.Visible 
end)

