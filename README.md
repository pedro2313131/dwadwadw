-- Vox Seas HUB
-- Criado por ChatGPT (26/07/2025)

-- Servicos
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Criar GUI Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 250)
MainFrame.Position = UDim2.new(0.05, 0, 0.2, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 10)

local Title = Instance.new("TextLabel")
Title.Text = "VOX SEAS HUB"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 20
Title.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Title.BorderSizePixel = 0
Title.Parent = MainFrame

-- Botão Auto TP Fruits
local TPBtn = Instance.new("TextButton")
TPBtn.Size = UDim2.new(0, 280, 0, 40)
TPBtn.Position = UDim2.new(0, 10, 0, 50)
TPBtn.Text = "AUTO-TP FRUITS (OFF)"
TPBtn.TextColor3 = Color3.new(1, 1, 1)
TPBtn.BackgroundColor3 = Color3.fromRGB(60, 120, 60)
TPBtn.Parent = MainFrame

local autoTP = false

TPBtn.MouseButton1Click:Connect(function()
    autoTP = not autoTP
    TPBtn.Text = autoTP and "AUTO-TP FRUITS (ON)" or "AUTO-TP FRUITS (OFF)"
end)

-- Lista de Teleports de Ilhas
local TeleportList = {
    ["Spawn"] = Vector3.new(0, 10, 0),
    ["Marine Base"] = Vector3.new(500, 10, -200),
    ["Pirate Island"] = Vector3.new(-600, 10, 400),
}

-- Cria Botões para Teleports
local yPos = 100
for name, pos in pairs(TeleportList) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 280, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, yPos)
    btn.Text = "Teleport: " .. name
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.BackgroundColor3 = Color3.fromRGB(80, 80, 200)
    btn.Parent = MainFrame

    btn.MouseButton1Click:Connect(function()
        local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        char:WaitForChild("HumanoidRootPart").CFrame = CFrame.new(pos)
    end)

    yPos = yPos + 35
end

-- Função para achar frutas
local function findFruits()
    local fruits = {}
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj.Name:lower():find("fruit") then
            table.insert(fruits, obj)
        end
    end
    return fruits
end

-- Auto TP Loop
RunService.RenderStepped:Connect(function()
    if autoTP then
        local fruits = findFruits()
        if #fruits > 0 then
            local fruit = fruits[1] -- Teleporta para a primeira fruta encontrada
            local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
            char:WaitForChild("HumanoidRootPart").CFrame = fruit.CFrame + Vector3.new(0, 3, 0)
            wait(1)
        end
    end
end)
