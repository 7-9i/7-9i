local Library = loadstring(game:HttpGet("https://pastebin.com/raw/hYt8sNUU"))() --you can go into the github link and copy all of it and modify it for yourself.
local Window = Library:CreateWindow("79iware.cc", Vector2.new(492, 598), Enum.KeyCode.RightShift) --you can change your UI keybind
local AimingTab = Window:CreateTab("Blatant")
local Legit = Window:CreateTab("Legit")
local Misc = Window:CreateTab("Misc")
local Visuals = Window:CreateTab("Visuals")
local Universal = Window:CreateTab("Universal")
local Camlockv = Window:CreateTab("Camlock")
local Config = Window:CreateTab("Settings")

      
local sound = Instance.new("Sound", game.Workspace)
sound.SoundId = "rbxassetid://5226834046"

if not sound.IsLoaded then
	sound.Loaded:wait()
end

local sound1 = Instance.new("Sound", game.Workspace)
sound1.SoundId = "rbxassetid://1905367471"

if not sound1.IsLoaded then
	sound1.Loaded:wait()
end



getgenv().ZyZKey = Enum.KeyCode.Q
getgenv().Prediction = 0.19
getgenv().Tracer = false
getgenv().TracerBugged = false
getgenv().LookAt = false
getgenv().ZyZPart = "LowerTorso"
getgenv().NotifyZyZ = false
getgenv().BlindStrafe = false
getgenv().ViewPlr = false
getgenv().NotificationsSound = false
getgenv().Enabled = false
_G.AirshotFunction = false
_G.AirshotPart = "LowerTorso"
_G.FRAME = Vector3.new(0,20,0)
_G.FRAME2 = Vector3.new(0,20,0)


local guimain = Instance.new("Folder", game.CoreGui)
local CC = game:GetService "Workspace".CurrentCamera
local LocalMouse = game.Players.LocalPlayer:GetMouse()
local Locking = false
local cc = game:GetService("Workspace").CurrentCamera
local gs = game:GetService("GuiService")
local ggi = gs.GetGuiInset
local lp = game:GetService("Players").LocalPlayer
local mouse = lp:GetMouse()

local Tracer = Drawing.new("Line")
Tracer.Visible = false
Tracer.Color = Color3.fromRGB(123, 56, 182)
Tracer.Thickness = 1
Tracer.Transparency = 1

local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(
    function(keygo, ok)
        if (not ok) then
            if (keygo.KeyCode == getgenv().ZyZKey) then
                if getgenv().Enabled then
                    Locking = not Locking
                    if Locking then
                        Plr = getClosestPlayerToCursor()
                        if getgenv().ViewPlr then
                        	game.Workspace.CurrentCamera.CameraSubject = Plr.Character
                            
                        end
                        if getgenv().NotificationsSound then
                            sound:Play()
                        end
                        if getgenv().NotifyZyZ then
               local Notify = NotifyLibrary.Notify
                        end
                    elseif not Locking then
                        
if getgenv().ViewPlr then
	game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character
end

if getgenv().NotificationsSound then
    sound1:Play()
end

                        if getgenv().NotifyZyZ then
                            Library:Notify('unlock')
                        end
                    end
                end
            end
        end
    end
)
function getClosestPlayerToCursor()
    local closestPlayer
    local shortestDistance = 137

    for i, v in pairs(game.Players:GetPlayers()) do
        if
            v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") and
                v.Character.Humanoid.Health ~= 0 and
                v.Character:FindFirstChild("LowerTorso")
         then
            local pos = CC:WorldToViewportPoint(v.Character.PrimaryPart.Position)
            local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(LocalMouse.X, LocalMouse.Y)).magnitude
            if magnitude < shortestDistance then
                closestPlayer = v
                shortestDistance = magnitude
            end
        end
    end
    return closestPlayer
end

local rawmetatable = getrawmetatable(game)
local old = rawmetatable.__namecall
setreadonly(rawmetatable, false)
rawmetatable.__namecall =
    newcclosure(
    function(...)
        local args = {...}
        if Locking and getnamecallmethod() == "FireServer" and args[2] == "UpdateMousePos" then
            args[3] =
                Plr.Character[getgenv().ZyZPart].Position + (Plr.Character[getgenv().ZyZPart].Velocity * Prediction)
            return old(unpack(args))
        end
        return old(...)
    end
)

game:GetService("RunService").RenderStepped:Connect(
    function()
        if getgenv().autosetup == true then
            local pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
            local split = string.split(pingvalue, " ")
            local ping = split[1]
            if tonumber(ping) < 30 then
                getgenv().Prediction = 1.14
            elseif tonumber(ping) <= 30 then
                if tonumber(ping) < 40 then
                    getgenv().Prediction = 1.16
                elseif tonumber(ping) <= 40 then
                    if tonumber(ping) < 50 then
                        getgenv().Prediction = 1.19
                    elseif tonumber(ping) <= 50 then
                        if tonumber(ping) < 70 then
                            getgenv().Prediction = 1.22
                        elseif tonumber(ping) <= 80 then
                            getgenv().Prediction = 1.38
                        elseif tonumber(ping) <= 80 then
                            getgenv().Prediction = 1.39
                        elseif tonumber(ping) <= 90 then
                            getgenv().Prediction = 1.42
                        elseif tonumber(ping) <= 150 then
                            getgenv().Prediction = 1.51
                        elseif tonumber(ping) >= 200 then
                            getgenv().Prediction = 1.69
                        end
                    end
                end
            end
        end
        
        if _G.AirshotFunction == true then
            if Plr.Character.Humanoid.Jump == true and Plr.Character.Humanoid.FloorMaterial == Enum.Material.Air then
                getgenv().ZyZPart = _G.AirshotPart
            else
                Plr.Character:WaitForChild("Humanoid").StateChanged:Connect(
                    function(old, new)
                        if new == Enum.HumanoidStateType.Freefall then
                            getgenv().Partz = _G.AirshotPart
                        else
                            getgenv().ZyZPart = "LowerTorso"
                        end
                    end
                )
            end
        end
if getgenv().BlindStrafe and Locking and getgenv().Enabled then
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Plr.Character.HumanoidRootPart.Position + _G.FRAME)
    

end

if getgenv().BlindStrafe and Locking and getgenv().Enabled then
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Plr.Character.HumanoidRootPart.Position + _G.FRAME)
    

end

if getgenv().LookAt and Locking and getgenv().Enabled then
                    local Char = game.Players.LocalPlayer.Character
                local PrimaryPartOfChar = game.Players.LocalPlayer.Character.PrimaryPart
                local NearestChar = Plr.Character
                local NearestRoot = Plr.Character.HumanoidRootPart
                local NearestPos = CFrame.new(PrimaryPartOfChar.Position, Vector3.new(NearestRoot.Position.X, NearestRoot.Position.Y, NearestRoot.Position.Z))
                Char:SetPrimaryPartCFrame(NearestPos)
                
    end
        if getgenv().Tracer == true and Locking then
            local Vector, OnScreen =
                cc:worldToViewportPoint(
                Plr.Character[getgenv().ZyZPart].Position + (Plr.Character[getgenv().ZyZPart].Velocity * Prediction / 10)
            )
            Tracer.Visible = true
            Tracer.From = Vector2.new(mouse.X, mouse.Y + ggi(gs).Y)
            Tracer.To = Vector2.new(Vector.X, Vector.Y)
        elseif getgenv().Tracer == false then
            Tracer.Visible = false
        end
if Tracer.Visible == true and not Locking and getgenv().Enabled then
    getgenv().TracerBugged = true
    Tracer.Visible = false
end

if getgenv().Tracer == true and getgenv().TracerBugged and Locking then
    Tracer.Visible = true
end
    end)
  

local NotifyLibrary = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/Dynissimo/main/Scripts/AkaliNotif.lua"))()
local Notify = NotifyLibrary.Notify

local testSection = AimingTab:CreateSector("Main Aimlock", "left")  --you can  change the section code, for example "testsection" can be changed to "FunnyCoolSection" etc.

testSection:AddToggle("Enable Aimlock", false, function(f)
   getgenv().Enabled = f
end)

testSection:AddTextbox("Prediction", nil, function(State)
getgenv().Prediction = State
end)

testSection:AddToggle("Tracer", false, function(f)
 getgenv().Tracer = f  
end)



testSection:AddToggle("Look At", false, function(f)
getgenv().LookAt = f   
end)

testSection:AddDropdown("AimPart", {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso"}, "LowerTorso", false, function(f)
getgenv().ZyZPart = f
end)
testSection:AddToggle("Notfications", false, function(f)
NotifyLibrary.Notify = f  
end)

testSection:AddToggle("Notfication Sound ", false, function(f)
   getgenv().NotificationsSound = f
end)

testSection:AddToggle("Air Strafe", false, function(f)
getgenv().BlindStrafe = f   
end)

testSection:AddToggle("View Player", false, function(f)
getgenv().ViewPlr = f   
end)


testSection:AddSlider("Air Strafe Height", 0, 30, 50, 2, function(State)
_G.FRAME = Vector3.new(0,State,0)    
end)



testSection:AddSlider("Tracer Thickness", 0, 1, 10, 2, function(State)
    Tracer.Thickness = State
end)

testSection:AddSlider("Tracer Transparency", 0, 1, 3, 2, function(State)
  Tracer.Transparency = State  
end)

local ColorToggle = testSection:AddToggle("Tracer Color", false, function(e)

end)

ColorToggle:AddColorpicker(Color3.fromRGB(75, 0,130), function(e)
Tracer.Color = e   
end)

local Misc = Misc:CreateSector("Misc", "left") 

Misc:AddButton("Cframe (Z)", function()
      	repeat
		wait()
	until game:IsLoaded()
	local L_134_ = game:service('Players')
	local L_135_ = L_134_.LocalPlayer
	repeat
		wait()
	until L_135_.Character
	local L_136_ = game:service('UserInputService')
	local L_137_ = game:service('RunService')
	getgenv().Multiplier = 0.5
	local L_138_ = true
	local L_139_
	L_136_.InputBegan:connect(function(L_140_arg0)
		if L_140_arg0.KeyCode == Enum.KeyCode.LeftBracket then
			Multiplier = Multiplier + 0.01
			print(Multiplier)
			wait(0.2)
			while L_136_:IsKeyDown(Enum.KeyCode.LeftBracket) do
				wait()
				Multiplier = Multiplier + 0.01
				print(Multiplier)
			end
		end
		if L_140_arg0.KeyCode == Enum.KeyCode.RightBracket then
			Multiplier = Multiplier - 0.01
			print(Multiplier)
			wait(0.2)
			while L_136_:IsKeyDown(Enum.KeyCode.RightBracket) do
				wait()
				Multiplier = Multiplier - 0.01
				print(Multiplier)
			end
		end
		if L_140_arg0.KeyCode == Enum.KeyCode.Z then
			L_138_ = not L_138_
			if L_138_ == true then
				repeat
					game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame + game.Players.LocalPlayer.Character.Humanoid.MoveDirection * Multiplier
					game:GetService("RunService").Stepped:wait()
				until L_138_ == false
			end
		end
	end) 
end)


Misc:AddSlider("Cframe Speed", 0, 1, 8, 1, function(s)
getgenv().Multiplier = s
end)


Misc:AddButton("Cframe Fixer", function()
      for _, v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
        if v:IsA("Script") and v.Name ~= "Health" and v.Name ~= "Sound" and v:FindFirstChild("LocalScript") then
            v:Destroy()
        end
    end
    game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
        repeat
            wait()
        until game.Players.LocalPlayer.Character
        char.ChildAdded:Connect(function(child)
            if child:IsA("Script") then 
                wait(0.1)
                if child:FindFirstChild("LocalScript") then
                    child.LocalScript:FireServer()
                end
            end
        end)
    end)
end)

Misc:AddButton("AntiStomp (M)", function(IhateGayPeople)
         game.Players.LocalPlayer:GetMouse().KeyDown:Connect(function(KeyPressed)
 if KeyPressed == "m" then
    for L_170_forvar0, L_171_forvar1 in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
        if L_171_forvar1:IsA("BasePart") then
            L_171_forvar1:Destroy()
        end
    end
    end
end)
end)

Misc:AddButton("AutoClicker (L)", function(IhateGayPeople)
    local time = 0.01 --decrease if too slow increase if too fast

click = false
m = game.Players.LocalPlayer:GetMouse()
m.KeyDown:connect(function(key)
if key == "l" then
if click == true then click = false
elseif
click == false then click = true

while click == true do 
wait(time)
mouse1click()
end
end
end
end)
end)


Misc:AddButton("No Jump Cooldown", function(IhateGayPeople)
   if not game.IsLoaded(game) then 
        game.Loaded.Wait(game.Loaded);
    end

    local IsA = game.IsA;
    local newindex = nil 

    newindex = hookmetamethod(game, "__newindex", function(self, Index, Value)
        if not checkcaller() and IsA(self, "Humanoid") and Index == "JumpPower" then 
            return
        end

        return newindex(self, Index, Value);
    end)
end)

Misc:AddButton("AntiSlow (H)", function(IhateGayPeople)
    repeat
		wait()
	until game:IsLoaded()
	getgenv().Fix = true
	getgenv().TeclasWS = {
		["tecla1"] = "nil",
		["tecla2"] = "nil",
		["tecla3"] = "H"
	}
	local L_121_ = game:GetService("Players")
	local L_122_ = game:GetService("StarterGui") or "son una mierda"
	local L_123_ = L_121_.LocalPlayer
	local L_124_ = L_123_:GetMouse()
	local L_125_ = getrenv()._G
	local L_126_ = getrawmetatable(game)
	local L_127_ = L_126_.__newindex
	local L_128_ = L_126_.__index
	local L_129_ = 22
	local L_130_ = true
	function anunciar_atentado_terrorista(L_138_arg0)
		L_122_:SetCore("SendNotification", {
			Title = "antispeed",
			Text = L_138_arg0
		})
	end
	getgenv().TECHWAREWALKSPEED_LOADED = true
	wait(1.5)
	anunciar_atentado_terrorista("Press  " .. TeclasWS.tecla3 .. " to turn on/off antislow")
	L_124_.KeyDown:Connect(
            function(L_139_arg0)
		if L_139_arg0:lower() == TeclasWS.tecla1:lower() then
			L_129_ = L_129_ + 1
			anunciar_atentado_terrorista("播放器速度已提高 (speed = " .. tostring(L_129_) .. ")")
		elseif L_139_arg0:lower() == TeclasWS.tecla2:lower() then
			L_129_ = L_129_ - 1
			anunciar_atentado_terrorista("玩家的速度已降低 (speed = " .. tostring(L_129_) .. ")")
		elseif L_139_arg0:lower() == TeclasWS.tecla3:lower() then
			if L_130_ then
				L_130_ = false
				anunciar_atentado_terrorista("antislow off")
			else
				L_130_ = true
				anunciar_atentado_terrorista("antislow on")
			end
		end
	end
        )
	setreadonly(L_126_, false)
	L_126_.__index =
            newcclosure(
            function(L_140_arg0, L_141_arg1)
		local L_142_ = checkcaller()
		if L_141_arg1 == "WalkSpeed" and not L_142_ then
			return L_125_.CurrentWS
		end
		return L_128_(L_140_arg0, L_141_arg1)
	end
        )
	L_126_.__newindex =
            newcclosure(
            function(L_143_arg0, L_144_arg1, L_145_arg2)
		local L_146_ = checkcaller()
		if L_130_ then
			if L_144_arg1 == "WalkSpeed" and L_145_arg2 ~= 0 and not L_146_ then
				return L_127_(L_143_arg0, L_144_arg1, L_129_)
			end
		end
		return L_127_(L_143_arg0, L_144_arg1, L_145_arg2)
	end
        )
	setreadonly(L_126_, true)
	repeat
		wait()
	until game:IsLoaded()
	local L_131_ = game:service("Players")
	local L_132_ = L_131_.LocalPlayer
	repeat
		wait()
	until L_132_.Character
	local L_133_ = game:service("UserInputService")
	local L_134_ = game:service("RunService")
	local L_135_ = -0.27
	local L_136_ = false
	local L_137_
	L_133_.InputBegan:connect(
            function(L_147_arg0)
		if L_147_arg0.KeyCode == Enum.KeyCode.LeftBracket then
			L_135_ = L_135_ + 0.01
			print(L_135_)
			wait(0.2)
			while L_133_:IsKeyDown(Enum.KeyCode.LeftBracket) do
				wait()
				L_135_ = L_135_ + 0.01
				print(L_135_)
			end
		end
		if L_147_arg0.KeyCode == Enum.KeyCode.RightBracket then
			L_135_ = L_135_ - 0.01
			print(L_135_)
			wait(0.2)
			while L_133_:IsKeyDown(Enum.KeyCode.RightBracket) do
				wait()
				L_135_ = L_135_ - 0.01
				print(L_135_)
			end
		end
		if L_147_arg0.KeyCode == Enum.KeyCode.Z then
			L_136_ = not L_136_
			if L_136_ == true then
				repeat
					game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame +
                                game.Players.LocalPlayer.Character.Humanoid.MoveDirection * L_135_
					game:GetService("RunService").Stepped:wait()
				until L_136_ == false
			end
		end
	end
        )
end)

Misc:AddButton("No Recoil (Breaks Aimlock)", function(IhateGayPeople)
    local CurrentFocus = game:GetService("Workspace").CurrentCamera.CFrame
    game:GetService("Workspace").CurrentCamera:Destroy()
    local Instance = Instance.new("Camera", game:GetService("Workspace"))
    Instance.CameraSubject = game:GetService("Players").LocalPlayer.Character.Humanoid
    Instance.CameraType = Enum.CameraType.Custom
    Instance.CFrame = CurrentFocus
end)

Misc:AddTextbox("FOV", nil, function(v)
  workspace.CurrentCamera.FieldOfView = v
end)

Misc:AddTextbox("Fog", nil, function(b)
game.Lighting.FogEnd = b
end)

Misc:AddButton("Rejoin", function(IhateGayPeople)
               local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local Rejoin = coroutine.create(function()
    local Success, ErrorMessage = pcall(function()
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    end)

    if ErrorMessage and not Success then
        warn(ErrorMessage)
    end
end)

coroutine.resume(Rejoin)    
end)

Misc:AddButton("UnderGround AA (N)", function(IhateGayPeople)
        local Toggled = false
local KeyCode = 'n'
local hip = 2.80
local val = -35





function AA()
    local oldVelocity = game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity
    game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(oldVelocity.X, val, oldVelocity.Z)
    game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(oldVelocity.X, oldVelocity.Y, oldVelocity.Z)
    game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(oldVelocity.X, val, oldVelocity.Z)
    game.Players.LocalPlayer.Character.Humanoid.HipHeight = hip
end

game:GetService('UserInputService').InputBegan:Connect(function(Key)
    if Key.KeyCode == Enum.KeyCode[KeyCode:upper()] and not game:GetService('UserInputService'):GetFocusedTextBox() then
        if Toggled then
            Toggled = false
            game.Players.LocalPlayer.Character.Humanoid.HipHeight = hip

        elseif not Toggled then
            Toggled = true

            while Toggled do
                AA()
                task.wait()
            end
        end
    end
end)    
end)

Misc:AddButton("DeSync (V)", function(IhateGayPeople)
     local P1000ToggleKey = "v" -- Change that to whatever keybind: "t"


--// Services
checkcaller = checkcaller
newcclosure = newcclosure
hookmetamethod = hookmetamethod

local PastedSources = false
local BruhXD = game:GetService("RunService")
local LocalPlayer = game:GetService("Players").LocalPlayer
local Bullshit = LocalPlayer:GetMouse()


--// Toggles
Bullshit.KeyDown:Connect(function(SayNoToOblivity)
	if SayNoToOblivity == string.lower(P1000ToggleKey) then
		pcall(function()
			if PastedSources == false then
				PastedSources = true
				print("Enabled P1000")
			elseif PastedSources == true then
				PastedSources = false
				print("Disabled P1000")
			end
		end)
	end
end)

Bullshit.KeyDown:Connect(function(SayNoToOblivity)
	if SayNoToOblivity == ("=") then
		game:GetService("TeleportService"):Teleport(game.PlaceId, LocalPlayer) 
	end
end)


--// Desync_Source
function RandomNumberRange(a)
	return math.random(-a * 100, a * 100) / 100
end

function RandomVectorRange(a, b, c)
	return Vector3.new(RandomNumberRange(a), RandomNumberRange(b), RandomNumberRange(c))
end


local DesyncTypes = {}
BruhXD.Heartbeat:Connect(function()
	if PastedSources == true then
		DesyncTypes[1] = LocalPlayer.Character.HumanoidRootPart.CFrame
		DesyncTypes[2] = LocalPlayer.Character.HumanoidRootPart.AssemblyLinearVelocity

		local SpoofThis = LocalPlayer.Character.HumanoidRootPart.CFrame

		SpoofThis = SpoofThis * CFrame.new(Vector3.new(0, 0, 0))
		SpoofThis = SpoofThis * CFrame.Angles(math.rad(RandomNumberRange(180)), math.rad(RandomNumberRange(180)), math.rad(RandomNumberRange(180)))

		LocalPlayer.Character.HumanoidRootPart.CFrame = SpoofThis

		LocalPlayer.Character.HumanoidRootPart.AssemblyLinearVelocity = Vector3.new(1, 1, 1) * 16384

		BruhXD.RenderStepped:Wait()

		LocalPlayer.Character.HumanoidRootPart.CFrame = DesyncTypes[1]
		LocalPlayer.Character.HumanoidRootPart.AssemblyLinearVelocity = DesyncTypes[2]
	end
end)


--// Hook_CFrame
local XDDDDDD = nil
XDDDDDD = hookmetamethod(game, "__index", newcclosure(function(self, key)
	if PastedSources == true then
		if not checkcaller() then
			if key == "CFrame" and PastedSources == true and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character:FindFirstChild("Humanoid") and LocalPlayer.Character:FindFirstChild("Humanoid").Health > 0 then
				if self == LocalPlayer.Character.HumanoidRootPart then
					return DesyncTypes[1] or CFrame.new()
				elseif self == LocalPlayer.Character.Head then
					return DesyncTypes[1] and DesyncTypes[1] + Vector3.new(0, LocalPlayer.Character.HumanoidRootPart.Size / 2 + 0.5, 0) or CFrame.new()
				end
			end
		end
	end
	return XDDDDDD(self, key)
end))    
end)

Misc:AddButton("Anti Bag", function(IhateGayPeople)
      game.Players.LocalPlayer.Character["Christmas_Sock"]:Destroy()    
end)

Misc:AddButton("Spin Bot (B)", function(IhateGayPeople)
            local L_165_ = false
    local L_166_ = game:GetService("UserInputService");
    L_166_.InputBegan:Connect(function(L_167_arg0, L_168_arg1)
        if (L_167_arg0.KeyCode == Enum.KeyCode.B and L_168_arg1 == false) then
            if L_165_ == false then
                L_165_ = true
                wait()
                getgenv().urspeed = 500
                local L_169_ = game.Players.LocalPlayer.Character
                while wait() do
                    L_169_.HumanoidRootPart.CFrame = L_169_.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(urspeed), 0)
                end
            else
                L_165_ = false
                getgenv().urspeed = 0
            end
        end
    end);
    game:GetService('RunService').Stepped:connect(function()
        if L_165_ == true then
        end
    end)
    game:GetService('RunService').Stepped:connect(function()
        if L_165_ == false then
            stopTracks();
        end
    end)   
end)

Misc:AddButton("AntiBan", function(IhateGayPeople)
        assert(getrawmetatable)
gmt = getrawmetatable(game)
setreadonly(gmt, false)
old = gmt.__namecall
gmt.__namecall =
    newcclosure(
        function(self, ...)
        local args = {...}
        if tostring(args[1]) == "BreathingHAMON" then
            return
        elseif tostring(args[1]) == "TeleportDetect" then
            return
        elseif tostring(args[1]) == "JJARC" then
            return
        elseif tostring(args[1]) == "TakePoisonDamage" then
            return
        elseif tostring(args[1]) == "CHECKER_1" then
            return
        elseif tostring(args[1]) == "CHECKER" then
            return
        elseif tostring(args[1]) == "GUI_CHECK" then
            return
        elseif tostring(args[1]) == "OneMoreTime" then
            return
        elseif tostring(args[1]) == "checkingSPEED" then
            return
        elseif tostring(args[1]) == "BANREMOTE" then
            return
        elseif tostring(args[1]) == "PERMAIDBAN" then
            return
        elseif tostring(args[1]) == "KICKREMOTE" then
            return
        elseif tostring(args[1]) == "BR_KICKPC" then
            return
        elseif tostring(args[1]) == "FORCEFIELD" then
            return
        elseif tostring(args[1]) == "Christmas_Sock" then
            return
        elseif tostring(args[1]) == "VirusCough" then
            return
        elseif tostring(args[1]) == "Symbiote" then
            return
        elseif tostring(args[1]) == "Symbioted" then
            return 
         
        end
        return old(self, ...)
    end)   
end)

Misc:AddButton("GodMode", function(IhateGayPeople)
   local localPlayer = game:GetService('Players').LocalPlayer;
                local localCharacter = localPlayer.Character;
                localCharacter:FindFirstChildOfClass('Humanoid').Health = 0;
                local newCharacter = localPlayer.CharacterAdded:Wait();
                local spoofFolder = Instance.new('Folder');
                spoofFolder.Name = 'FULLY_LOADED_CHAR';
                spoofFolder.Parent = newCharacter;
                newCharacter:WaitForChild('RagdollConstraints'):Destroy();
                local spoofValue = Instance.new('BoolValue', newCharacter);
                spoofValue.Name = 'RagdollConstraints';
                local name = game.Players.LocalPlayer.Name
                local lol =    game.Workspace:WaitForChild(name)
                local money = Instance.new("Folder",game.Players.LocalPlayer.Character);money.Name = "FULLY_LOADED_CHAR"
                lol.Parent = game.Workspace.Players
                game.Players.LocalPlayer.Character:WaitForChild("BodyEffects")
                game.Players.LocalPlayer.Character.BodyEffects.BreakingParts:Destroy()
end)

local Camlock = Legit:CreateSector("Camlock", "left") 
local Silent = Legit:CreateSector("Silent Aim", "right") 
getgenv().Aimbot = { 
    Enabled = true,
    Smoothness = 0.005,
    Smoothing = false,
    AirshotFunc = false,
    AirshotPart = "RightFoot",
    AimPart = "HumanoidRootPart",
    Predicting = 1.2,
    Key = Enum.KeyCode.Q,
    Toggled
}


function x(tt,tx,cc)
    game.StarterGui:SetCore("SendNotification", {
        Title = tt;
        Text = tx;
        Duration = cc;
        Icon = "rbxthumb://type=Asset&id=7262533709&w=150&h=150";
    })
end

local CurrentCamera = game:GetService("Workspace").CurrentCamera
local RunService = game:GetService("RunService")
local Mouse = game.Players.LocalPlayer:GetMouse()
local LocalPlayer = game.Players.LocalPlayer
local Plr

function FindClosestPlayer()
    local ClosestDistance, ClosestPlayer = math.huge, nil;
    for _, Player in next, game:GetService("Players"):GetPlayers() do
        local ISNTKNOCKED = Player.Character:WaitForChild("BodyEffects")["K.O"].Value ~= true
        local ISNTGRABBED = Player.Character:FindFirstChild("GRABBING_COINSTRAINT") == nil

        if Player ~= LocalPlayer then
            local Character = Player.Character
            if Character and Character.Humanoid.Health > 1 and ISNTKNOCKED and ISNTGRABBED then
                local Position, IsVisibleOnViewPort = CurrentCamera:WorldToViewportPoint(Character.HumanoidRootPart
                                                                                             .Position)
                if IsVisibleOnViewPort then
                    local Distance = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(Position.X, Position.Y)).Magnitude
                    if Distance < ClosestDistance then
                        ClosestPlayer = Player
                        ClosestDistance = Distance
                    end
                end
            end
        end
    end
    return ClosestPlayer, ClosestDistance
end



    game:GetService("UserInputService").InputBegan:Connect(function(keygo)
           if (keygo.KeyCode == getgenv().Aimbot.Key) then
               Toggled = not Toggled
               if Toggled then
               Plr =  FindClosestPlayer()
end
         end
           
end)
game:GetService("RunService").RenderStepped:Connect(function()
if getgenv().Aimbot.Smoothing and getgenv().Aimbot.Enabled and Toggled == true then
    local Main = CFrame.new(workspace.CurrentCamera.CFrame.p, Plr.Character[getgenv().Aimbot.AimPart].Position + Plr.Character[getgenv().Aimbot.AimPart].Velocity*getgenv().Aimbot.Predicting)
                                 workspace.CurrentCamera.CFrame = workspace.CurrentCamera.CFrame:Lerp(Main, getgenv().Aimbot.Smoothness, Enum.EasingStyle.Elastic, Enum.EasingDirection.InOut)
                            elseif getgenv().Aimbot.Smoothing == false and getgenv().Aimbot.Enabled and Toggled == true then
    workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, Plr.Character[getgenv().Aimbot.AimPart].Position + Plr.Character[getgenv().Aimbot.AimPart].Velocity*getgenv().Aimbot.Predicting)
                            end

end)

if getgenv().Aimbot.AirshotFunc == true then
    
                if Plr.Character.Humanoid.Jump == true and Plr.Character.Humanoid.FloorMaterial == Enum.Material.Air then
                    getgenv().Aimbot.AimPart = getgenv().Aimbot.AirshotPart
                else
                    Plr.Character:WaitForChild("Humanoid").StateChanged:Connect(function(old,new)
                        if new == Enum.HumanoidStateType.Freefall then
                        getgenv().Aimbot.AimPart = getgenv().Aimbot.AirshotPart
                        else
                            getgenv().Aimbot.AimPart = getgenv().Aimbot.AimPart
                        end
                    end)
                end
            end

Camlock:AddToggle("Enable", false, function(t)
  getgenv().Aimbot.Enabled = t
end)

Camlock:AddToggle("Airshot Function", false, function(f)
   getgenv().TeamCheck = f
end)

Camlock:AddToggle("Airshot Function", false, function(f)
   getgenv().Aimbot.AirshotFunc = f
end)



Camlock:AddToggle("Smoothness", false, function(f)
   getgenv().Aimbot.Smoothness = f
end)

Camlock:AddTextbox("Prediction", nil, function(f)
getgenv().Aimbot.Predicting = f
end)

Camlock:AddTextbox("Smoothness", nil, function(f)
getgenv().Aimbot.Smoothing = f
end)

Silent:AddButton("Silent Aim Enable", function(IhateGayPeople)
loadstring(game:HttpGet("https://pastebin.com/raw/KRBZu04H", true))()
end)

Silent:AddTextbox("Prediction", nil, function(State)
DaHoodSettings.Prediction = State
end)

Silent:AddToggle("Silent Aim FOV", false, function(f)
   Aiming.ShowFOV = f
end)

Silent:AddTextbox("Silent Aim FOV Size", nil, function(State)
Aiming.FOV = State
end)

Silent:AddDropdown("Dropdown", {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso"},  false, function(f)
Aiming.TargetPart = f
end)

local ESP = Visuals:CreateSector("ESP", "right") 
local Me = Visuals:CreateSector("Characteristics", "left") 

   local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/trendyylol/roblox/main/Libraries/ESP/Pikahub"))()
local library = loadstring(game:HttpGet("https://pastebin.com/raw/U2HwmEyF"))()    

local dwEntities = game:GetService("Players")
local dwLocalPlayer = dwEntities.LocalPlayer 
local dwRunService = game:GetService("RunService")

local settings_tbl = {
    ESP_Enabled = false,
    ESP_TeamCheck = false,
    Chams = true,
    Chams_Color = Color3.fromRGB(255,255,255),
    Chams_Transparency = 0.1,
    Chams_Glow_Color = Color3.fromRGB(0,0,0)
}

function destroy_chams(char)

    for k,v in next, char:GetChildren() do 

        if v:IsA("BasePart") and v.Transparency ~= 1 then

            if v:FindFirstChild("Glow") and 
            v:FindFirstChild("Chams") then

                v.Glow:Destroy()
                v.Chams:Destroy() 

            end 

        end 

    end 

end

dwRunService.Heartbeat:Connect(function()

    if settings_tbl.ESP_Enabled then

        for k,v in next, dwEntities:GetPlayers() do 

            if v ~= dwLocalPlayer then

                if v.Character and
                v.Character:FindFirstChild("HumanoidRootPart") and 
                v.Character:FindFirstChild("Humanoid") and 
                v.Character:FindFirstChild("Humanoid").Health ~= 0 then

                    if settings_tbl.ESP_TeamCheck == false then

                        local char = v.Character 

                        for k,b in next, char:GetChildren() do 

                            if b:IsA("BasePart") and 
                            b.Transparency ~= 1 then
                                
                                if settings_tbl.Chams then

                                    if not b:FindFirstChild("Glow") and
                                    not b:FindFirstChild("Chams") then

                                        local chams_box = Instance.new("BoxHandleAdornment", b)
                                        chams_box.Name = "Chams"
                                        chams_box.AlwaysOnTop = true 
                                        chams_box.ZIndex = 4 
                                        chams_box.Adornee = b 
                                        chams_box.Color3 = settings_tbl.Chams_Color
                                        chams_box.Transparency = settings_tbl.Chams_Transparency
                                        chams_box.Size = b.Size + Vector3.new(0.02, 0.02, 0.02)

                                        local glow_box = Instance.new("BoxHandleAdornment", b)
                                        glow_box.Name = "Glow"
                                        glow_box.AlwaysOnTop = false 
                                        glow_box.ZIndex = 3 
                                        glow_box.Adornee = b 
                                        glow_box.Color3 = settings_tbl.Chams_Glow_Color
                                        glow_box.Size = chams_box.Size + Vector3.new(0.13, 0.13, 0.13)

                                    end

                                else

                                    destroy_chams(char)

                                end
                            
                            end

                        end

                    else

                        if v.Team == dwLocalPlayer.Team then
                            destroy_chams(v.Character)
                        end

                    end

                else

                    destroy_chams(v.Character)

                end

            end

        end

    else 

        for k,v in next, dwEntities:GetPlayers() do 

            if v ~= dwLocalPlayer and 
            v.Character and 
            v.Character:FindFirstChild("HumanoidRootPart") and 
            v.Character:FindFirstChild("Humanoid") and 
            v.Character:FindFirstChild("Humanoid").Health ~= 0 then
                
                destroy_chams(v.Character)

            end

        end

    end

end)

ESP:AddToggle("Boxes", false, function(f)
  getgenv().PikaESPSettings.Box = f  
end)

ESP:AddToggle("Name", false, function(f)
   getgenv().PikaESPSettings.Name = f
end)

ESP:AddToggle("Tracers", false, function(f)
    getgenv().PikaESPSettings.Tracers = f   
end)

ESP:AddToggle("Unlock Tracer", false, function(f)
   getgenv().PikaESPSettings.UnlockTracers = f   
end)

ESP:AddToggle("Chams", false, function(f)
  settings_tbl.ESP_Enabled = f
end)





ESP:AddToggle("Team Check", false, function(f)
   getgenv().PikaESPSettings.Teammates = f
end)


Me:AddButton("Headless", function(IhateGayPeople)
                 local L_393_ = game.Players.LocalPlayer.Character
    local L_394_ = L_393_:WaitForChild("Head")
    local L_395_ = L_394_:WaitForChild("face")
    L_395_.Transparency = 1 
    L_394_.Transparency = 1   
end)

Me:AddButton("Korblox", function(IhateGayPeople)
              local L_396_ = game.Players.LocalPlayer.Character
    local L_397_ = game.Players.LocalPlayer.Character
    local L_398_ = L_396_.Head
    local L_399_ = L_398_.face
    local L_400_ = L_397_.RightFoot
    local L_401_ = L_397_.RightLowerLeg
    local L_402_ = L_397_.RightUpperLeg
    local L_403_ = L_397_.LeftFoot
    local L_404_ = L_397_.LeftLowerLeg
    local L_405_ = L_397_.LeftUpperLeg
    
    -- Right
    L_400_.MeshId = "http://www.roblox.com/asset/?id=902942093"
    L_401_.MeshId = "http://www.roblox.com/asset/?id=902942093"
    L_402_.MeshId = "http://www.roblox.com/asset/?id=902942096"
    L_402_.TextureID = "http://roblox.com/asset/?id=902843398"
    L_400_.Transparency = 1
    L_401_.Transparency = 1    
end)

Me:AddButton("Fat", function(IhateGayPeople)
   loadstring(game:HttpGet("https://raw.githubusercontent.com/slammy1/fat/main/3"))() 
end)


Me:AddButton("FE Korblox", function(IhateGayPeople)
    game.Players.LocalPlayer.Character.RightUpperLeg:Destroy()
end)

Me:AddButton("Playful Vampire", function(IhateGayPeople)
                local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://2409281591"    
end)

Me:AddButton("Red Beastmode", function(IhateGayPeople)
           local L_412_ = game.Players.LocalPlayer.Character
	local L_413_ = L_412_:WaitForChild("Head")
	local L_414_ = L_413_:WaitForChild("face")
	L_414_.Texture = "rbxassetid://127959433"    
end)

Me:AddButton("Green Beastmode", function(IhateGayPeople)
           local L_412_ = game.Players.LocalPlayer.Character
	local L_413_ = L_412_:WaitForChild("Head")
	local L_414_ = L_413_:WaitForChild("face")
	L_414_.Texture = "rbxassetid://2225757922"    
end)

Me:AddButton("Blue Beastmode", function(IhateGayPeople)
           local L_412_ = game.Players.LocalPlayer.Character
	local L_413_ = L_412_:WaitForChild("Head")
	local L_414_ = L_413_:WaitForChild("face")
	L_414_.Texture = "rbxassetid://209995252"    
end)

Me:AddButton("Pink Bubble Trouble", function(IhateGayPeople)
           local L_412_ = game.Players.LocalPlayer.Character
	local L_413_ = L_412_:WaitForChild("Head")
	local L_414_ = L_413_:WaitForChild("face")
	L_414_.Texture = "rbxassetid://19264845"    
end)

Me:AddButton("Green Bubble Trouble", function(IhateGayPeople)
           local L_412_ = game.Players.LocalPlayer.Character
	local L_413_ = L_412_:WaitForChild("Head")
	local L_414_ = L_413_:WaitForChild("face")
	L_414_.Texture = "rbxassetid://380754227"    
end)

Me:AddButton("Purple Bubble Trouble", function(IhateGayPeople)
           local L_412_ = game.Players.LocalPlayer.Character
	local L_413_ = L_412_:WaitForChild("Head")
	local L_414_ = L_413_:WaitForChild("face")
	L_414_.Texture = "rbxassetid://362051999"    
end)

Me:AddButton("Blue Bubble Trouble", function(IhateGayPeople)
           local L_412_ = game.Players.LocalPlayer.Character
	local L_413_ = L_412_:WaitForChild("Head")
	local L_414_ = L_413_:WaitForChild("face")
	L_414_.Texture = "rbxassetid://330296924"    
end)

Me:AddButton("Yum!", function(IhateGayPeople)
                local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://26018945" 
end)

Me:AddButton("Super Super Happy Face", function(IhateGayPeople)
             local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://494290547"   
end)

Me:AddButton("Prankster", function(IhateGayPeople)
                  local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://20052028"   
end)

Me:AddButton("Trouble Maker", function(IhateGayPeople)
           local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://22920500"    
end)

Me:AddButton("Meanie", function(IhateGayPeople)
               local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://508490451"   
end)

Me:AddButton("Stitchface", function(IhateGayPeople)
             local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://8329438"    
end)

Me:AddButton("Madness", function(IhateGayPeople)
                  local L_415_ = game.Players.LocalPlayer.Character
	local L_416_ = L_415_:WaitForChild("Head")
	local L_417_ = L_416_:WaitForChild("face")
	L_417_.Texture = "rbxassetid://129900258"   
end)

local Universal = Universal:CreateSector("Universal Shit", "left") 

Universal:AddButton("Hitbox Extender", function(IhateGayPeople)
  _G.HeadSize = 30
_G.Disabled = true

game:GetService('RunService').RenderStepped:connect(function()
if _G.Disabled then
for i,v in next, game:GetService('Players'):GetPlayers() do
if v.Name ~= game:GetService('Players').LocalPlayer.Name then
pcall(function()
v.Character.HumanoidRootPart.Size = Vector3.new(_G.HeadSize,_G.HeadSize,_G.HeadSize)
v.Character.HumanoidRootPart.Transparency = 0.7
v.Character.HumanoidRootPart.BrickColor = BrickColor.new("Really blue")
v.Character.HumanoidRootPart.Material = "Neon"
v.Character.HumanoidRootPart.CanCollide = false
end)
end
end
end
end)  
end)



Universal:AddTextbox("Hitbox Size", nil, function(State)
  _G.HeadSize = State
end)

Universal:AddButton("Triggerbot (T)", function(IhateGayPeople)
    ---triggerbot
-- Settings
local HoldClick = true
local Hotkey = 't' 
local HotkeyToggle = true 

local Players = game:GetService('Players')
local RunService = game:GetService('RunService')

local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

local Toggle = (Hotkey ~= '')
local CurrentlyPressed = false

Mouse.KeyDown:Connect(function(key)
	if HotkeyToggle == true and key == Hotkey then
		Toggle = not Toggle
	elseif 
		key == Hotkey then
		Toggle = true
	end
end)

Mouse.KeyUp:Connect(function(key)
	if HotkeyToggle ~= true and key == Hotkey then
		Toggle = false
	end
end)

RunService.RenderStepped:Connect(function()
	if Toggle then
		if Mouse.Target then
			if Mouse.Target.Parent:FindFirstChild('Humanoid') then
				if HoldClick then
					if not CurrentlyPressed then
						CurrentlyPressed = true
						mouse1press()
					end
				else
					mouse1click()
				end
			else
				if HoldClick then
					CurrentlyPressed = false
					mouse1release()
				end
			end
		end
	end
end)
end)

Universal:AddButton("Universal Silent Aim (C)", function(IhateGayPeople)
    getgenv().Prediction =  (  0  )

getgenv().FOV =  (  160  )   -- [ FOV RADIUS: Increases Or Decreases FOV Radius ]

getgenv().AimKey =  (  "c"  )  -- [ TOGGLE KEY: Toggles Silent Aim On And Off ]

local SilentAim = true
local NeckOffSet = Vector3.new(0,-.5,0)
local Players = game:GetService("Players")
local LocalPlayer = game:GetService("Players").LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Camera = game:GetService("Workspace").CurrentCamera
local connections = getconnections(game:GetService("LogService").MessageOut)
for _, v in ipairs(connections) do
	v:Disable()
end

getgenv = getgenv
Drawing = Drawing
getrawmetatable = getrawmetatable
getconnections = getconnections
hookmetamethod = hookmetamethod

local FOV_CIRCLE = Drawing.new("Circle")
FOV_CIRCLE.Visible = true
FOV_CIRCLE.Filled = false
FOV_CIRCLE.Thickness = 1
FOV_CIRCLE.Transparency = .4
FOV_CIRCLE.Color = Color3.new(0, 1, 0)
FOV_CIRCLE.Radius = getgenv().FOV
FOV_CIRCLE.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

local Options = {
	Torso = "HumanoidRootPart";
	Head = "Head";
}

local function MoveFovCircle()
	pcall(function()
		local DoIt = true
		spawn(function()
			while DoIt do task.wait()
				FOV_CIRCLE.Position = Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
			end
		end)
	end)
end coroutine.wrap(MoveFovCircle)()

Mouse.KeyDown:Connect(function(KeyPressed)
	if KeyPressed == (getgenv().AimKey:lower()) then
		if SilentAim == false then
			FOV_CIRCLE.Color = Color3.new(0, 1, 0)
			SilentAim = true
		elseif SilentAim == true then
			FOV_CIRCLE.Color = Color3.new(1, 0, 0)
			SilentAim = false
		end
	end
end)
Mouse.KeyDown:Connect(function(Rejoin)
	if Rejoin == "=" then
		local LocalPlayer = game:GetService("Players").LocalPlayer
		game:GetService("TeleportService"):Teleport(game.PlaceId, LocalPlayer)
	end
end)

getgenv().FORCE_REPEAT = {
  "FORIINDEXRETURNADMININGAME";
	"AimLockPsycho";
	"proboy32007";
	"autofarmaccountgrind";
}

local function InRadius()
	local Target = nil
	local Distance = 9e9
	local Players = game:GetService("Players")
	local LocalPlayer = game:GetService("Players").LocalPlayer
	local Camera = game:GetService("Workspace").CurrentCamera
	for _, v in pairs(Players:GetPlayers()) do 
		if not table.find(getgenv().FORCE_REPEAT, v.Name) then
			if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") and v.Character:FindFirstChild("Humanoid") and v.Character:FindFirstChild("Humanoid").Health > 0 then
				local Enemy = v.Character	
				local CastingFrom = CFrame.new(Camera.CFrame.Position, Enemy[Options.Torso].CFrame.Position) * CFrame.new(0, 0, -4)
				local RayCast = Ray.new(CastingFrom.Position, CastingFrom.LookVector * 9000)
				local World, ToSpace = game:GetService("Workspace"):FindPartOnRayWithIgnoreList(RayCast, {LocalPlayer.Character:FindFirstChild("Head")})
				local RootWorld = (Enemy[Options.Torso].CFrame.Position - ToSpace).magnitude
				if RootWorld < 4 then		
					local RootPartPosition, Visible = Camera:WorldToViewportPoint(Enemy[Options.Torso].Position)
					if Visible then
						local Real_Magnitude = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(RootPartPosition.X, RootPartPosition.Y)).Magnitude
						if Real_Magnitude < Distance and Real_Magnitude < FOV_CIRCLE.Radius then
							Distance = Real_Magnitude
							Target = Enemy
						end
					end
				end
			end
		end
	end
	return Target
end

local oldIndex = nil
oldIndex = hookmetamethod(game, "__index", function(self, Index, Screw)
	local Screw = oldIndex(self, Index)
	local CALC = Mouse
	local GG = "hit"
	local MONCLER = GG
    if SilentAim then
	if self == CALC and (Index:lower() == MONCLER) then	
		local Enemy = InRadius()
		local Camera = game:GetService("Workspace").CurrentCamera
		if Enemy ~= nil and Enemy[Options.Head] and Enemy:FindFirstChild("Humanoid") and Enemy:FindFirstChild("Humanoid").Health > 0 then
			local Madox = Enemy[Options.Head]
			local Formulate = Madox.CFrame + (Madox.AssemblyLinearVelocity * getgenv().Prediction + NeckOffSet)	
			return (Index:lower() == MONCLER and Formulate)
		end
		return Screw
	end
    end
	return oldIndex(self, Index, Screw)
end)

--Farewell Atman, Nosss, Toru.

end)

Universal:AddTextbox("Silent Aim Prediction", nil, function(State)
getgenv().Prediction = State
end)

Universal:AddTextbox("Silent Aim FOV Size", nil, function(State)
getgenv().FOV = State
end)

local Camlockv = Camlockv:CreateSector("Camlock", "left") 

Camlockv:AddButton("Enable (Q)", function(IhateGayPeople)
    

getgenv().Prediction = 0.15038
getgenv().AimPart = "HumanoidRootPart"
getgenv().Key = "Q"
getgenv().DisableKey = "P"

getgenv().FOV = true
getgenv().ShowFOV = false
getgenv().FOVSize = 55

--// Variables (Service)

local Players = game:GetService("Players")
local RS = game:GetService("RunService")
local WS = game:GetService("Workspace")
local GS = game:GetService("GuiService")
local SG = game:GetService("StarterGui")

--// Variables (regular)

local LP = Players.LocalPlayer
local Mouse = LP:GetMouse()
local Camera = WS.CurrentCamera
local GetGuiInset = GS.GetGuiInset

local AimlockState = true
local Locked
local Victim

local SelectedKey = getgenv().Key
local SelectedDisableKey = getgenv().DisableKey

--// Notification function

function Notify(tx)
    SG:SetCore("SendNotification", {
        Title = "destory.DF",
        Text = tx,
        Duration = 5
    })
end

--// Check if aimlock is loaded

if getgenv().Loaded == true then
    Notify("Aimlock is already loaded!")
    return
end

getgenv().Loaded = true

--// FOV Circle

local fov = Drawing.new("Circle")
fov.Filled = false
fov.Transparency = 1
fov.Thickness = 1
fov.Color = Color3.fromRGB(255, 255, 0)
fov.NumSides = 1000

--// Functions

function update()
    if getgenv().FOV == true then
        if fov then
            fov.Radius = getgenv().FOVSize * 2
            fov.Visible = getgenv().ShowFOV
            fov.Position = Vector2.new(Mouse.X, Mouse.Y + GetGuiInset(GS).Y)

            return fov
        end
    end
end

function WTVP(arg)
    return Camera:WorldToViewportPoint(arg)
end

function WTSP(arg)
    return Camera.WorldToScreenPoint(Camera, arg)
end

function getClosest()
    local closestPlayer
    local shortestDistance = math.huge

    for i, v in pairs(game.Players:GetPlayers()) do
        local notKO = v.Character:WaitForChild("BodyEffects")["K.O"].Value ~= true
        local notGrabbed = v.Character:FindFirstChild("GRABBING_COINSTRAINT") == nil
        
        if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0 and v.Character:FindFirstChild(getgenv().AimPart) and notKO and notGrabbed then
            local pos = Camera:WorldToViewportPoint(v.Character.PrimaryPart.Position)
            local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(Mouse.X, Mouse.Y)).magnitude
            
            if (getgenv().FOV) then
                if (fov.Radius > magnitude and magnitude < shortestDistance) then
                    closestPlayer = v
                    shortestDistance = magnitude
                end
            else
                if (magnitude < shortestDistance) then
                    closestPlayer = v
                    shortestDistance = magnitude
                end
            end
        end
    end
    return closestPlayer
end
 
--// Checks if key is down

Mouse.KeyDown:Connect(function(k)
    SelectedKey = SelectedKey:lower()
    SelectedDisableKey = SelectedDisableKey:lower()
    if k == SelectedKey then
        if AimlockState == true then
            Locked = not Locked
            if Locked then
                Victim = getClosest()

                Notify("Locked onto: "..tostring(Victim.Character.Humanoid.DisplayName))
            else
                if Victim ~= nil then
                    Victim = nil

                    Notify("Unlocked!")
                end
            end
        else
            Notify("Aimlock is not enabled!")
        end
    end
    if k == SelectedDisableKey then
        AimlockState = not AimlockState
    end
end)

--// Loop update FOV and loop camera lock onto target

RS.RenderStepped:Connect(function()
    update()
    if AimlockState == true then
        if Victim ~= nil then
            Camera.CFrame = CFrame.new(Camera.CFrame.p, Victim.Character[getgenv().AimPart].Position + Victim.Character[getgenv().AimPart].Velocity*getgenv().Prediction)
        end
    end
end)
	while wait() do
        if getgenv().AutoPrediction == true then
        local pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
        local split = string.split(pingvalue,'(')
        local ping = tonumber(split[1])
            if ping < 225 then
            getgenv().Prediction = 1.4
        elseif ping < 215 then
            getgenv().Prediction = 1.2
	    elseif ping < 205 then
            getgenv().Prediction = 1.0
	    elseif ping < 190 then
            getgenv().Prediction = 0.10
        elseif ping < 180 then
            getgenv().Prediction = 0.12
	    elseif ping < 170 then
            getgenv().Prediction = 0.15
	    elseif ping < 160 then
            getgenv().Prediction = 0.18
	    elseif ping < 150 then
            getgenv().Prediction = 0.110
        elseif ping < 140 then
            getgenv().Prediction = 0.113
        elseif ping < 130 then
            getgenv().Prediction = 0.116
        elseif ping < 120 then
            getgenv().Prediction = 0.120
        elseif ping < 110 then
            getgenv().Prediction = 0.124
        elseif ping < 105 then
            getgenv().Prediction = 0.127
        elseif ping < 90 then
            getgenv().Prediction = 0.130
        elseif ping < 80 then
            getgenv().Prediction = 0.133
        elseif ping < 70 then
            getgenv().Prediction = 0.136
        elseif ping < 60 then
            getgenv().Prediction = 0.15038
        elseif ping < 50 then
            getgenv().Prediction = 0.15038
        elseif ping < 40 then
            getgenv().Prediction = 0.145
        elseif ping < 30 then
            getgenv().Prediction = 0.155
        elseif ping < 20 then
            getgenv().Prediction = 0.157
        end
        end
	end
end)

Camlockv:AddTextbox("Prediction", nil, function(State)
getgenv().Prediction = State
end)

Camlockv:AddDropdown("Dropdown", {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso"}, "HumanoidRootPart", false, function(dropdown)
getgenv().AimPart = "HumanoidRootPart"
end)

Camlockv:AddToggle("Use FOV", true, function(f)
   getgenv().FOV = f
end)

Camlockv:AddToggle("Show FOV", false, function(f)
   getgenv().ShowFOV = f
end)

Camlockv:AddTextbox("FOV Size", nil, function(State)
getgenv().FOVSize = State
end)

Camlockv:AddToggle("Filled FOV", false, function(f)
 fov.Filled = f  
end)

Camlockv:AddTextbox("FOV Transparency", nil, function(State)
fov.Transparency = State
end)

Camlockv:AddTextbox("FOV Thickness", nil, function(State)
fov.Thickness = State
end)

local ColorToggle = Camlockv:AddToggle("FOV Color", false, function(e)

end)

ColorToggle:AddColorpicker(Color3.fromRGB(75, 0,130), function(ztx)
  fov.Color = ztx
end)

Camlockv:AddTextbox("NumSides", nil, function(State)
fov.NumSides = State
end)


Config:CreateConfigSystem("left") --this is the config system

local n = Config:CreateSector("Imformation", "right") 


n:AddLabel("Made By Slammy", function(State)

end)

n:AddLabel("slammy#5898", function(State)

end)

n:AddLabel("79iware.cc ON TOP", function(State)

end)

