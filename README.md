local Config = {
    WindowName = "NPXV HUB",
    Color = Color3.fromRGB(0,183,255),
    Keybind = Enum.KeyCode.RightControl
}

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/AlexR32/Roblox/main/BracketV3.lua"))()
local Window = Library:CreateWindow(Config, game:GetService("CoreGui"))

local Tab1 = Window:CreateTab("Main")
local Section1 = Tab1:CreateSection("Teleport")
local Tab2 = Window:CreateTab("On/Off Ui")
local Section2 = Tab1:CreateSection("Ui")

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

local Button1 = Section1:CreateButton("Sky2", function()
   TP3(CFrame.new(-7837.703125, 5559.31591796875, -465.2409973144531))
end)

local Button1 = Section1:CreateButton("Starter Island", function()
   TP3(CFrame.new(885.473876953125, 18.673505783081055, 1478.0687255859375))
end)

local Button1 = Section1:CreateButton("Island", function()
   TP3(CFrame.new(-690.0905151367188, 15.09426498413086, 1583.0869140625))
end)

local Button1 = Section1:CreateButton("Jungle", function()
   TP3(CFrame.new(-1485.489501953125, 22.852113723754883, 381.3757019042969))
end)

local Button1 = Section1:CreateButton("Marine", function()
   TP3(CFrame.new(-2727.513916015625, 24.48883628845215, 2051.77099609375))
end)

local Toggle1 = Section1:CreateToggle("Fast Attack", false, function(State)
_G.FastAttack  = State
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

local Toggle2 = Section2:CreateToggle("UI Toggle", nil, function(State)
    Window:Toggle(State)
end)
Toggle2:CreateKeybind(tostring(Config.Keybind):gsub("Enum.KeyCode.", ""), function(Key)
    Config.Keybind = Enum.KeyCode[Key]
end)
Toggle2:SetState(true)
