-- Configuración
getgenv().AutoKill = false
getgenv().KillDelay = 1 -- segundos entre cada ciclo
getgenv().KillRadius = 100 -- 0 = sin distancia
getgenv().Creator = "тцк"

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Loop siempre activo, pero solo ejecuta cuando está activado
task.spawn(function()
    while true do
        task.wait(getgenv().KillDelay)

        if getgenv().AutoKill and getgenv().Creator == "тцк" then
            pcall(function()
                sethiddenproperty(player, "SimulationRadius", 1e9)
                sethiddenproperty(player, "MaxSimulationRadius", 1e9)
            end)

            local char = player.Character
            if not (char and char:FindFirstChild("HumanoidRootPart")) then continue end
            local hrp = char.HumanoidRootPart

            for _, d in pairs(workspace:GetDescendants()) do
                if d:IsA("Humanoid") and d.Parent ~= char then
                    local targetPlayer = Players:GetPlayerFromCharacter(d.Parent)
                    local targetHRP = d.Parent:FindFirstChild("HumanoidRootPart")
                    if not targetPlayer and d.Health > 0 and targetHRP then
                        local dist = (targetHRP.Position - hrp.Position).Magnitude
                        if getgenv().KillRadius == 0 or dist <= getgenv().KillRadius then
                            d.Health = 0
                        end
                    end
                end
            end
        end
    end
end)

-- UI
local existing = playerGui:FindFirstChild("KillUI")
if existing then existing:Destroy() end

local gui = Instance.new("ScreenGui", playerGui)
gui.Name = "KillUI"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 260, 0, 160)
frame.Position = UDim2.new(0.05, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local toggle = Instance.new("TextButton", frame)
toggle.Size = UDim2.new(1, -20, 0, 40)
toggle.Position = UDim2.new(0, 10, 0, 10)
toggle.Text = "Activar AutoKill"
toggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggle.TextColor3 = Color3.new(1, 1, 1)

toggle.MouseButton1Click:Connect(function()
    getgenv().AutoKill = not getgenv().AutoKill
    toggle.Text = getgenv().AutoKill and "Desactivar AutoKill" or "Activar AutoKill"
end)

local label = Instance.new("TextLabel", frame)
label.Size = UDim2.new(1, -20, 0, 20)
label.Position = UDim2.new(0, 10, 0, 60)
label.Text = "Radio actual: " .. getgenv().KillRadius
label.TextColor3 = Color3.new(1, 1, 1)
label.BackgroundTransparency = 1

local textBox = Instance.new("TextBox", frame)
textBox.Size = UDim2.new(1, -20, 0, 30)
textBox.Position = UDim2.new(0, 10, 0, 90)
textBox.PlaceholderText = "Nuevo radio (0 = infinito)"
textBox.Text = ""
textBox.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
textBox.TextColor3 = Color3.new(1, 1, 1)

textBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local num = tonumber(textBox.Text)
        if num and num >= 0 and num <= 1000 then
            getgenv().KillRadius = num
            label.Text = "Radio actual: " .. num
        else
            textBox.Text = ""
            textBox.PlaceholderText = "Rango válido: 0 - 1000"
        end
    end
end)
