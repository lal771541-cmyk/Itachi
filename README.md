-- ========================================================
-- SCRIPT: TIME BOMB (FULL SCREEN LOADER + ANTI-LAG)
-- DEVELOPER: برمجة سكربت بواسطة علاوي المشاكس👨🏻‍💻
-- ========================================================

local players = game:GetService("Players")
local tweenService = game:GetService("TweenService")
local lighting = game:GetService("Lighting")
local localPlayer = players.LocalPlayer

-- [1] إنشاء واجهة تحميل على كامل الشاشة (Full Screen)
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AlawiFullScreenLoader"
screenGui.ResetOnSpawn = false
screenGui.IgnoreGuiInset = true -- لجعلها تغطي الشاشة بالكامل بما فيها الشريط العلوي

pcall(function()
    screenGui.Parent = localPlayer:WaitForChild("PlayerGui", 5)
end)

-- الخلفية السوداء بالكامل
local fullBackground = Instance.new("Frame")
fullBackground.Size = UDim2.new(1, 0, 1, 0)
fullBackground.Position = UDim2.new(0, 0, 0, 0)
fullBackground.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
fullBackground.BorderSizePixel = 0
fullBackground.Parent = screenGui

-- عنوان البرمجة
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.Position = UDim2.new(0, 0, 0.4, -40)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "برمجة سكربت بواسطة علاوي المشاكس👨🏻‍💻"
titleLabel.TextColor3 = Color3.fromRGB(0, 255, 150)
titleLabel.TextSize = 22
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.Parent = fullBackground

-- حالة التحميل
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, 0, 0, 30)
statusLabel.Position = UDim2.new(0, 0, 0.4, 15)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "جاري تهيئة الخريطة وتقليل اللاق..."
statusLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
statusLabel.TextSize = 15
statusLabel.Font = Enum.Font.SourceSans
statusLabel.Parent = fullBackground

-- خلفية شريط التحميل (ProgressBar Background)
local barBg = Instance.new("Frame")
barBg.Size = UDim2.new(0.6, 0, 0, 12)
barBg.Position = UDim2.new(0.2, 0, 0.4, 55)
barBg.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
barBg.BorderSizePixel = 0
barBg.Parent = fullBackground

local barBgCorner = Instance.new("UICorner")
barBgCorner.CornerRadius = UDim.new(0, 6)
barBgCorner.Parent = barBg

-- الخط الممتلئ (Fill Bar)
local barFill = Instance.new("Frame")
barFill.Size = UDim2.new(0, 0, 1, 0)
barFill.BackgroundColor3 = Color3.fromRGB(0, 255, 150)
barFill.BorderSizePixel = 0
barFill.Parent = barBg

local barFillCorner = Instance.new("UICorner")
barFillCorner.CornerRadius = UDim.new(0, 6)
barFillCorner.Parent = barFill

-- تحريك الشريط ليتملأ خلال 5 ثوانٍ
local tweenInfo = TweenInfo.new(5, Enum.EasingStyle.Linear)
local tween = tweenService:Create(barFill, tweenInfo, {Size = UDim2.new(1, 0, 1, 0)})
tween:Play()

-- إخفاء الشاشة بعد اكتمال التحميل
task.spawn(function()
    task.wait(5)
    statusLabel.Text = "تم التفعيل بنجاح!"
    task.wait(0.5)
    if screenGui then
        screenGui:Destroy()
    end
end)

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
                if connection then connection:Disconnect() end
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

print("تم تفعيل السكربت بنجاح!")
