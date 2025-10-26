local player = game.Players.LocalPlayer
local players = game:GetService("Players")
local RunService = game:GetService("RunService")

local function criarPainel()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "PainelKaua"
    screenGui.Parent = player:WaitForChild("PlayerGui")

    -- Painel principal
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 200, 0, 120)
    frame.Position = UDim2.new(0.5, -100, 0.5, -60)
    frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    frame.BorderSizePixel = 0
    frame.Active = true
    frame.Draggable = true
    frame.Parent = screenGui

    -- Nome colorido rainbow (letra por letra)
    local nome = "kauascriptmush"
    local letras = {}
    local spacing = 14

    -- Remove textos antigos (caso existam)
    for _, v in pairs(frame:GetChildren()) do
        if v.Name == "LetraColorida" then
            v:Destroy()
        end
    end

    for i = 1, #nome do
        local letra = Instance.new("TextLabel")
        letra.Name = "LetraColorida"
        letra.BackgroundTransparency = 1
        letra.Size = UDim2.new(0, spacing, 0.4, 0)
        letra.Position = UDim2.new(0, (i-1)*spacing + 10, 0, 0)
        letra.Text = nome:sub(i,i)
        letra.Font = Enum.Font.GothamBold
        letra.TextScaled = true
        letra.TextStrokeTransparency = 0.7
        letra.Parent = frame
        table.insert(letras, letra)
    end

    -- Função de cor rainbow
    local function rainbowColor(t, offset)
        local r = math.floor(127 * math.sin(t + offset) + 128)
        local g = math.floor(127 * math.sin(t + offset + 2*math.pi/3) + 128)
        local b = math.floor(127 * math.sin(t + offset + 4*math.pi/3) + 128)
        return Color3.fromRGB(r, g, b)
    end

    local t = 0
    RunService.Heartbeat:Connect(function(dt)
        t = t + dt * 4
        for i, letra in ipairs(letras) do
            letra.TextColor3 = rainbowColor(t, i * 1.5)
        end
    end)

    -- Botão de Opções
    local btnOpcoes = Instance.new("TextButton")
    btnOpcoes.Size = UDim2.new(1, 0, 0.6, 0)
    btnOpcoes.Position = UDim2.new(0, 0, 0.4, 0)
    btnOpcoes.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    btnOpcoes.Text = "Opções"
    btnOpcoes.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnOpcoes.TextScaled = true
    btnOpcoes.Font = Enum.Font.GothamBold
    btnOpcoes.Parent = frame

    -- Frame de Opções
    local frameOpcoes = Instance.new("Frame")
    frameOpcoes.Size = UDim2.new(0, 200, 0, 140) -- altura reduzida pois tiramos AntibanMic
    frameOpcoes.Position = UDim2.new(0, 0, 0, 120)
    frameOpcoes.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    frameOpcoes.BorderSizePixel = 0
    frameOpcoes.Visible = false
    frameOpcoes.Parent = frame

    -- Infinity Yield
    local btnInfinityYield = Instance.new("TextButton")
    btnInfinityYield.Size = UDim2.new(1, -10, 0, 40)
    btnInfinityYield.Position = UDim2.new(0, 5, 0, 10)
    btnInfinityYield.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    btnInfinityYield.Text = "Infinity Yield"
    btnInfinityYield.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnInfinityYield.TextScaled = true
    btnInfinityYield.Font = Enum.Font.GothamBold
    btnInfinityYield.Parent = frameOpcoes

    btnInfinityYield.MouseButton1Click:Connect(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/DarkNetworks/Infinite-Yield/main/latest.lua'))()
    end)

    -- TP
    local btnTP = Instance.new("TextButton")
    btnTP.Size = UDim2.new(1, -10, 0, 40)
    btnTP.Position = UDim2.new(0, 5, 0, 60)
    btnTP.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    btnTP.Text = "TP"
    btnTP.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnTP.TextScaled = true
    btnTP.Font = Enum.Font.GothamBold
    btnTP.Parent = frameOpcoes

    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(1, -10, 0, 30)
    textBox.Position = UDim2.new(0, 5, 0, 100)
    textBox.PlaceholderText = "Nome do jogador"
    textBox.TextColor3 = Color3.fromRGB(0, 0, 0)
    textBox.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
    textBox.TextScaled = true
    textBox.ClearTextOnFocus = false
    textBox.Visible = false
    textBox.Parent = frameOpcoes

    local btnTPConfirm = Instance.new("TextButton")
    btnTPConfirm.Size = UDim2.new(1, -10, 0, 30)
    btnTPConfirm.Position = UDim2.new(0, 5, 0, 140)
    btnTPConfirm.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    btnTPConfirm.Text = "Teleportar"
    btnTPConfirm.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnTPConfirm.TextScaled = true
    btnTPConfirm.Font = Enum.Font.GothamBold
    btnTPConfirm.Visible = false
    btnTPConfirm.Parent = frameOpcoes

    btnTP.MouseButton1Click:Connect(function()
        textBox.Visible = not textBox.Visible
        btnTPConfirm.Visible = not btnTPConfirm.Visible
    end)

    btnTPConfirm.MouseButton1Click:Connect(function()
        local targetName = textBox.Text
        local targetPlayer = players:FindFirstChild(targetName)
        if targetPlayer and targetPlayer.Character and player.Character then
            player.Character:SetPrimaryPartCFrame(targetPlayer.Character.PrimaryPart.CFrame + Vector3.new(0,5,0))
        else
            warn("Jogador não encontrado ou sem personagem.")
        end
    end)

    -- Mostrar/Esconder opções
    btnOpcoes.MouseButton1Click:Connect(function()
        frameOpcoes.Visible = not frameOpcoes.Visible
    end)
end

criarPainel()
player.CharacterAdded:Connect(function()
    criarPainel()
end)
