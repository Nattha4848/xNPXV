local placeId = game.PlaceId
if placeId == 2753915549 then
	world1 = true
elseif placeId == 4442272183 then
	world2 = true
elseif placeId == 7449423635 then
	world3 = true
else 
end

-----------------------

function TP3(P1)
    Distance = (P1.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
    if Distance < 1000 then
        Speed = 400
    elseif Distance >= 1000 then
        Speed = 250
    end
    game:GetService("TweenService"):Create(
        game.Players.LocalPlayer.Character.HumanoidRootPart,
        TweenInfo.new(Distance/Speed, Enum.EasingStyle.Linear),
        {CFrame = P1}
    ):Play()
    Clip = true
    wait(Distance/Speed)
    Clip = false
end

----------------------------

local DiscordLib = loadstring(game:HttpGet"https://raw.githubusercontent.com/dawid-scripts/UI-Libs/main/discord%20lib.txt")()

local win = DiscordLib:Window("NPXV HUB")

local serv = win:Server("Function", "http://www.roblox.com/asset/?id=6031075938")

local btns = serv:Channel("Auto Farm")

local Weaponlist = {}
local Weapon = nil
 
for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
    table.insert(Weaponlist,v.Name)
end

btns:Dropdown("Select Weapon", Weaponlist, function(bool)
    Weapon = bool
end)

btns:Toggle("Auto Equip",false, function(bool)
    AutoEquiped = bool
end)

spawn(function()
while wait() do
if AutoEquiped then
pcall(function()
game.Players.LocalPlayer.Character.Humanoid:EquipTool(game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(Weapon))
end)
end
end
end)

btns:Toggle("Fast Attack",false, function(bool)
    _G.FastAttack = bool
end)


spawn(function()
    game:GetService("RunService").RenderStepped:Connect(function()
     pcall(function()
        if _G.FastAttack then
        local Combat = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework)
        local Cemara = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework.CameraShaker)
        Cemara.CameraShakeInstance.CameraShakeState = {FadingIn = 3, FadingOut = 2, Sustained = 0, Inactive = 1}
        Combat.activeController.timeToNextAttack = 0
        end
    end)
end) 
end)
 
 
spawn(function()
    game:GetService("RunService").RenderStepped:Connect(function()
     pcall(function()
        if _G.FastAttack then
        game:GetService'VirtualUser':CaptureController()
        game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
        end
    end)
end) 
end)

local drops = serv:Channel("Player")

players = {}

for i,v in pairs(game:GetService("Players"):GetChildren()) do
    table.insert(players,v.Name)
end

local drop = drops:Dropdown("Select Player",players, function(String)
    Select = String
end)

drops:Button("Refresh Players", function()
    DiscordLib:Notification("Notification", "Success !", "Okay!")
    table.clear(players)
for i,v in pairs(game:GetService("Players"):GetChildren()) do
    table.insert(players,v.Name)
end
end)

drops:Button("Teleport To Player", function()
    TP3(game.Players[Select].Character.HumanoidRootPart.CFrame)
end)

drops:Label(".............")

drops:Button("Open Devil Shop",function()
    local args = {
        [1] = "GetFruits"
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    game.Players.localPlayer.PlayerGui.Main.FruitShop.Visible = true
end)

drops:Button("Open Inventory",function()
    local args = {
        [1] = "getInventoryWeapons"
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    game.Players.localPlayer.PlayerGui.Main.Inventory.Visible = true
end)

drops:Button("Open Color Haki",function()
    game.Players.localPlayer.PlayerGui.Main.Colors.Visible = true
end)

drops:Button("Open Title Name",function()
    local args = {
        [1] = "getTitles"
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
    game.Players.localPlayer.PlayerGui.Main.Titles.Visible = true
end)

local sldrs = serv:Channel("Main")

local function round(n)
    return math.floor(tonumber(n) + 0.5)
end
Number = math.random(1, 1000000)
function UpdatePlayerChams()
    for i,v in pairs(game:GetService'Players':GetChildren()) do
        pcall(function()
            if not isnil(v.Character) then
                if ESPPlayer then
                    if not isnil(v.Character.Head) and not v.Character.Head:FindFirstChild('NameEsp'..Number) then
                        local bill = Instance.new('BillboardGui',v.Character.Head)
                        bill.Name = 'NameEsp'..Number
                        bill.ExtentsOffset = Vector3.new(0, 1, 0)
                        bill.Size = UDim2.new(1,200,1,30)
                        bill.Adornee = v.Character.Head
                        bill.AlwaysOnTop = true
                        local name = Instance.new('TextLabel',bill)
                        name.Font = "SourceSansBold"
                        name.FontSize = "Size14"
                        name.TextWrapped = true
                        name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Character.Head.Position).Magnitude/3) ..' M')
                        name.Size = UDim2.new(1,0,1,0)
                        name.TextYAlignment = 'Top'
                        name.BackgroundTransparency = 1
                        name.TextStrokeTransparency = 0.5
                        if v.Team == game.Players.LocalPlayer.Team then
                            name.TextColor3 = Color3.new(0.196078, 0.196078, 0.196078)
                        else
                            name.TextColor3 = Color3.new(1, 0.333333, 0.498039)
                        end
                    else
                        v.Character.Head['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Character.Head.Position).Magnitude/3) ..' M')
                    end
                else
                    if v.Character.Head:FindFirstChild('NameEsp'..Number) then
                        v.Character.Head:FindFirstChild('NameEsp'..Number):Destroy()
                    end
                end
            end
        end)
    end
end
function UpdateChestChams() 
    for i,v in pairs(game.Workspace:GetChildren()) do
        pcall(function()
            if string.find(v.Name,"Chest") then
                if ChestESP then
                    if string.find(v.Name,"Chest") then
                        if not v:FindFirstChild('NameEsp'..Number) then
                            local bill = Instance.new('BillboardGui',v)
                            bill.Name = 'NameEsp'..Number
                            bill.ExtentsOffset = Vector3.new(0, 1, 0)
                            bill.Size = UDim2.new(1,200,1,30)
                            bill.Adornee = v
                            bill.AlwaysOnTop = true
                            local name = Instance.new('TextLabel',bill)
                            name.Font = "SourceSansBold"
                            name.FontSize = "Size14"
                            name.TextWrapped = true
                            name.Size = UDim2.new(1,0,1,0)
                            name.TextYAlignment = 'Top'
                            name.BackgroundTransparency = 1
                            name.TextStrokeTransparency = 0.5
                            if v.Name == "Chest1" then
                                name.TextColor3 = Color3.fromRGB(109, 109, 109)
                                name.Text = ("Chest 1" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' M')
                            end
                            if v.Name == "Chest2" then
                                name.TextColor3 = Color3.fromRGB(173, 158, 21)
                                name.Text = ("Chest 2" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' M')
                            end
                            if v.Name == "Chest3" then
                                name.TextColor3 = Color3.fromRGB(85, 255, 255)
                                name.Text = ("Chest 3" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' M')
                            end
                        else
                            v['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' M')
                        end
                    end
                else
                    if v:FindFirstChild('NameEsp'..Number) then
                        v:FindFirstChild('NameEsp'..Number):Destroy()
                    end
                end
            end
        end)
    end
end
function UpdateDevilChams() 
    for i,v in pairs(game.Workspace:GetChildren()) do
        pcall(function()
            if DevilFruitESP then
                if string.find(v.Name, "Fruit") then   
                    if not v.Handle:FindFirstChild('NameEsp'..Number) then
                        local bill = Instance.new('BillboardGui',v.Handle)
                        bill.Name = 'NameEsp'..Number
                        bill.ExtentsOffset = Vector3.new(0, 1, 0)
                        bill.Size = UDim2.new(1,200,1,30)
                        bill.Adornee = v.Handle
                        bill.AlwaysOnTop = true
                        local name = Instance.new('TextLabel',bill)
                        name.Font = "SourceSansBold"
                        name.FontSize = "Size14"
                        name.TextWrapped = true
                        name.Size = UDim2.new(1,0,1,0)
                        name.TextYAlignment = 'Top'
                        name.BackgroundTransparency = 1
                        name.TextStrokeTransparency = 0.5
                        name.TextColor3 = Color3.fromRGB(255, 0, 0)
                        name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' M')
                    else
                        v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' M')
                    end
                end
            else
                if v.Handle:FindFirstChild('NameEsp'..Number) then
                    v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
                end
            end
        end)
    end
end
function UpdateFlowerChams() 
    for i,v in pairs(game.Workspace:GetChildren()) do
        pcall(function()
            if v.Name == "Flower2" or v.Name == "Flower1" then
                if FlowerESP then 
                    if not v:FindFirstChild('NameEsp'..Number) then
                        local bill = Instance.new('BillboardGui',v)
                        bill.Name = 'NameEsp'..Number
                        bill.ExtentsOffset = Vector3.new(0, 1, 0)
                        bill.Size = UDim2.new(1,200,1,30)
                        bill.Adornee = v
                        bill.AlwaysOnTop = true
                        local name = Instance.new('TextLabel',bill)
                        name.Font = "SourceSansBold"
                        name.FontSize = "Size14"
                        name.TextWrapped = true
                        name.Size = UDim2.new(1,0,1,0)
                        name.TextYAlignment = 'Top'
                        name.BackgroundTransparency = 1
                        name.TextStrokeTransparency = 0.5
                        name.TextColor3 = Color3.fromRGB(255, 0, 0)
                        if v.Name == "Flower1" then 
                            name.Text = ("Blue Flower" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' M')
                            name.TextColor3 = Color3.fromRGB(0, 0, 255)
                        end
                        if v.Name == "Flower2" then
                            name.Text = ("Red Flower" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' M')
                            name.TextColor3 = Color3.fromRGB(255, 0, 0)
                        end
                    else
                        v['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' M')
                    end
                else
                    if v:FindFirstChild('NameEsp'..Number) then
                        v:FindFirstChild('NameEsp'..Number):Destroy()
                    end
                end
            end   
        end)
    end
end

sldrs:Toggle("Esp Chest ",false,function(a)
    ChestESP = a
    while ChestESP do wait()
        UpdateChestChams() 
    end
end)

sldrs:Toggle("Esp Devil Fruit ",false,function(a)
    DevilFruitESP = a
    while DevilFruitESP do wait()
        UpdateDevilChams() 
    end
end)

if game.PlaceId == 4442272183 then

    sldrs:Toggle("Esp Flower ",false,function(a)
        FlowerESP = a
        while FlowerESP do wait()
            UpdateFlowerChams() 
        end
    end)
end

sldrs:Toggle("Anit AFK",false,function(vu)
    DiscordLib:Notification("Notification", "Working", "Okay!")
    local vu = game:GetService("VirtualUser")
    game:GetService("Players").LocalPlayer.Idled:connect(function()
        vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        wait(1)
        vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end)
end)

sldrs:Button("Redeem All Codes", function()
    DiscordLib:Notification("Notification", "Success", "Okay!")
    function UseCode(Text)
        game:GetService("ReplicatedStorage").Remotes.Redeem:InvokeServer(Text)
    end
    UseCode("UPD16")
    UseCode("1MLIKES_RESET")
    UseCode("2BILLION")
    UseCode("UPD15")
    UseCode("FUDD10")
    UseCode("BIGNEWS")
    UseCode("THEGREATACE")
    UseCode("SUB2GAMERROBOT_EXP1")
    UseCode("StrawHatMaine")
    UseCode("Sub2OfficialNoobie")
    UseCode("SUB2NOOBMASTER123")
    UseCode("Sub2Daigrock")
    UseCode("Axiore")
    UseCode("TantaiGaming")
    UseCode("STRAWHATMAINE")
end)

local lbls = serv:Channel("Auto Buy")

lbls:Button("SkyJump ($10,000 Beli)",function()
    local args = {
        [1] = "BuyHaki",
        [2] = "Geppo"
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
end)

lbls:Button("Buso Haki ($25,000 Beli)",function()
    local args = {
        [1] = "BuyHaki",
        [2] = "Buso"
    }

    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
end)

lbls:Button("Observation haki ($750,000 Beli)",function()
    local args = {
        [1] = "KenTalk",
        [2] = "Buy"
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
end)

lbls:Button("Soru ($100,000 Beli)",function()
    local args = {
        [1] = "BuyHaki",
        [2] = "Soru"
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
end)

local clrs = serv:Channel("Auto Start")

clrs:Toggle("Melee",nil, function(value)
    _G.Melee = value
    if _G.Melee == true then
    while _G.Melee do wait()
    local A_1 = "AddPoint"
    local A_2 = "Melee"
    local A_3 = PointStats
    local Event = game:GetService("ReplicatedStorage").Remotes["CommF_"]
    Event:InvokeServer(A_1, A_2, A_3)
    end
    end
end)

clrs:Toggle("Defense",nil, function(value)
    _G.Defense = value
    if _G.Defense == true then
    while _G.Defense do wait()
    local A_1 = "AddPoint"
    local A_2 = "Defense"
    local A_3 = PointStats
    local Event = game:GetService("ReplicatedStorage").Remotes["CommF_"]
    Event:InvokeServer(A_1, A_2, A_3)
    end
    end
end)

clrs:Toggle("Sword",nil, function(value)
    _G.Sword = value
    if _G.Sword == true then
    while _G.Sword do wait()
    local A_1 = "AddPoint"
    local A_2 = "Sword"
    local A_3 = PointStats
    local Event = game:GetService("ReplicatedStorage").Remotes["CommF_"]
    Event:InvokeServer(A_1, A_2, A_3)
    end
    end
end)

clrs:Toggle("Gun",nil, function(value)
    _G.Gun = value
    if _G.Gun == true then
    while _G.Gun do wait()
    local A_1 = "AddPoint"
    local A_2 = "Gun"
    local A_3 = PointStats
    local Event = game:GetService("ReplicatedStorage").Remotes["CommF_"]
    Event:InvokeServer(A_1, A_2, A_3)
    end
    end
end)

clrs:Toggle("Fruit",nil, function(value)
    _G.Bloxfruit = value
    if _G.Bloxfruit == true then
    while _G.Bloxfruit do wait()
    local A_1 = "AddPoint"
    local A_2 = "Demon Fruit"
    local A_3 = PointStats
    local Event = game:GetService("ReplicatedStorage").Remotes["CommF_"]
    Event:InvokeServer(A_1, A_2, A_3)
    end
    end
end)

local tgls = serv:Channel("Setting")

Fly = false
		function activatefly()
			local mouse=game.Players.LocalPlayer:GetMouse''
			localplayer=game.Players.LocalPlayer
			game.Players.LocalPlayer.Character:WaitForChild("HumanoidRootPart")
			local torso = game.Players.LocalPlayer.Character.HumanoidRootPart
			local speedSET=25
			local keys={a=false,d=false,w=false,s=false}
			local e1
			local e2
			local function start()
				local pos = Instance.new("BodyPosition",torso)
				local gyro = Instance.new("BodyGyro",torso)
				pos.Name="EPIXPOS"
				pos.maxForce = Vector3.new(math.huge, math.huge, math.huge)
				pos.position = torso.Position
				gyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
				gyro.cframe = torso.CFrame
				repeat
					wait()
					localplayer.Character.Humanoid.PlatformStand=true
					local new=gyro.cframe - gyro.cframe.p + pos.position
					if not keys.w and not keys.s and not keys.a and not keys.d then
						speed=1
					end
					if keys.w then
						new = new + workspace.CurrentCamera.CoordinateFrame.lookVector * speed
						speed=speed+speedSET
					end
					if keys.s then
						new = new - workspace.CurrentCamera.CoordinateFrame.lookVector * speed
						speed=speed+speedSET
					end
					if keys.d then
						new = new * CFrame.new(speed,0,0)
						speed=speed+speedSET
					end
					if keys.a then
						new = new * CFrame.new(-speed,0,0)
						speed=speed+speedSET
					end
					if speed>speedSET then
						speed=speedSET
					end
					pos.position=new.p
					if keys.w then
						gyro.cframe = workspace.CurrentCamera.CoordinateFrame*CFrame.Angles(-math.rad(speed*15),0,0)
					elseif keys.s then
						gyro.cframe = workspace.CurrentCamera.CoordinateFrame*CFrame.Angles(math.rad(speed*15),0,0)
					else
						gyro.cframe = workspace.CurrentCamera.CoordinateFrame
					end
				until not Fly
				if gyro then 
					gyro:Destroy() 
				end
				if pos then 
					pos:Destroy() 
				end
				flying=false
				localplayer.Character.Humanoid.PlatformStand=false
				speed=0
			end
			e1=mouse.KeyDown:connect(function(key)
				if not torso or not torso.Parent then 
					flying=false e1:disconnect() e2:disconnect() return 
				end
				if key=="w" then
					keys.w=true
				elseif key=="s" then
					keys.s=true
				elseif key=="a" then
					keys.a=true
				elseif key=="d" then
					keys.d=true
				end
			end)
			e2=mouse.KeyUp:connect(function(key)
				if key=="w" then
					keys.w=false
				elseif key=="s" then
					keys.s=false
				elseif key=="a" then
					keys.a=false
				elseif key=="d" then
					keys.d=false
				end
			end)
			start()
		end

        tgls:Toggle("Fly",false,function(Value)
			Fly = Value
			activatefly()
		end)

tgls:Toggle("RTX MODE",false, function(state)
    if state then
        DiscordLib:Notification("Notification", "RTX ON", "Okay!")
        getgenv().mode = "Autumn" -- Choose from Summer and Autumn
            settings().Rendering.QualityLevel = 10
            local a = game.Lighting
            a.Ambient = Color3.fromRGB(33, 33, 33)
            a.Brightness = 6.67
            a.ColorShift_Bottom = Color3.fromRGB(0, 0, 0)
            a.ColorShift_Top = Color3.fromRGB(255, 247, 237)
            a.EnvironmentDiffuseScale = 0.105
            a.EnvironmentSpecularScale = 0.522
            a.GlobalShadows = true
            a.OutdoorAmbient = Color3.fromRGB(51, 54, 67)
            a.ShadowSoftness = 0.04
            a.GeographicLatitude = -15.525
            a.ExposureCompensation = 0.75
            local b = Instance.new("BloomEffect", a)
            b.Enabled = true
            b.Intensity = 0.04
            b.Size = 1900
            b.Threshold = 0.915
            local c = Instance.new("ColorCorrectionEffect", a)
            c.Brightness = 0.176
            c.Contrast = 0.39
            c.Enabled = true
            c.Saturation = 0.2
            c.TintColor = Color3.fromRGB(217, 145, 57)
            if getgenv().mode == "Summer" then
                c.TintColor = Color3.fromRGB(255, 220, 148)
            elseif getgenv().mode == "Autumn" then
                c.TintColor = Color3.fromRGB(217, 145, 57)
            else
                warn("No mode selected!")
                print("Please select a mode")
                b:Destroy()
                c:Destroy()
            end
            local d = Instance.new("DepthOfFieldEffect", a)
            d.Enabled = true
            d.FarIntensity = 0.077
            d.FocusDistance = 21.54
            d.InFocusRadius = 20.77
            d.NearIntensity = 0.277
            local e = Instance.new("ColorCorrectionEffect", a)
            e.Brightness = 0
            e.Contrast = -0.07
            e.Saturation = 0
            e.Enabled = true
            e.TintColor = Color3.fromRGB(255, 247, 239)
            local e2 = Instance.new("ColorCorrectionEffect", a)
            e2.Brightness = 0.2
            e2.Contrast = 0.45
            e2.Saturation = -0.1
            e2.Enabled = true
            e2.TintColor = Color3.fromRGB(255, 255, 255)
            local s = Instance.new("SunRaysEffect", a)
            s.Enabled = true
            s.Intensity = 0.01
            s.Spread = 0.146
    else
        DiscordLib:Notification("Notification", "RTX OFF", "Okay!")
        local light = game.Lighting
    for i, v in pairs(light:GetChildren()) do
        v:Destroy()
    end

    local ter = workspace.Terrain
    local color = Instance.new("ColorCorrectionEffect")
    local bloom = Instance.new("BloomEffect")
    local sun = Instance.new("SunRaysEffect")
    local blur = Instance.new("BlurEffect")

    color.Parent = light
    bloom.Parent = light
    sun.Parent = light
    blur.Parent = light

    -- enable or disable shit

    local config = {

        Terrain = true;
        ColorCorrection = true;
        Sun = true;
        Lighting = true;
        BloomEffect = true;

    }

    -- settings {

    color.Enabled = false
    color.Contrast = 0.15
    color.Brightness = 0.1
    color.Saturation = 0.25
    color.TintColor = Color3.fromRGB(255, 222, 211)

    bloom.Enabled = false
    bloom.Intensity = 0.1

    sun.Enabled = false
    sun.Intensity = 0.2
    sun.Spread = 1

    bloom.Enabled = false
    bloom.Intensity = 0.05
    bloom.Size = 32
    bloom.Threshold = 1

    blur.Enabled = false
    blur.Size = 6

    -- settings }


    if config.ColorCorrection then
        color.Enabled = true
    end


    if config.Sun then
        sun.Enabled = true
    end


    if config.Terrain then
        -- settings {
        ter.WaterColor = Color3.fromRGB(10, 10, 24)
        ter.WaterWaveSize = 0.1
        ter.WaterWaveSpeed = 22
        ter.WaterTransparency = 0.9
        ter.WaterReflectance = 0.05
        -- settings }
    end


    if config.Lighting then
        -- settings {
        light.Ambient = Color3.fromRGB(0, 0, 0)
        light.Brightness = 4
        light.ColorShift_Bottom = Color3.fromRGB(0, 0, 0)
        light.ColorShift_Top = Color3.fromRGB(0, 0, 0)
        light.ExposureCompensation = 0
        light.FogColor = Color3.fromRGB(132, 132, 132)
        light.GlobalShadows = true
        light.OutdoorAmbient = Color3.fromRGB(112, 117, 128)
        light.Outlines = false
        -- settings }
    end
    end
end)
