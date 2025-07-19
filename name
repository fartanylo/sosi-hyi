-- // СКРИПТ ДЛЯ COUNTER BLOX С GUI, AIMBOT, WALLHACK, BHOP, TRIGGERBOT // --

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local TweenService = game:GetService("TweenService")

-- // Глобальные настройки
getgenv().WallhackEnabled = false
getgenv().AimbotEnabled = false
getgenv().BhopEnabled = false
getgenv().ThirdPersonEnabled = false
getgenv().TriggerbotEnabled = false
getgenv().SkinChangerEnabled = false
getgenv().TriggerbotDelay = 0.05 -- по умолчанию 50 мс
getgenv().HeadHitboxSize = 5 -- начальный размер хитбокса головы


-- // Переменные
local RightMouseDown = false
-- Отслеживание ПКМ для активации Aimbot
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if input.UserInputType == Enum.UserInputType.MouseButton2 then
		RightMouseDown = true
	end
end)

UserInputService.InputEnded:Connect(function(input, gameProcessed)
	if input.UserInputType == Enum.UserInputType.MouseButton2 then
		RightMouseDown = false
	end
end)

local bhopConnection = nil
local thirdPersonConn = nil

--// Wallhack Script with Healthbar //

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

getgenv().WallhackEnabled = false

local function isEnemy(plr)
    if plr.Team ~= nil and LocalPlayer.Team ~= nil then
        return plr.Team ~= LocalPlayer.Team
    end
    return true
end

local function clearHighlights()
    for _, plr in pairs(Players:GetPlayers()) do
        if plr.Character then
            local highlight = plr.Character:FindFirstChild("WH_Highlight")
            if highlight then
                highlight:Destroy()
            end
            local head = plr.Character:FindFirstChild("HumanoidRootPart")
            if head then
                local gui = head:FindFirstChild("WH_HealthGui")
                if gui then gui:Destroy() end
            end
            local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local light = hrp:FindFirstChild("WH_Light")
                if light then light:Destroy() end
            end
        end
    end
end

local function createHealthDisplay(char)
    local head = char:FindFirstChild("Head")
    if not head then return end

    if head:FindFirstChild("WH_HealthGui") then return end

    local billboard = Instance.new("BillboardGui")
    billboard.Name = "WH_HealthGui"
    billboard.Adornee = head
    billboard.StudsOffset = Vector3.new(3, 0, 0)
    billboard.Size = UDim2.new(4, 0, 0.5, 0)
    billboard.AlwaysOnTop = true
    billboard.Parent = head

    local barBackground = Instance.new("Frame")
    barBackground.Size = UDim2.new(1, 0, 0.3, 0)
    barBackground.Position = UDim2.new(0, 0, 0.35, 0)
    barBackground.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    barBackground.BorderSizePixel = 1
    barBackground.BorderColor3 = Color3.fromRGB(255, 255, 255)
    barBackground.Parent = billboard

    local healthBar = Instance.new("Frame")
    healthBar.Name = "HPFill"
    healthBar.Size = UDim2.new(1, 0, 1, 0)
    healthBar.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    healthBar.BorderSizePixel = 0
    healthBar.Parent = barBackground

    local uicorner = Instance.new("UICorner")
    uicorner.CornerRadius = UDim.new(0, 4)
    uicorner.Parent = barBackground
end

local function createHighlight(char)
    if not char:FindFirstChild("WH_Highlight") then
        local hl = Instance.new("Highlight")
        hl.Name = "WH_Highlight"
        hl.FillColor = Color3.new(1, 0, 0)
        hl.OutlineColor = Color3.new(1, 1, 1)
        hl.FillTransparency = 0.25
        hl.OutlineTransparency = 0
        hl.Adornee = char
        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        hl.Parent = char
    end

    local hrp = char:FindFirstChild("HumanoidRootPart")
    if hrp and not hrp:FindFirstChild("WH_Light") then
        local light = Instance.new("PointLight")
        light.Name = "WH_Light"
        light.Color = Color3.new(1, 0, 0)
        light.Brightness = 5
        light.Range = 15
        light.Shadows = true
        light.Parent = hrp
    end
end

local function updateWallhack()
    clearHighlights()
    if not getgenv().WallhackEnabled then return end
    for _, plr in pairs(Players:GetPlayers()) do
        if plr.Character and isEnemy(plr) then
            createHighlight(plr.Character)
            createHealthDisplay(plr.Character)
        end
    end
end

RunService.RenderStepped:Connect(function()
    if getgenv().WallhackEnabled then
        for _, plr in pairs(Players:GetPlayers()) do
            if plr.Character and isEnemy(plr) then
                local hum = plr.Character:FindFirstChildOfClass("Humanoid")
                local head = plr.Character:FindFirstChild("Head")
                if hum and head then
                    local hpGui = head:FindFirstChild("WH_HealthGui")
                    if hpGui then
                        local barBackground = hpGui:FindFirstChildOfClass("Frame")
                        if barBackground then
                            local hpBar = barBackground:FindFirstChild("HPFill")
                            if hpBar then
                                local targetScale = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
local currentScale = hpBar.Size.X.Scale
local newScale = currentScale + (targetScale - currentScale) * 0.15
hpBar.Size = UDim2.new(newScale, 0, 1, 0)

                                if hum.Health / hum.MaxHealth < 0.3 then
                                    hpBar.BackgroundColor3 = Color3.fromRGB(255, 85, 81)
                                elseif hum.Health / hum.MaxHealth < 0.6 then
                                    hpBar.BackgroundColor3 = Color3.fromRGB(255, 155, 75)
                                else
                                hpBar.BackgroundColor3 = Color3.fromRGB(106, 255, 89) -- красивый зелёный (#32FF79)
                                end
                            end
                        end
                    end
                end
            end
        end
    end
end)

local function setupCharacterTracking(plr)
    plr.CharacterAdded:Connect(function()
        task.wait(0.5)
        updateWallhack()
    end)
end

Players.PlayerAdded:Connect(setupCharacterTracking)

for _, plr in pairs(Players:GetPlayers()) do
    setupCharacterTracking(plr)
end

LocalPlayer:GetPropertyChangedSignal("Team"):Connect(updateWallhack)

updateWallhack()

-- Автоматическое обновление Wallhack каждые 3 секунды
task.spawn(function()
    while true do
        if getgenv().WallhackEnabled then
            updateWallhack()
        end
        task.wait(2)
    end
end)



-- // Aimbot
RunService.RenderStepped:Connect(function()
    if not getgenv().AimbotEnabled or not RightMouseDown then return end
    local bestTarget = nil
    local lowestDist = math.huge
    local screenCenter = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

    for _, plr in pairs(Players:GetPlayers()) do
        if plr.Character and isEnemy(plr) then
            local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local screenPos, onScreen = Camera:WorldToViewportPoint(hrp.Position)
                if onScreen then
                    local dist = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
                    if dist < lowestDist then
                        lowestDist = dist
                        bestTarget = hrp.Position
                    end
                end
            end
        end
    end

    if bestTarget then
        Camera.CFrame = CFrame.new(Camera.CFrame.Position, bestTarget)
    end
end)

-- // Triggerbot (точный и с повторной проверкой)
local indicatorText = indicatorText -- переиспользуем существующий
local targetHeldFrames = 0
local requiredFrames = 4  -- кол-во кадров, сколько цель должна быть под прицелом

RunService.RenderStepped:Connect(function()
	if not getgenv().TriggerbotEnabled then
		targetHeldFrames = 0
		return
	end

	local ray = Camera:ViewportPointToRay(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
	local rayParams = RaycastParams.new()
	rayParams.FilterType = Enum.RaycastFilterType.Blacklist
	rayParams.FilterDescendantsInstances = {LocalPlayer.Character}
	rayParams.IgnoreWater = true

	local result = workspace:Raycast(ray.Origin, ray.Direction * 1000, rayParams)

	if result and result.Instance then
		local part = result.Instance
		local character = part:FindFirstAncestorOfClass("Model")
		local player = character and Players:GetPlayerFromCharacter(character)

		if player and isEnemy(player) and character:FindFirstChild("Humanoid") and character:FindFirstChild("Head") then
			targetHeldFrames += 1
			if targetHeldFrames >= requiredFrames then
				if indicatorText then
					indicatorText.Text = "TRIGGERED!"
				end
				mouse1press()
				task.wait(getgenv().TriggerbotDelay)
				mouse1release()
				task.delay(0.2, function()
					if indicatorText then
						indicatorText.Text = ""
					end
				end)
				targetHeldFrames = 0
			end
		else
			targetHeldFrames = 0
		end
	else
		targetHeldFrames = 0
	end
end)


-- // BunnyHop
local function toggleBhop(state)
    if bhopConnection then
        bhopConnection:Disconnect()
        bhopConnection = nil
    end

    if state then
        bhopConnection = RunService.RenderStepped:Connect(function()
            local char = LocalPlayer.Character
            if not char then return end
            local hum = char:FindFirstChildOfClass("Humanoid")
            if not hum then return end
            if hum:GetState() == Enum.HumanoidStateType.Running and UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                hum.Jump = true
            end
        end)
    end
end

-- // Third Person (исправлено с управлением камерой)
local camDistance = 6
local camHeight = 2
local camSensitivity = 0.3
local camRotX, camRotY = 0, 0
local renderConn, inputConn

local function toggleThirdPerson(state)
    if renderConn then renderConn:Disconnect() end
    if inputConn then inputConn:Disconnect() end

    local char = LocalPlayer.Character
    if not char then return end

    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end

    if not state then
        Camera.CameraType = Enum.CameraType.Custom
        Camera.CameraSubject = humanoid
        return
    end

    Camera.CameraType = Enum.CameraType.Custom
    Camera.CameraSubject = humanoid

    inputConn = UserInputService.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            camRotY = camRotY - input.Delta.X * camSensitivity
            camRotX = math.clamp(camRotX - input.Delta.Y * camSensitivity, -80, 80)
        end
    end)

    renderConn = RunService.RenderStepped:Connect(function()
        local root = humanoid.RootPart
        if not root then return end

        local startPos = root.Position + Vector3.new(0, camHeight, 0)
        local offset = CFrame.Angles(0, math.rad(camRotY), 0) * CFrame.Angles(math.rad(camRotX), 0, 0)
        local camPos = startPos - offset.LookVector * camDistance

        Camera.CFrame = CFrame.new(camPos, startPos)
    end)
end
-- // GUI ИНТЕРФЕЙС
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "CBX_CustomGUI"

local frame = Instance.new("Frame", ScreenGui)
frame.Size = UDim2.new(0, 400, 0, 600)
frame.Position = UDim2.new(0, 100, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.BorderSizePixel = 1
frame.Active = true
frame.Draggable = true

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.RightControl then
        Frame.Visible = not Frame.Visible
    end
end)


Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 8)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "CBX Custom GUI"
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1
title.TextSize = 18

local UIListLayout = Instance.new("UIListLayout", frame)
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.FillDirection = Enum.FillDirection.Vertical
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Top

local function makeToggle(name, globalVar, callback)
	local toggle = Instance.new("TextButton", frame)
	toggle.Size = UDim2.new(0, 220, 0, 30)
	toggle.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
	toggle.Font = Enum.Font.Gotham
	toggle.TextSize = 14
	toggle.Text = "[ OFF ] " .. name
	toggle.AutoButtonColor = false

	Instance.new("UICorner", toggle).CornerRadius = UDim.new(0, 6)
	Instance.new("UIStroke", toggle).Thickness = 1

	getgenv()[globalVar] = false

	toggle.MouseButton1Click:Connect(function()
		getgenv()[globalVar] = not getgenv()[globalVar]
		toggle.Text = (getgenv()[globalVar] and "[ ON  ] " or "[ OFF ] ") .. name
		if callback then
			callback(getgenv()[globalVar])
		end
	end)
end

makeToggle("Wallhack", "WallhackEnabled")
makeToggle("Aimbot", "AimbotEnabled")
makeToggle("BunnyHop", "BhopEnabled", toggleBhop)
makeToggle("3rd Person View", "ThirdPersonEnabled", toggleThirdPerson)
makeToggle("Triggerbot", "TriggerbotEnabled")

-- // Triggerbot Delay Slider
local delayLabel = Instance.new("TextLabel", frame)
delayLabel.Size = UDim2.new(0, 220, 0, 20)
delayLabel.BackgroundTransparency = 1
delayLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
delayLabel.Font = Enum.Font.Gotham
delayLabel.TextSize = 14
delayLabel.Text = "Triggerbot Delay: " .. tostring(math.floor(getgenv().TriggerbotDelay * 1000)) .. " ms"

local sliderFrame = Instance.new("Frame", frame)
sliderFrame.Size = UDim2.new(0, 220, 0, 6)
sliderFrame.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
sliderFrame.BorderSizePixel = 0
Instance.new("UICorner", sliderFrame).CornerRadius = UDim.new(0, 3)

local fill = Instance.new("Frame", sliderFrame)
fill.Size = UDim2.new(getgenv().TriggerbotDelay, 0, 1, 0)
fill.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
fill.BorderSizePixel = 0
fill.Name = "Fill"
Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 3)
fill.Parent = sliderFrame

local dragging = false

local function updateSlider(inputX)
	local relativeX = math.clamp((inputX - sliderFrame.AbsolutePosition.X) / sliderFrame.AbsoluteSize.X, 0.01, 1)
	getgenv().TriggerbotDelay = relativeX
	fill.Size = UDim2.new(relativeX, 0, 1, 0)
	delayLabel.Text = "Triggerbot Delay: " .. tostring(math.floor(relativeX * 1000)) .. " ms"
end

sliderFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		updateSlider(input.Position.X)
	end
end)

sliderFrame.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		updateSlider(input.Position.X)
	end
end)

-- // Triggerbot индикатор
indicatorText = Instance.new("TextLabel", frame)
indicatorText.Size = UDim2.new(0, 220, 0, 20)
indicatorText.BackgroundTransparency = 1
indicatorText.TextColor3 = Color3.fromRGB(255, 100, 100)
indicatorText.Font = Enum.Font.GothamBold
indicatorText.TextSize = 14
indicatorText.Text = ""

-- // Skin Changer Button
local skinBtn = Instance.new("TextButton", frame)
skinBtn.Size = UDim2.new(0, 220, 0, 30)
skinBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
skinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
skinBtn.Font = Enum.Font.Gotham
skinBtn.TextSize = 14
skinBtn.Text = "[ ► ] Launch Skin Changer"
skinBtn.AutoButtonColor = false

Instance.new("UICorner", skinBtn).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", skinBtn).Thickness = 1

local skinLoaded = false
skinBtn.MouseButton1Click:Connect(function()
	if skinLoaded then return end
	skinLoaded = true

	task.spawn(function()
		local success, result = pcall(function()
			local script_key = "KEY HERE"
			local loaderUrl = "https://api.luarmor.net/files/v3/loaders/62cee44fa4b2a83752a4ea9e8eef7081.lua"
			local loaderCode = game:HttpGet(loaderUrl)
			loadstring(loaderCode)()
		end)

		if not success then
			warn("[SkinChanger] Ошибка загрузки:", result)
			skinBtn.Text = "[ × ] Load Failed"
			skinBtn.TextColor3 = Color3.fromRGB(255, 80, 80)
		else
			skinBtn.Text = "[ ✔ ] Loaded"
			skinBtn.TextColor3 = Color3.fromRGB(80, 255, 100)
		end
	end)
end)

-- // No Recoil Button
local externalBtn = Instance.new("TextButton", frame)
externalBtn.Size = UDim2.new(0, 220, 0, 30)
externalBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
externalBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
externalBtn.Font = Enum.Font.Gotham
externalBtn.TextSize = 14
externalBtn.Text = "[ OFF ] No Recoil"
externalBtn.AutoButtonColor = false

Instance.new("UICorner", externalBtn).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", externalBtn).Thickness = 1

-- // No Recoil Toggle Logic
local noRecoilEnabled = false
local originalSpreadValues = {}

externalBtn.MouseButton1Click:Connect(function()
	noRecoilEnabled = not noRecoilEnabled

	local rs = game:GetService("ReplicatedStorage")
	local weaponsFolder = rs:FindFirstChild("Weapons")

	if weaponsFolder then
		for _, weapon in pairs(weaponsFolder:GetChildren()) do
			local spreadFolder = weapon:FindFirstChild("Spread")
			if spreadFolder then
				for _, val in pairs(spreadFolder:GetChildren()) do
					if val:IsA("NumberValue") then
						local key = weapon.Name .. "/" .. val.Name
						if noRecoilEnabled then
							if originalSpreadValues[key] == nil then
								originalSpreadValues[key] = val.Value
							end
							val.Value = 0
						else
							if originalSpreadValues[key] then
								val.Value = originalSpreadValues[key]
							end
						end
					end
				end
			end
		end
	end

	if noRecoilEnabled then
		externalBtn.Text = "[ ✔ ] No Recoil ON"
		externalBtn.TextColor3 = Color3.fromRGB(80, 255, 100)
	else
		externalBtn.Text = "[ OFF ] No Recoil"
		externalBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	end
end)

