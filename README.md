-- CriaÃ§Ã£o da GUI
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local TitleLabel = Instance.new("TextLabel")
local ActivateButton = Instance.new("TextButton")
local DeactivateButton = Instance.new("TextButton")
local DestroyButton = Instance.new("TextButton")
local MusicButton = Instance.new("TextButton") -- BotÃ£o para tocar mÃºsica
local StopMusicButton = Instance.new("TextButton") -- Novo botÃ£o para parar a mÃºsica
local ToggleButton = Instance.new("TextButton") -- BotÃ£o para alternar visibilidade

ScreenGui.Parent = game.CoreGui
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.new(0.5, 0.25, 0)
Frame.Size = UDim2.new(0, 200, 0, 210) -- Aumentar altura para acomodar novos botÃµes
Frame.Position = UDim2.new(0.5, -100, 0.5, -105)
Frame.Active = true
Frame.Draggable = true

TitleLabel.Parent = Frame
TitleLabel.Size = UDim2.new(1, 0, 0, 30)
TitleLabel.Position = UDim2.new(0, 0, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "ðŸŽ© ESP ðŸŽ©"
TitleLabel.TextColor3 = Color3.new(1, 1, 1)
TitleLabel.TextScaled = true

ActivateButton.Parent = Frame
ActivateButton.BackgroundColor3 = Color3.new(0, 1, 0)
ActivateButton.Size = UDim2.new(0, 180, 0, 30)
ActivateButton.Position = UDim2.new(0, 10, 0, 40)
ActivateButton.Text = "Ativar ESP"

DeactivateButton.Parent = Frame
DeactivateButton.BackgroundColor3 = Color3.new(0, 1, 0)
DeactivateButton.Size = UDim2.new(0, 180, 0, 30)
DeactivateButton.Position = UDim2.new(0, 10, 0, 80)
DeactivateButton.Text = "Desativar ESP"

MusicButton.Parent = Frame -- Adicionar botÃ£o para tocar mÃºsica
MusicButton.BackgroundColor3 = Color3.new(0, 0, 1)
MusicButton.Size = UDim2.new(0, 180, 0, 30)
MusicButton.Position = UDim2.new(0, 10, 0, 120)
MusicButton.Text = "Tocar MÃºsica"

StopMusicButton.Parent = Frame -- Adicionar botÃ£o para parar mÃºsica
StopMusicButton.BackgroundColor3 = Color3.new(1, 0, 1)
StopMusicButton.Size = UDim2.new(0, 180, 0, 30)
StopMusicButton.Position = UDim2.new(0, 10, 0, 160)
StopMusicButton.Text = "Parar MÃºsica"

DestroyButton.Parent = Frame
DestroyButton.BackgroundColor3 = Color3.new(1, 0, 0)
DestroyButton.Size = UDim2.new(0, 180, 0, 30)
DestroyButton.Position = UDim2.new(0, 10, 0, 200)
DestroyButton.Text = "DESTRUIR GUI"

-- Adicionar botÃ£o para alternar visibilidade da GUI
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.new(1, 1, 1)
ToggleButton.Size = UDim2.new(0, 50, 0, 50)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Text = "GUI"
ToggleButton.Draggable = true

local guiVisible = true

-- FunÃ§Ã£o para alternar visibilidade da GUI
local function toggleGUI()
    guiVisible = not guiVisible
    Frame.Visible = guiVisible
end

ToggleButton.MouseButton1Click:Connect(toggleGUI)

-- FunÃ§Ã£o para tocar mÃºsica
local sound = Instance.new("Sound", game.Workspace)
sound.SoundId = "rbxassetid://1845756489"

local function playMusic()
    sound:Play()
end

-- FunÃ§Ã£o para parar mÃºsica
local function stopMusic()
    sound:Stop()
end

-- FunÃ§Ã£o para criar ESP
local function createESP(player)
    local espLabel = Instance.new("BillboardGui")
    espLabel.Name = "ESPLabel"
    espLabel.Adornee = player.Character:FindFirstChild("HumanoidRootPart")
    espLabel.Size = UDim2.new(0, 100, 0, 50)
    espLabel.AlwaysOnTop = true
    espLabel.StudsOffset = Vector3.new(0, 2, 0)
    espLabel.Parent = player.Character:FindFirstChild("HumanoidRootPart")

    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = espLabel
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.new(1, 1, 1)
    textLabel.TextScaled = true
    textLabel.TextWrapped = true -- Ajustar tamanho do texto

    local function updateLabel(status)
        textLabel.Text = player.Name .. " - stats: " .. status
    end

    local function updateESP()
        local status = "normal"
        local color = Color3.new(0, 1, 0)
        if player.Character.Humanoid.Sit then
            status = "Sitting"
            color = Color3.new(0, 1, 1)
        elseif player.Character:FindFirstChild("noclip") then
            status = "noclipando"
            color = Color3.new(0.5, 0.25, 0) -- Cor marrom para noclip
        end
        for _, part in pairs(player.Character:GetChildren()) do
            if part:IsA("BasePart") then
                local espBox = part:FindFirstChild("ESP")
                if not espBox then
                    espBox = Instance.new("BoxHandleAdornment")
                    espBox.Name = "ESP"
                    espBox.Adornee = part
                    espBox.Size = part.Size
                    espBox.Transparency = 0.5
                    espBox.AlwaysOnTop = true
                    espBox.ZIndex = 10
                    espBox.Parent = part
                end
                espBox.Color3 = color
            end
        end
        updateLabel(status)
    end

    updateESP()
    
    player.Character.Humanoid:GetPropertyChangedSignal("Sit"):Connect(updateESP)
    player.Character.Humanoid.StateChanged:Connect(updateESP)
    player.Character.HumanoidRootPart:GetPropertyChangedSignal("Velocity"):Connect(updateESP)
    player.Character.ChildAdded:Connect(function(child)
        if child.Name == "noclip" then
            updateESP()
        end
    end)
end

-- FunÃ§Ã£o para ativar ESP
local function activateESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            createESP(player)
        end
    end
end

-- FunÃ§Ã£o para desativar ESP
local function deactivateESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character:FindFirstChild("HumanoidRootPart") and player.Character.HumanoidRootPart:FindFirstChild("ESPLabel") then
            player.Character.HumanoidRootPart.ESPLabel:Destroy()
        end
        for _, part in pairs(player.Character:GetChildren()) do
            if part:IsA("BasePart") and part:FindFirstChild("ESP") then
                part.ESP:Destroy()
            end
        end
    end
end

-- FunÃ§Ã£o para destruir a GUI
local function destroyGUI()
    ScreenGui:Destroy()
end

ActivateButton.MouseButton1Click:Connect(activateESP)
DeactivateButton.MouseButton1Click:Connect(deactivateESP)
DestroyButton.MouseButton1Click:Connect(destroyGUI)
MusicButton.MouseButton1Click:Connect(playMusic) -- Conectar botÃ£o Ã  funÃ§Ã£o de tocar mÃºsica
StopMusicButton.MouseButton1Click:Connect(stopMusic) -- Conectar botÃ£o Ã  funÃ§Ã£o de parar mÃºsica
