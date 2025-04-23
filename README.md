-- OL_TrapGiggle.Lua
-- Made by OoOoA_lemon
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:FindFirstChild("Humanoid")

local ToolEmpty = game:GetObjects("rbxassetid://90916518275398")[1]
ToolEmpty.Parent = Player.Backpack
ToolEmpty.RequiresHandle = true

local AnimationEmpty = Instance.new("Animation")
AnimationEmpty.AnimationId = "rbxassetid://10479585177"
AnimationEmpty.Name = "IdleEmpty"

local IdleEmpty

ToolEmpty.Equipped:Connect(function()
    local Humanoid = Character:FindFirstChild("Humanoid")
    if Humanoid and Humanoid:FindFirstChild("Animator") then
        IdleEmpty = Humanoid.Animator:LoadAnimation(AnimationEmpty)
        IdleEmpty.Looped = true
        IdleEmpty:Play()
    end
end)

ToolEmpty.Unequipped:Connect(function()
    if IdleEmpty then
        IdleEmpty:Stop()
        IdleEmpty = nil
    end
end)

local function ReplaceTool(targetModel)
    if not targetModel then return end
    
    ToolEmpty:Destroy()
    targetModel:Destroy()
    
    local Tool = game:GetObjects("rbxassetid://112311666444334")[1]
    Tool.Parent = Player.Backpack
    Tool.RequiresHandle = true
    
    local Animation = Instance.new("Animation")
    Animation.AnimationId = "rbxassetid://10479585177"
    
    local AnimationTrack
    local Sound = Instance.new("Sound")
Sound.Parent = Tool
Sound.SoundId = "rbxassetid://17812530874"
Sound.Volume = 0.5
Sound.Looped = true
    
    if Sound then
        Sound.Looped = true
    end
    
    Tool.Equipped:Connect(function()
        if Humanoid then
            AnimationTrack = Humanoid:LoadAnimation(Animation)
            AnimationTrack.Looped = true
            AnimationTrack:Play()
        end
        if Sound then
            Sound:Play()
        end
    end)
    
    Tool.Unequipped:Connect(function()
        if AnimationTrack then
            AnimationTrack:Stop()
            AnimationTrack = nil
        end
        if Sound then
            Sound:Stop()
        end
    end)
end

workspace.ChildAdded:Connect(function(child)
    if child:IsA("Model") and child.Name:find("GiggleRagdoll") then
        local ClickDetector = Instance.new("ClickDetector")
        ClickDetector.Parent = child
        ClickDetector.MaxActivationDistance = 10

        ClickDetector.MouseClick:Connect(function(player)
            if player == Player and Player.Backpack:FindFirstChild(ToolEmpty.Name) then
                ReplaceTool(child)
            end
        end)
    end
end)
