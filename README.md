# Toxic.Ware-
local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()
local RequiredData = { -- dont mess with the setting spls
    ["WalkSpeed"] = 16,
    ["JumpPower"] = 50,
    ["FieldOfView"] = 70,
    ["VisualData"] = {
        ["Enabled-ESP"] = false,
        ["ESP-Part"] = "",
        ["Transparency"] = 0.5 / 100,
    },
    ["AimbotData"] = {
        ["FOV"] = 60,
        ["AimPart"] = "Head",
        ["TeamCheck"] = true,
        ["Draw-FOV"] = true,
        ["FOV-Color"] = Color3.fromRGB(255, 0, 0),
        ["Yes-Toggle"] = false
    },
    ["MiscData"] = {
        ["Anti-FPS Drop"] = false
    }
}

local UI = Material.Load({
    Title = "Toxic.ware Free",
	Style = 3,
	SizeX = 500,
	SizeY = 350,
	Theme = "Dark",
})

local MainPage = UI.New({
    Title = "LocalPlayer"
})
local VisualPage = UI.New({
    Title = "Visuals Updated:)"
})
local AimbotPage = UI.New({
    Title = "Aimbot"
})
local MiscPage = UI.New({
    Title = "Misc"
})
local Note = MiscPage.Label({Text = "Anti-FPS Drop may bug Visuals."})
local AntiFPS = MiscPage.Toggle({
    Text = "Anti-FPS Drop",
    Callback = function(value)
        RequiredData.MiscData["Anti-FPS Drop"] = value
    end,
})
local Note = AimbotPage.Label({
    Text = "Please do not press more than 1 time"
})
local AimPart = AimbotPage.Dropdown({
    Text = "AimPart",
    Callback = function(value)
        RequiredData.AimbotData.AimPart = value
    end,
    Options = {"Head"}
})
local TeamCheck = AimbotPage.Toggle({
    Text = "TeamCheck",
    Callback = function(value)
        RequiredData.AimbotData.TeamCheck = value
    end,
})
local DrawFOV = AimbotPage.Toggle({
    Text = "Draw FOV",
    Callback = function(value)
        RequiredData.AimbotData["Draw-FOV"] = value
    end,
})
local FovRadius = AimbotPage.Slider({
    Text = "FOV Radius",
    Callback = function(value)
        RequiredData.AimbotData.FOV = value
    end,
    Min = 30,
    Max = 200,
    Def = 60
})
local ToggleAimbot = AimbotPage.Button({
    Text = "Aimbot",
    Callback = function(value)
        local dwCamera = workspace.CurrentCamera
local dwRunService = game:GetService("RunService")
local dwUIS = game:GetService("UserInputService")
local dwEntities = game:GetService("Players")
local dwLocalPlayer = dwEntities.LocalPlayer
local dwMouse = dwLocalPlayer:GetMouse()
RequiredData.AimbotData["Yes-Toggle"] = true
local settings = {
    Aimbot = true,
    Aiming = false,
    Aimbot_AimPart = RequiredData.AimbotData.AimPart,
    Aimbot_TeamCheck = RequiredData.AimbotData.TeamCheck,
    Aimbot_Draw_FOV = RequiredData.AimbotData["Draw-FOV"],
    Aimbot_FOV_Radius = RequiredData.AimbotData.FOV,
    Aimbot_FOV_Color = Color3.fromRGB(27, 15, 25)
}

local fovcircle = Drawing.new("Circle")
fovcircle.Color = settings.Aimbot_FOV_Color
fovcircle.Thickness = 1
fovcircle.Filled = false


fovcircle.Position = Vector2.new(dwCamera.ViewportSize.X / 2, dwCamera.ViewportSize.Y / 2)

dwUIS.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton2 then
        settings.Aiming = true
    end
end)

dwUIS.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton2 then
        settings.Aiming = false
    end
end)

dwRunService.RenderStepped:Connect(function()
    local settings2 = {
        Aimbot1 = settings.Aimbot,
        Aiming1 = settings.Aiming,
        Aimbot_AimPart1 = RequiredData.AimbotData.AimPart,
        Aimbot_TeamCheck1 = RequiredData.AimbotData.TeamCheck,
        Aimbot_Draw_FOV1 = RequiredData.AimbotData["Draw-FOV"],
        Aimbot_FOV_Radius1 = RequiredData.AimbotData.FOV,
        Aimbot_FOV_Color1 = Color3.fromRGB(27, 15, 25)
    }
    fovcircle.Visible = settings2.Aimbot_Draw_FOV1
    fovcircle.Radius = settings2.Aimbot_FOV_Radius1
    local dist = math.huge
    local closest_char = nil

    if settings2.Aiming1 then

        for i,v in next, dwEntities:GetChildren() do 

            if v ~= dwLocalPlayer and
            v.Character and
            v.Character:FindFirstChild("HumanoidRootPart") and
            v.Character:FindFirstChild("Humanoid") and
            v.Character:FindFirstChild("Humanoid").Health > 0 then

                if settings2.Aimbot_TeamCheck1 == true and
                v.Team ~= dwLocalPlayer.Team or
                settings2.Aimbot_TeamCheck1 == false then

                    local char = v.Character
                    local char_part_pos, is_onscreen = dwCamera:WorldToViewportPoint(char[settings.Aimbot_AimPart].Position)

                    if is_onscreen then

                        local mag = (Vector2.new(dwMouse.X, dwMouse.Y) - Vector2.new(char_part_pos.X, char_part_pos.Y)).Magnitude

                        if mag < dist and mag < settings2.Aimbot_FOV_Radius1 then

                            dist = mag
                            closest_char = char

                        end
                    end
                end
            end
        end

        if closest_char ~= nil and
        closest_char:FindFirstChild("HumanoidRootPart") and
        closest_char:FindFirstChild("Humanoid") and
        closest_char:FindFirstChild("Humanoid").Health > 0 then

            dwCamera.CFrame = CFrame.new(dwCamera.CFrame.Position, closest_char[settings.Aimbot_AimPart].Position)
        end
    end
end)
    end,
})
local ESPPart = VisualPage.Dropdown({
    Text = "Bone",
    Callback = function(value)
        RequiredData.VisualData["ESP-Part"] = value
    end,
    Options = {"Body","Head","Torso"}
})
local TransSlider = VisualPage.Slider({
    Text = "Transparency",
    Callback = function(value)
        RequiredData.VisualData.Transparency = value
    end,
    Min = 1,
    Max = 100,
    Def = 50
})
local Note = VisualPage.Label({
    Text = "Re-Toggle ESP To Update Transparency"
})
local ToggleESP = VisualPage.Toggle({
    Text = "Toggle ESP",
    Callback = function(value)
        RequiredData.VisualData["Enabled-ESP"] = value
    end,
})
local FOVSlider = MainPage.Slider({
    Text = "FieldOfView Slider",
    Callback = function(value)
        RequiredData.FieldOfView = value
    end,
    Min = 70,
    Max = 120,
    Def = 70
})
local JoinDiscord = MainPage.Button({
    Text = "Copy Discord Invite",
    Callback = function(value)
        if syn then
            setclipboard("https://discord.gg/ywzBphws4Y")
        end
    end,
})
game:GetService("RunService").RenderStepped:Connect(function()
    local lplr = game:GetService("Players").LocalPlayer
    local hum = lplr.Character.Humanoid
    if RequiredData.MiscData["Anti-FPS Drop"] then
        RequiredData.VisualData["Enabled-ESP"] = true
    end
    if RequiredData.FieldOfView ~= 70 then
        game:GetService("Workspace").CurrentCamera.FieldOfView = RequiredData.FieldOfView
    end
    if RequiredData.VisualData["Enabled-ESP"] then
        for _,x in pairs(game:GetService("Players"):GetPlayers()) do
			for _,q in pairs(x.Character:GetChildren()) do
				if x~=lplr then
                    -- note for coders: i know it looks bad pls nu judge :O
					if x.Character.Head:FindFirstChild("ESP") then
                    
					else
                        if RequiredData.VisualData["ESP-Part"] == "Body" and x.Character["Left Arm"] then
                            local esp = Instance.new("BoxHandleAdornment")
                            local Part = Instance.new("Part")
                            Part.BrickColor = x.TeamColor
                            esp.Name = "ESP"
                            esp.Parent = x.Character.Head
                            esp.Size = Vector3.new(1, 1, 1)
                            esp.Color3 = Part.Color
                            esp.AlwaysOnTop = true
                            esp.Transparency = RequiredData.VisualData.Transparency / 100
                            esp.Adornee = x.Character.Head
                            esp.Visible = true
                            esp.ZIndex = 2
                            local esp1 = Instance.new("BoxHandleAdornment")
                            esp1.Name = "ESP5"
                            esp1.Parent = x.Character.Head
                            esp1.Size = Vector3.new(1, 2, 1)
                            esp1.Color3 = Part.Color
                            esp1.AlwaysOnTop = true
                            esp1.Transparency = RequiredData.VisualData.Transparency / 100
                            esp1.Adornee = x.Character["Right Arm"]
                            esp1.Visible = true
                            esp1.ZIndex = 2
                            local esp2 = Instance.new("BoxHandleAdornment")
                            esp2.Name = "ESP4"
                            esp2.Parent = x.Character.Head
                            esp2.Size = Vector3.new(2, 2, 1)
                            esp2.Color3 = Part.Color
                            esp2.AlwaysOnTop = true
                            esp2.Transparency = RequiredData.VisualData.Transparency / 100
                            esp2.Adornee = x.Character.Torso
                            esp2.Visible = true
                            esp2.ZIndex = 2
                            local esp3 = Instance.new("BoxHandleAdornment")
                            esp3.Name = "ESP3"
                            esp3.Parent = x.Character.Head
                            esp3.Size = Vector3.new(1, 2, 1)
                            esp3.Color3 = Part.Color
                            esp3.AlwaysOnTop = true
                            esp3.Transparency = RequiredData.VisualData.Transparency / 100
                            esp3.Adornee = x.Character["Left Arm"]
                            esp3.Visible = true
                            esp3.ZIndex = 2
                            local esp4 = Instance.new("BoxHandleAdornment")
                            esp4.Name = "ESP2"
                            esp4.Parent = x.Character.Head
                            esp4.Size = Vector3.new(1, 2, 1)
                            esp4.Color3 = Part.Color
                            esp4.AlwaysOnTop = true
                            esp4.Transparency = RequiredData.VisualData.Transparency / 100
                            esp4.Adornee = x.Character["Left Leg"]
                            esp4.Visible = true
                            esp4.ZIndex = 2
                            local esp5 = Instance.new("BoxHandleAdornment")
                            esp5.Name = "ESP1"
                            esp5.Parent = x.Character.Head
                            esp5.Size = Vector3.new(1, 2, 1)
                            esp5.Color3 = Part.Color
                            esp5.AlwaysOnTop = true
                            esp5.Transparency = RequiredData.VisualData.Transparency / 100
                            esp5.Adornee = x.Character["Right Leg"]
                            esp5.Visible = true
                            esp5.ZIndex = 2
                    
                        end
					end
                    if x.Character.Head:FindFirstChild("ESPH") then
                       
					else
                        if RequiredData.VisualData["ESP-Part"] == "Torso" and x.Character["Torso"] then
                            local esp = Instance.new("BoxHandleAdornment")
                            local Part = Instance.new("Part")
                            Part.BrickColor = x.TeamColor
                            esp.Name = "ESPH"
                            esp.Parent = x.Character.Head
                            esp.Size = Vector3.new(1, 1, 1)
                            esp.Color3 = Part.Color
                            esp.AlwaysOnTop = true
                            esp.Transparency = RequiredData.VisualData.Transparency / 100
                            esp.Adornee = x.Character.Torso
                            esp.Visible = true
                            esp.ZIndex = 2
                        end
                    end
                    if x.Character.Head:FindFirstChild("ESPG") then
                       
					else
                        if RequiredData.VisualData["ESP-Part"] == "Head" and x.Character["Head"] then
                            local esp = Instance.new("BoxHandleAdornment")
                            local Part = Instance.new("Part")
                            Part.BrickColor = x.TeamColor
                            esp.Name = "ESPG"
                            esp.Parent = x.Character.Head
                            esp.Size = Vector3.new(1, 1, 1)
                            esp.Color3 = Part.Color
                            esp.AlwaysOnTop = true
                            esp.Transparency = RequiredData.VisualData.Transparency / 100
                            esp.Adornee = x.Character.Head
                            esp.Visible = true
                            esp.ZIndex = 2
                        end
                    end
				end
			end
		end
    else
        for _,q in pairs(game:GetService("Players"):GetPlayers()) do
            for _,x in pairs(q.Character:GetChildren()) do
                if x.Parent.Name ~= lplr.Name and x.Parent.Head then
                    local headdie = x.Parent.Head
                    for _,g in pairs(headdie:GetChildren()) do
                        if g and g.ClassName == "BoxHandleAdornment" then
                            g:Destroy()
                        end
                    end
                end
            end
        end
    end
    if RequiredData.WalkSpeed ~= 16 then
        if hum then
            hum.WalkSpeed = RequiredData.WalkSpeed
        end
    end
    if RequiredData.JumpPower ~= 50 then
        if hum then
            hum.JumpPower = RequiredData.JumpPower
        end
    end
end)
Visuals
