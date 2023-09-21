- 👋 Hi, I’m @animelio
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
animelio/animelio is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
loadstring(game:HttpGet('https://raw.githubusercontent.com/1201for/littlegui/main/The-Wild-One'))()

   WindowName = "V.G Hub",
	Color = Color3.fromRGB(255,128,64),
	Keybind = Enum.KeyCode.RightControl
}
repeat wait() until game:IsLoaded() wait()
game:GetService("Players").LocalPlayer.Idled:connect(function()
game:GetService("VirtualUser"):ClickButton2(Vector2.new())
end)
local OldNameCall = nil
OldNameCall = hookmetamethod(game, "__namecall", function(self, ...)
    local Args = {...}
    local Self = Args[1]
    if getnamecallmethod()=="FireServer" and tostring(Self)=="ErrorLoggerRemote" then
            return nil
    end
    return OldNameCall(self, ...)
end)
pcall(function()
for i, v in pairs(getconnections(game:GetService("ScriptContext").Error)) do
    v:Disable()
    game:GetService("ScriptContext").Error:Connect(
        function(...)
            local Arg = {...}
            local i, v =
                pcall(
                function()
                    return Arg[3].Name
                end
            )
            if i == false then
                return
            end
            v:Fire(...)
        end
    )
end
end)
function FreeForAll(v)
    if getgenv().FreeForAll == false then
        if game.Players.LocalPlayer.Team == v.Team then return false
        else return true end
    else return true end
end

local Utils = require(game:GetService("ReplicatedStorage").Modules.Utils.Utils)
local Player = require(game:GetService("ReplicatedStorage").Modules.Character.PlayerCharacter)
local R =  require(game:GetService("ReplicatedStorage").Modules.Load)("Ragdolls")
local Animal = require(game:GetService("ReplicatedStorage").Modules.Load)("Animal")
local AnimalRiding = require(game:GetService("ReplicatedStorage").Modules.Load)("AnimalRiding")
Keys = {}
OreDeposits = {}
for i,v in pairs(game:GetService("Workspace")["WORKSPACE_Interactables"].Mining.OreDeposits:GetChildren()) do
     if v.ClassName == "Folder" and v:FindFirstChildOfClass("Model") then
        table.insert(OreDeposits, v.Name)
    end
end



local Circle = Drawing.new("Circle")
Circle.Color =  Color3.fromRGB(22, 13, 56)
Circle.Thickness = 1
Circle.Radius = 250
Circle.Visible = false
Circle.NumSides = 1000
Circle.Filled = false
Circle.Transparency = 1

game:GetService("RunService").RenderStepped:Connect(function()
    local Mouse = game:GetService("UserInputService"):GetMouseLocation()
    Circle.Position = Vector2.new(Mouse.X, Mouse.Y)
end)
getgenv().AimBot = {
FreeForAll= false,
WallCheck = false,
Enabled = false,
FOV = 250,
Smoothness = 0.25
}



local Shoot = false

function FreeForAll(v)
    if getgenv().AimBot.FreeForAll == false then
        if game.Players.LocalPlayer.Team == v.Team then return false
        else return true end
    else return true end
end

function NotObstructing(i, v)
    if getgenv().AimBot.WallCheck then
        c = workspace.CurrentCamera.CFrame.p
        a = Ray.new(c, i- c)
        f = workspace:FindPartOnRayWithIgnoreList(a, v)
        return f == nil
    else
        return true
    end
end
game:GetService("UserInputService").InputBegan:Connect(function(v)
    if v.UserInputType == Enum.UserInputType.MouseButton2 then
        Shoot = true
    end
end)

game:GetService("UserInputService").InputEnded:Connect(function(v)
    if v.UserInputType == Enum.UserInputType.MouseButton2 then
        Shoot = false
    end
end)

function GetMouse()
    return Vector2.new(game.Players.LocalPlayer:GetMouse().X, game.Players.LocalPlayer:GetMouse().Y)
end
function GetClosestToCuror()
    MousePos = GetMouse()
    Closest = math.huge
    Target = nil
    for _,v in pairs(game:GetService("Players"):GetPlayers()) do
        if FreeForAll(v) then
            if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0  then
                Point,OnScreen = workspace.CurrentCamera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
                if OnScreen and NotObstructing(v.Character.HumanoidRootPart.Position,{game.Players.LocalPlayer.Character,v.Character}) then
                    Distance = (Vector2.new(Point.X,Point.Y) - MousePos).magnitude
                      if Distance <= getgenv().AimBot.FOV then
                          Closest = Distance
                       Target = v
                     end
                  end
               end
            end
         end
    return Target
end 

game:GetService("RunService").RenderStepped:Connect(
    function()
        if getgenv().AimBot.Enabled == false or Shoot == false then
            return
        end
        ClosestPlayer = GetClosestToCuror()
        if ClosestPlayer then
            local Mouse = game:GetService("UserInputService"):GetMouseLocation()
            local TargetPos = game.workspace.Camera:WorldToViewportPoint(ClosestPlayer.Character.HumanoidRootPart.Position)
            mousemoverel(
                (TargetPos.X - Mouse.X) * getgenv().AimBot.Smoothness,
                (TargetPos.Y - Mouse.Y) * getgenv().AimBot.Smoothness
            )
        end
    end
)



local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/1201for/V.G-Hub/main/im-retarded-3"))()
local Window = Library:CreateWindow(Config, game:GetService("CoreGui"))

local Tab1 = Window:CreateTab("The Wild West")
local Tab2 = Window:CreateTab("UI Settings")

local Section1 = Tab1:CreateSection("")
local Section2 = Tab1:CreateSection("")
local Section3 = Tab2:CreateSection("Menu")
local Section4 = Tab2:CreateSection("Background")

local Toggle1 = Section1:CreateToggle("Auto Sprint", nil, function(State)
sex = State
game:GetService("RunService").Stepped:Connect(
    function()
        if sex then
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 25
        end
    end
)

end)
local Toggle1 = Section1:CreateToggle("FullBright", nil, function(State)
FullBright = State
        if FullBright then
            game:GetService("Lighting").Ambient = Color3.new(1, 1, 1)
            game:GetService("Lighting").ColorShift_Bottom = Color3.new(1, 1, 1)
            game:GetService("Lighting").ColorShift_Top = Color3.new(1, 1, 1)
        else
            game:GetService("Lighting").Ambient = Color3.new(0, 0, 0)
            game:GetService("Lighting").ColorShift_Bottom = Color3.new(0, 0, 0)
            game:GetService("Lighting").ColorShift_Top = Color3.new(0, 0, 0)
        end
game.Lighting.Changed:connect(
    function()
        if FullBright then
            game:GetService("Lighting").Ambient = Color3.new(1, 1, 1)
            game:GetService("Lighting").ColorShift_Bottom = Color3.new(1, 1, 1)
            game:GetService("Lighting").ColorShift_Top = Color3.new(1, 1, 1)
        else
            game:GetService("Lighting").Ambient = Color3.new(0, 0, 0)
            game:GetService("Lighting").ColorShift_Bottom = Color3.new(0, 0, 0)
            game:GetService("Lighting").ColorShift_Top = Color3.new(0, 0, 0)
        end
    end
)
end)

local Toggle1 = Section1:CreateToggle("Aimbot", nil, function(State)
    getgenv().AimBot.Enabled = State
end)

local Toggle1 = Section1:CreateToggle("FreeForAll", nil, function(State)
    getgenv().AimBot.FreeForAll = State
end)

local Toggle1 = Section1:CreateToggle("WallCheck", nil, function(State)
    getgenv().AimBot.WallCheck = State
end)

local Slider2 = Section1:CreateSlider("Aimbot Smoothness", 0,10,nil,false, function(Value)
    getgenv().AimBot.Smoothness = Value
end)
local Slider2 = Section1:CreateSlider("Aimbot Radius", 0,1000,nil,false, function(Value)
    getgenv().AimBot.FOV = Value
    Circle.Radius = Value
end)
local Toggle1 = Section1:CreateToggle("Circle Visible", nil, function(State)
   Circle.Visible = State
end)

local Colorpicker3 = Section1:CreateColorpicker("Circle Color", function(Color)
    Circle.Color = Color
end)

local Toggle1 = Section1:CreateToggle("Player Silent Aim", nil, function(State)
getgenv().SilentAim = State
local Ignore = {}
local function ClosestPlayerToCurser()
    local Closest = nil
    local MaxDistance = math.huge
    for i, v in pairs(game:GetService("Players"):GetPlayers()) do
        if
            v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Head") and
                v.Character.Head.PlayerInfo.Screen.HealthContainer.HealthBar.HealthProgressFrame.Size.X.Scale ~= 0 and FreeForAll(v)
         then
            local AimPoint, Part =
