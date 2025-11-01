-- This is a LocalScript. Place it in StarterPlayer > StarterPlayerScripts in Roblox Studio.
-- It highlights all players in the game and ensures their name tags are visible (they are by default, but this reinforces it).

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Function to highlight a player's character
local function highlightPlayer(player)
    if player == LocalPlayer then return end -- Skip highlighting yourself if desired
    local character = player.Character
    if character then
        -- Create a Highlight instance
        local highlight = Instance.new("Highlight")
        highlight.Adornee = character -- Highlights the entire character model
        highlight.FillColor = Color3.fromRGB(255, 0, 0) -- Red highlight; change as needed
        highlight.FillTransparency = 0.5 -- Semi-transparent
        highlight.OutlineColor = Color3.fromRGB(255, 255, 255) -- White outline
        highlight.OutlineTransparency = 0
        highlight.Parent = character
        
        -- Ensure name tag is visible (it's usually on by default)
        local humanoid = character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.DisplayName = player.Name -- Forces display name to match username
        end
    end
end

-- Highlight existing players
for _, player in ipairs(Players:GetPlayers()) do
    highlightPlayer(player)
end

-- Highlight new players when they join
Players.PlayerAdded:Connect(highlightPlayer)

-- Optional: Remove highlight when a player leaves (to clean up)
Players.PlayerRemoving:Connect(function(player)
    local character = player.Character
    if character then
        local highlight = character:FindFirstChild("Highlight")
        if highlight then
            highlight:Destroy()
        end
    end
end)
