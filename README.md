-- Aimbot Educativo - Para jogos próprios
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local localPlayer = Players.LocalPlayer
local mouse = localPlayer:GetMouse()
local camera = workspace.CurrentCamera
local maxDistance = 150

local function getClosestTarget()
	local closestPlayer = nil
	local shortestDistance = maxDistance

	for _, otherPlayer in pairs(Players:GetPlayers()) do
		if otherPlayer ~= localPlayer and otherPlayer.Character and otherPlayer.Character:FindFirstChild("Head") then
			local head = otherPlayer.Character.Head
			local screenPoint, onScreen = camera:WorldToViewportPoint(head.Position)

			if onScreen then
				local dist = (Vector2.new(mouse.X, mouse.Y) - Vector2.new(screenPoint.X, screenPoint.Y)).Magnitude
				if dist < shortestDistance then
					shortestDistance = dist
					closestPlayer = head
				end
			end
		end
	end

	return closestPlayer
end

RunService.RenderStepped:Connect(function()
	if UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
		local target = getClosestTarget()
		if target then
			camera.CFrame = CFrame.new(camera.CFrame.Position, target.Position)
		end
	end
end)
