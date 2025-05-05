local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local AIM_RADIUS = 1000 -- raio máximo de busca
local TARGET_PART = "Head" -- parte a mirar

local function getClosestEnemy()
    local closest = nil
    local shortestDistance = AIM_RADIUS

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Team ~= LocalPlayer.Team and player.Character then
            local targetPart = player.Character:FindFirstChild(TARGET_PART)
            if targetPart and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local distance = (targetPart.Position - Camera.CFrame.Position).Magnitude
                if distance < shortestDistance then
                    closest = targetPart
                    shortestDistance = distance
                end
            end
        end
    end

    return closest
end

RunService.RenderStepped:Connect(function()
    local target = getClosestEnemy()
    if target then
        Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
    end
end)
