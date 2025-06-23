-- MiniCity Secure UI com funções adaptadas do script Rayfield

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Evita múltiplas UIs
if CoreGui:FindFirstChild("MiniCitySecureUI") then
    CoreGui.MiniCitySecureUI:Destroy()
end

-- Proteção
local function proteger(obj)
    if syn and syn.protect_gui then
        syn.protect_gui(obj)
    elseif get_hidden_gui or gethui then
        local hiddenUI = get_hidden_gui and get_hidden_gui() or gethui()
        obj.Parent = hiddenUI
        return
    end
    obj.Parent = CoreGui
end

-- UI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MiniCitySecureUI"
ScreenGui.ResetOnSpawn = false
proteger(ScreenGui)

-- Frame
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 320, 0, 300)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.BackgroundTransparency = 0.2
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.Parent = ScreenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = MainFrame

-- Título
local Title = Instance.new("TextLabel")
Title.Text = "Mini City - Painel Arthur"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20
Title.Parent = MainFrame

-- Botão: AUTO ROUBAR
local PuxarButton = Instance.new("TextButton")
PuxarButton.Size = UDim2.new(0.8, 0, 0, 40)
PuxarButton.Position = UDim2.new(0.1, 0, 0.25, 0)
PuxarButton.Text = "AUTO ROUBAR"
PuxarButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
PuxarButton.TextColor3 = Color3.fromRGB(255, 255, 255)
PuxarButton.Font = Enum.Font.Gotham
PuxarButton.TextSize = 16
PuxarButton.Parent = MainFrame

local puxarCorner = Instance.new("UICorner")
puxarCorner.CornerRadius = UDim.new(0, 8)
puxarCorner.Parent = PuxarButton

PuxarButton.MouseButton1Click:Connect(function()
    -- Função para deletar todas as NotifyGui
    local function deletarNotifyGui()
        local playerGui = game.Players.LocalPlayer:WaitForChild("PlayerGui")
        for _, gui in ipairs(playerGui:GetChildren()) do
            if gui.Name == "NotifyGui" and gui:IsA("ScreenGui") then
                gui:Destroy() -- Deleta a NotifyGui
            end
        end
    end

    -- Lista de itens para pegar
    local itens = {"AK47", "Uzi", "Glock17", "Glock 17", "Parafal", "PARAFAL", "Faca", "IA2", "G3", "Tratamento", "FACA", "Xbox", "Hi Power", "Natalina", "C4", "AR-15", "AR15", "Escudo", "ESCUDO"}

    -- Argumentos para a requisição
    local args = {
        [1] = "mudaInv",
        [2] = "2",
        [4] = "1"
    }

    -- Loop principal
    while true do
        -- Deletar todas as NotifyGui antes de pegar os itens
        deletarNotifyGui()

        -- Pegar itens
        for i, item in ipairs(itens) do
            if i <= 16 then  -- Só tenta alocar até 16 slots
                args[3] = item  -- Atualiza o item para o valor da vez
                args[2] = tostring(i)  -- Atribui o slot dinamicamente (1, 2, 3, ..., 16)

                -- Usar task.spawn() para execução sem delay
                task.spawn(function()
                    -- Envia a requisição para o servidor
                    game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("InvRemotes"):WaitForChild("InvRequest"):InvokeServer(unpack(args))
                end)
            end
        end

        wait(0)  -- Espera um frame para evitar lag
    end
end)

-- Botão: Revistar (Mobile)
local RevistarButton = Instance.new("TextButton")
RevistarButton.Size = UDim2.new(0.8, 0, 0, 40)
RevistarButton.Position = UDim2.new(0.1, 0, 0.5, 0)
RevistarButton.Text = "Revistar (Mobile)"
RevistarButton.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
RevistarButton.TextColor3 = Color3.fromRGB(255, 255, 255)
RevistarButton.Font = Enum.Font.Gotham
RevistarButton.TextSize = 16
RevistarButton.Parent = MainFrame

local revCorner = Instance.new("UICorner")
revCorner.CornerRadius = UDim.new(0, 8)
revCorner.Parent = RevistarButton

RevistarButton.MouseButton1Click:Connect(function()
    local TextChatService = game:GetService("TextChatService")

    local function sendRevistarMessage()
        local channel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
        channel:SendAsync("/revistar morto")
    end

    sendRevistarMessage()
end)

-- Botão: ESP Armas
local ESPButton = Instance.new("TextButton")
ESPButton.Size = UDim2.new(0.8, 0, 0, 40)
ESPButton.Position = UDim2.new(0.1, 0, 0.75, 0)
ESPButton.Text = "ESP Armas (Nil Backpack)"
ESPButton.BackgroundColor3 = Color3.fromRGB(100, 200, 100)
ESPButton.TextColor3 = Color3.fromRGB(0, 0, 0)
ESPButton.Font = Enum.Font.Gotham
ESPButton.TextSize = 16
ESPButton.Parent = MainFrame

local espCorner = Instance.new("UICorner")
espCorner.CornerRadius = UDim.new(0, 8)
espCorner.Parent = ESPButton

ESPButton.MouseButton1Click:Connect(function()
    local armasAlvo = {"AR-15", "Uzi", "G3", "PARAFAL", "FACA", "Hi power", "AK47", "IA2", "Planta Limpa", "Planta Suja", "Planta suja", "Planta limpa", "Peça de arma", "Peça de Arma", "Peça De Arma", "USP", "Xbox", "Skate", "SKATE", "C4", "Lockpick"}

    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player then
            local function atualizarESP()
                pcall(function()
                    if plr.Character and plr.Character:FindFirstChild("Head") then
                        local backpack = plr:FindFirstChild("Backpack") or plr:FindFirstChildOfClass("Folder")
                        local itens = {}
                        if backpack then
                            for _, obj in pairs(backpack:GetChildren()) do
                                if table.find(armasAlvo, string.upper(obj.Name)) or table.find(armasAlvo, obj.Name) then
                                    table.insert(itens, obj.Name)
                                end
                            end
                        end

                        -- Adiciona ESP
                        local head = plr.Character:FindFirstChild("Head")
                        if head and #itens > 0 then
                            if not head:FindFirstChild("ESP_Armas") then
                                local tag = Instance.new("BillboardGui")
                                tag.Name = "ESP_Armas"
                                tag.Size = UDim2.new(0, 200, 0, 50)
                                tag.StudsOffset = Vector3.new(0, 2, 0)
                                tag.Adornee = head
                                tag.AlwaysOnTop = true
                                tag.Parent = head

                                local label = Instance.new("TextLabel")
                                label.Size = UDim2.new(1, 0, 1, 0)
                                label.BackgroundTransparency = 1
                                label.TextColor3 = Color3.fromRGB(0, 255, 0)
                                label.TextScaled = true
                                label.Font = Enum.Font.GothamBold
                                label.Text = table.concat(itens, ", ")
                                label.Parent = tag
                            end
                        end
                    end
                end)
            end
            atualizarESP()
        end
    end
end)

-- Arrastar
local dragging, dragInput, dragStart, startPos

MainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = MainFrame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

MainFrame.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X,
			startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)
