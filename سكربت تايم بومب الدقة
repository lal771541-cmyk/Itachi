-- ========================================================
-- SCRIPT: TIME BOMB (ANTI-LAG + BOMB COLOR)
-- DEVELOPER: المطور علاوي المشاكس
-- ========================================================

local players = game:GetService("Players")
local tweenService = game:GetService("TweenService")
local lighting = game:GetService("Lighting")
local localPlayer = players.LocalPlayer

-- [1] إنشاء واجهة التحميل لمدة 5 ثوانٍ
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AlawiLoader"
screenGui.ResetOnSpawn = false
screenGui.Parent = localPlayer:WaitForChild("PlayerGui")

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 360, 0, 180)
mainFrame.Position = UDim2.new(0.5, -180, 0.5, -90)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim me = UDim.new(0, 12)
uiCorner.Parent = mainFrame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 45)
titleLabel.Position = UDim2.new(0, 0, 0, 15)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "المطور علاوي المشاكس"
titleLabel.TextColor3 = Color3.fromRGB(0, 255, 150)
titleLabel.TextSize = 22
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = mainFrame

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, 0, 0, 30)
statusLabel.Position = UDim2.new(0, 0, 0, 65)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "جاري تهيئة الخريطة وتقليل اللاق..."
statusLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
statusLabel.TextSize = 14
statusLabel.Font = Enum.Font.Gotham
statusLabel.Parent = mainFrame

local barBg = Instance.new("Frame")
barBg.Size = UDim2.new(0.85, 0, 0, 10)
barBg.Position = UDim2.new(0.075, 0, 0, 115)
barBg.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
barBg.BorderSizePixel = 0
barBg.Parent = mainFrame

local barCorner = Instance.new("UICorner")
barCorner.CornerRadius = UDim.new(0, 5)
barCorner.Parent = barBg

local barFill = Instance.new("Frame")
barFill.Size = UDim2.new(0, 0, 1, 0)
barFill.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
barFill.BorderSizePixel = 0
barFill.Parent = barBg

local fillCorner = Instance.new("UICorner")
fillCorner.CornerRadius = UDim.new(0, 5)
fillCorner.Parent = barFill

-- تشغيل شريط التحميل لمدة 5 ثوانٍ
local tweenInfo = TweenInfo.new(5, Enum.EasingStyle.Linear)
local tween = tweenService:Create(barFill, tweenInfo, {Size = UDim2.new(1, 0, 1, 0)})
tween:Play()

task.wait(5)

statusLabel.Text = "تم التفعيل بنجاح!"
task.wait(0.5)
screenGui:Destroy()

-- ========================================================
-- [2] قسم تقليل اللاق وتنظيف الخريطة
-- ========================================================

local function optimizePart(v)
    if v:IsA("BasePart") then
        v.Material = Enum.Material.SmoothPlastic
        v.CastShadow = false
    elseif v:IsA("Decal") or v:IsA("Texture") then
        v:Destroy()
    elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
        v.Enabled = false
    end
end

for _, v in pairs(workspace:GetDescendants()) do
    optimizePart(v)
end

lighting.GlobalShadows = false
local blur = lighting:FindFirstChildOfClass("BlurEffect")
if blur then blur:Destroy() end

-- ========================================================
-- [3] قسم التحكم بلون القنبلة
-- ========================================================

local function handleBomb(bomb)
    if bomb:IsA("BasePart") and (bomb.Name:lower():match("bomb") or bomb.Name:lower():match("ticking")) then
        bomb.Color = Color3.fromRGB(255, 255, 255)
        bomb.Material = Enum.Material.Neon
        
        local connection
        connection = bomb:GetPropertyChangedSignal("Color"):Connect(function()
            if not bomb or not bomb.Parent then
                connection:Disconnect()
                return
            end
            if bomb.Color.R > 0.5 and bomb.Color.G < 0.5 then
                bomb.Color = Color3.fromRGB(255, 0, 0)
            end
        end)
    end
end

for _, obj in pairs(workspace:GetDescendants()) do
    handleBomb(obj)
end

workspace.DescendantAdded:Connect(function(descendant)
    optimizePart(descendant)
    if descendant.Name:lower():match("bomb") or descendant.Name:lower():match("ticking") then
        task.defer(function()
            handleBomb(descendant)
        end)
    end
end)

print("تم تفعيل السكربت بواسطة المطور علاوي المشاكس!")
