-- DMIDIAOTIMIZA V3.0
-- Inspirado no PHZOptmizer para Delta Executor

local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local StarterGui = game:GetService("StarterGui")
local UserInputService = game:GetService("UserInputService")

-- Estados
local Config = {
    LowGraphics = false,
    ExtremeOpt = false,
    FPSBooster = false,
    RealisticTextures = false,
    SmoothTextures = false,
    LowTextures = false,
    Cinematic = false,
    SharpMode = false,
    BloomReal = false,
    StretchScreen = false
}

-- Criar Menu (usando biblioteca fictícia de UI — note: Delta não tem esse menu pronto, é estrutura profissional para você adaptar)
-- ❗ Muitos menus usam libs como SunSui / Fenrir etc — eu não incluo lib pirata

-- 🚀 FPS BOOST Funções
local function ApplyLowGraphics()
    Config.LowGraphics = not Config.LowGraphics
    local level = Config.LowGraphics and Enum.QualityLevel.Level01 or Enum.QualityLevel.Automatic
    settings().Rendering.QualityLevel = level
    Lighting.GlobalShadows = not Config.LowGraphics
end

local function ApplyExtremeOpt()
    Config.ExtremeOpt = not Config.ExtremeOpt
    if Config.ExtremeOpt then
        Lighting.EnvironmentSpecularScale = 0
        Lighting.EnvironmentDiffuseScale = 0.3
        Lighting.Ambient = Color3.new(0.2,0.2,0.2)
    else
        Lighting.EnvironmentSpecularScale = 1
        Lighting.EnvironmentDiffuseScale = 1
        Lighting.Ambient = Color3.new(0.3,0.3,0.3)
    end
end

local function FPSBoosterFunc()
    Config.FPSBooster = not Config.FPSBooster
    RunService.Heartbeat:Connect(function()
        if Config.FPSBooster then
            settings().Rendering.EjectionMargin = 0.1
        end
    end)
end

-- 🧱 TEXTURES
local function RealisticTexturesFunc()
    Config.RealisticTextures = not Config.RealisticTextures
    if Config.RealisticTextures then
        Lighting.ColorCorrection = Lighting:FindFirstChild("ColorCorrection") or Instance.new("ColorCorrectionEffect", Lighting)
        Lighting.ColorCorrection.Contrast = 0.15
        Lighting.ColorCorrection.Saturation = 0.1
    else
        if Lighting:FindFirstChild("ColorCorrection") then Lighting.ColorCorrection:Destroy() end
    end
end

local function SmoothTexturesFunc()
    Config.SmoothTextures = not Config.SmoothTextures
    -- Simulação suavização (não altera textura diretamente)
    Lighting.Bloom = Lighting:FindFirstChild("Bloom") or Instance.new("BloomEffect", Lighting)
    Lighting.Bloom.Intensity = Config.SmoothTextures and 0.2 or 0
end

local function LowTexturesFunc()
    Config.LowTextures = not Config.LowTextures
    settings().Rendering.EditQualityLevel = Config.LowTextures and 1 or 10
end

-- ✨ EFFECTS
local function CinematicFunc()
    Config.Cinematic = not Config.Cinematic
    Lighting.TiltShift = Lighting:FindFirstChild("TiltShift") or Instance.new("TiltShiftEffect", Lighting)
    Lighting.TiltShift.Intensity = Config.Cinematic and 0.3 or 0
end

local function SharpModeFunc()
    Config.SharpMode = not Config.SharpMode
    Lighting.ColorCorrection = Lighting:FindFirstChild("ColorCorrection") or Instance.new("ColorCorrectionEffect", Lighting)
    Lighting.ColorCorrection.Contrast = Config.SharpMode and 0.25 or 0
end

local function BloomRealFunc()
    Config.BloomReal = not Config.BloomReal
    Lighting.Bloom = Lighting:FindFirstChild("Bloom") or Instance.new("BloomEffect", Lighting)
    Lighting.Bloom.Intensity = Config.BloomReal and 0.35 or 0
    Lighting.Bloom.Threshold = Config.BloomReal and 0.7 or 1
end

local function StretchScreenFunc()
    Config.StretchScreen = not Config.StretchScreen
    -- ❗ Esticar tela real é bloqueado em muitos executores
    -- Versão simulada apenas: não altera resolução real
    StarterGui:SetCore("SendNotification", {
        Title = "DMIDIAOTIMIZA V3.0";
        Text = Config.StretchScreen and "Stretch Screen Ativado (Simulado)!" or "Stretch Screen Desativado";
        Duration = 2;
    })
end

-- ⚙️ SETTINGS
local function ResetAll()
    Config = {
        LowGraphics = false,
        ExtremeOpt = false,
        FPSBooster = false,
        RealisticTextures = false,
        SmoothTextures = false,
        LowTextures = false,
        Cinematic = false,
        SharpMode = false,
        BloomReal = false,
        StretchScreen = false
    }
    Lighting:ClearAllChildren()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Automatic
end

-- 📋 Mapeamento para Menu (precisa de UI lib para aparecer igual à foto)
-- Funções disponíveis para ligar ao menu:
-- ApplyLowGraphics()
-- ApplyExtremeOpt()
-- FPSBoosterFunc()
-- RealisticTexturesFunc()
-- SmoothTexturesFunc()
-- LowTexturesFunc()
-- CinematicFunc()
-- SharpModeFunc()
-- BloomRealFunc()
-- StretchScreenFunc()
-- ResetAll()

StarterGui:SetCore("SendNotification", {
    Title = "DMIDIAOTIMIZA V3.0";
    Text = "Script Carregado! Menu precisa de UI Library para abrir igual à foto";
    Duration = 3;
})
