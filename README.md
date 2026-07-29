-- Configurações Iniciais
local BodySize = 50
local TransparencyValue = 0.6
local Enabled = true

-- Criando a Interface (GUI)
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local ToggleBtn = Instance.new("TextButton")
local SizeInput = Instance.new("TextBox")
local SizeLabel = Instance.new("TextLabel")
local TranspInput = Instance.new("TextBox")
local TranspLabel = Instance.new("TextLabel")

ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.Name = "BodyHitboxExpanderGUI"

MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.05, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 200, 0, 220)
MainFrame.Active = true
MainFrame.Draggable = true

Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "LK+RONI HB"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16

ToggleBtn.Parent = MainFrame
ToggleBtn.Position = UDim2.new(0.1, 0, 0.2, 0)
ToggleBtn.Size = UDim2.new(0.8, 0, 0, 35)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
ToggleBtn.Font = Enum.Font.SourceSansBold
ToggleBtn.Text = "Status: ATIVADO"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.TextSize = 14

SizeLabel.Parent = MainFrame
SizeLabel.Position = UDim2.new(0.1, 0, 0.4, 0)
SizeLabel.Size = UDim2.new(0.8, 0, 0, 20)
SizeLabel.BackgroundTransparency = 1
SizeLabel.Font = Enum.Font.SourceSans
SizeLabel.Text = "Tamanho do Torso:"
SizeLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
SizeLabel.TextSize = 14

SizeInput.Parent = MainFrame
SizeInput.Position = UDim2.new(0.1, 0, 0.5, 0)
SizeInput.Size = UDim2.new(0.8, 0, 0, 30)
SizeInput.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SizeInput.Font = Enum.Font.SourceSans
SizeInput.Text = tostring(BodySize)
SizeInput.TextColor3 = Color3.fromRGB(255, 255, 255)
SizeInput.TextSize = 14

TranspLabel.Parent = MainFrame
TranspLabel.Position = UDim2.new(0.1, 0, 0.68, 0)
TranspLabel.Size = UDim2.new(0.8, 0, 0, 20)
TranspLabel.BackgroundTransparency = 1
TranspLabel.Font = Enum.Font.SourceSans
TranspLabel.Text = "Transparência (0 a 1):"
TranspLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
TranspLabel.TextSize = 14

TranspInput.Parent = MainFrame
TranspInput.Position = UDim2.new(0.1, 0, 0.78, 0)
TranspInput.Size = UDim2.new(0.8, 0, 0, 30)
TranspInput.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
TranspInput.Font = Enum.Font.SourceSans
TranspInput.Text = tostring(TransparencyValue)
TranspInput.TextColor3 = Color3.fromRGB(255, 255, 255)
TranspInput.TextSize = 14

ToggleBtn.MouseButton1Click:Connect(function()
    Enabled = not Enabled
    if Enabled then
        ToggleBtn.Text = "Status: ATIVADO"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
    else
        ToggleBtn.Text = "Status: DESATIVADO"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
        
        for _, player in pairs(game:GetService('Players'):GetPlayers()) do
            if player.Character then
                local rootPart = player.Character:FindFirstChild("HumanoidRootPart")
                if rootPart then
                    rootPart.Size = Vector3.new(2, 2, 1)
                    rootPart.Transparency = 1
                end
            end
        end
    end
end)

SizeInput.FocusLost:Connect(function()
    local num = tonumber(SizeInput.Text)
    if num then BodySize = num else SizeInput.Text = tostring(BodySize) end
end)

TranspInput.FocusLost:Connect(function()
    local num = tonumber(TranspInput.Text)
    if num then TransparencyValue = math.clamp(num, 0, 1) else TranspInput.Text = tostring(TransparencyValue) end
end)

-- Loop principal mantendo o funcionamento e o dano com a nova identificação
game:GetService('RunService').RenderStepped:Connect(function()
    if not Enabled then return end
    
    for _, player in pairs(game:GetService('Players'):GetPlayers()) do
        if player ~= game:GetService('Players').LocalPlayer and player.Character then
            pcall(function()
                local rootPart = player.Character:FindFirstChild("HumanoidRootPart")
                local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
                
                if rootPart and humanoid then
                    rootPart.Size = Vector3.new(BodySize, BodySize, BodySize)
                    rootPart.Transparency = TransparencyValue
                    rootPart.Color = Color3.fromRGB(255, 0, 0)
                    rootPart.Material = Enum.Material.Neon
                    rootPart.CanCollide = false
                    rootPart.CanQuery = true
                    rootPart.CanTouch = true
                    rootPart.Massless = true
                end
            end)
        end
    end
end)
