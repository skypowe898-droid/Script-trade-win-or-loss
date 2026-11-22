
-- CTD TRADE CALCULATOR
-- BLACK & RED EDITION

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Name = "CTDTradeCalculator"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- BOTON PARA ABRIR
local open = Instance.new("TextButton")
open.Parent = gui
open.Size = UDim2.new(0,120,0,40)
open.Position = UDim2.new(0,20,0.5,0)
open.BackgroundColor3 = Color3.fromRGB(180,0,0)
open.Text = "OPEN CTD"
open.TextColor3 = Color3.new(1,1,1)
open.Visible = false

-- VENTANA
local frame = Instance.new("Frame")
frame.Parent = gui
frame.Size = UDim2.new(0,300,0,260)
frame.Position = UDim2.new(0.5,-150,0.5,-130)
frame.BackgroundColor3 = Color3.fromRGB(20,20,20)
frame.BorderSizePixel = 0

-- BARRA SUPERIOR
local top = Instance.new("Frame")
top.Parent = frame
top.Size = UDim2.new(1,0,0,35)
top.BackgroundColor3 = Color3.fromRGB(150,0,0)

local title = Instance.new("TextLabel")
title.Parent = top
title.Size = UDim2.new(1,-40,1,0)
title.BackgroundTransparency = 1
title.Text = "CTD TRADE CALCULATOR"
title.TextColor3 = Color3.new(1,1,1)
title.TextSize = 15

-- CERRAR
local close = Instance.new("TextButton")
close.Parent = top
close.Size = UDim2.new(0,35,1,0)
close.Position = UDim2.new(1,-35,0,0)
close.BackgroundColor3 = Color3.fromRGB(100,0,0)
close.Text = "X"
close.TextColor3 = Color3.new(1,1,1)

close.MouseButton1Click:Connect(function()
    frame.Visible = false
    open.Visible = true
end)

open.MouseButton1Click:Connect(function()
    frame.Visible = true
    open.Visible = false
end)

-- MOVIMIENTO DE VENTANA
local dragging = false
local dragStart
local startPos

top.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
    or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
    or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

-- ENTRADAS
local function createInput(text, y)
    local box = Instance.new("TextBox")
    box.Parent = frame
    box.Size = UDim2.new(0,250,0,35)
    box.Position = UDim2.new(0,25,0,y)
    box.BackgroundColor3 = Color3.fromRGB(40,40,40)
    box.TextColor3 = Color3.new(1,1,1)
    box.PlaceholderText = text
    box.Text = ""
    box.TextSize = 14
    box.ClearTextOnFocus = false
    return box
end

local you = createInput("Tu valor",55)
local other = createInput("Valor rival",100)

-- COMPARAR
local compare = Instance.new("TextButton")
compare.Parent = frame
compare.Size = UDim2.new(0,250,0,35)
compare.Position = UDim2.new(0,25,0,150)
compare.BackgroundColor3 = Color3.fromRGB(170,0,0)
compare.Text = "COMPARAR TRADE"
compare.TextColor3 = Color3.new(1,1,1)

local result = Instance.new("TextLabel")
result.Parent = frame
result.Size = UDim2.new(1,0,0,40)
result.Position = UDim2.new(0,0,0,195)
result.BackgroundTransparency = 1
result.TextColor3 = Color3.new(1,1,1)
result.Text = "Esperando valores..."

compare.MouseButton1Click:Connect(function()
    local a = tonumber(you.Text)
    local b = tonumber(other.Text)

    if not a or not b or a < 0 or b < 0 then
        result.Text = "Introduce valores validos"
        return
    end

    if a > b then
        result.Text = "TU GANAS EL TRADE"
    elseif a < b then
        result.Text = "EL RIVAL GANA"
    else
        result.Text = "TRADE EQUILIBRADO"
    end
end)
