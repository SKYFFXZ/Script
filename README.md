-- SISTEMA DE ABAS (MENU, PLAYER, CONFIG)
local tabContent = Instance.new("Frame", main)
tabContent.Size = UDim2.new(1, -20, 1, -110)
tabContent.Position = UDim2.new(0, 10, 0, 90)
tabContent.BackgroundTransparency = 1

local tabs = {"MENU", "PLAYER", "CONFIG"}
local currentTab = nil
local activeOptions = {}

function clearTabContent()
	for _, child in ipairs(tabContent:GetChildren()) do
		if child:IsA("GuiObject") then
			child:Destroy()
		end
	end
end

function openTab(tabName)
	clearTabContent()
	currentTab = tabName

	local back = Instance.new("TextButton", tabContent)
	back.Size = UDim2.new(0, 120, 0, 30)
	back.Position = UDim2.new(0, 0, 0, 0)
	back.Text = "← Voltar ao menu"
	back.Font = Enum.Font.GothamBold
	back.TextSize = 14
	back.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	back.TextColor3 = Color3.fromRGB(255, 255, 255)
	Instance.new("UICorner", back).CornerRadius = UDim.new(0, 6)

	back.MouseButton1Click:Connect(function()
		clearTabContent()
		currentTab = nil
	end)

	if tabName == "MENU" then
		local btn1 = Instance.new("TextButton", tabContent)
		btn1.Size = UDim2.new(0, 200, 0, 30)
		btn1.Position = UDim2.new(0, 0, 0, 40)
		btn1.Text = "Infinite Stamina"
		btn1.BackgroundColor3 = Color3.fromRGB(0, 170, 127)
		btn1.TextColor3 = Color3.new(1,1,1)
		btn1.Font = Enum.Font.GothamBold
		btn1.TextSize = 14
		Instance.new("UICorner", btn1).CornerRadius = UDim.new(0, 6)

		btn1.MouseButton1Click:Connect(function()
			local args = {[1] = 0/0}
			game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("StaminaService"):WaitForChild("RE"):WaitForChild("DecreaseStamina"):FireServer(unpack(args))
			btn1.Text = "Ativado"
			activeOptions["InfiniteStamina"] = true
		end)

		local emBreve = Instance.new("TextLabel", tabContent)
		emBreve.Size = UDim2.new(0, 200, 0, 30)
		emBreve.Position = UDim2.new(0, 0, 0, 80)
		emBreve.Text = "Em breve..."
		emBreve.BackgroundTransparency = 1
		emBreve.TextColor3 = Color3.fromRGB(255, 255, 255)
		emBreve.Font = Enum.Font.Gotham
		emBreve.TextSize = 14

	elseif tabName == "PLAYER" then
		local esp = Instance.new("TextButton", tabContent)
		esp.Size = UDim2.new(0, 200, 0, 30)
		esp.Position = UDim2.new(0, 0, 0, 40)
		esp.Text = "ESP LINHA ESTILO FFH4X"
		esp.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
		esp.TextColor3 = Color3.new(1,1,1)
		esp.Font = Enum.Font.GothamBold
		esp.TextSize = 14
		Instance.new("UICorner", esp).CornerRadius = UDim.new(0, 6)

		esp.MouseButton1Click:Connect(function()
			activeOptions["ESP"] = true
		end)

		local speedInput = Instance.new("TextBox", tabContent)
		speedInput.PlaceholderText = "WALLCK SPEED (até 70)"
		speedInput.Size = UDim2.new(0, 200, 0, 30)
		speedInput.Position = UDim2.new(0, 0, 0, 80)
		speedInput.BackgroundColor3 = Color3.fromRGB(255,255,255)
		Instance.new("UICorner", speedInput).CornerRadius = UDim.new(0, 6)
		speedInput.FocusLost:Connect(function()
			local val = tonumber(speedInput.Text)
			if val and val <= 70 then
				activeOptions["Speed"] = val
			end
		end)

		local jumpInput = Instance.new("TextBox", tabContent)
		jumpInput.PlaceholderText = "WALLCK JUMP (até 70)"
		jumpInput.Size = UDim2.new(0, 200, 0, 30)
		jumpInput.Position = UDim2.new(0, 0, 0, 120)
		jumpInput.BackgroundColor3 = Color3.fromRGB(255,255,255)
		Instance.new("UICorner", jumpInput).CornerRadius = UDim.new(0, 6)
		jumpInput.FocusLost:Connect(function()
			local val = tonumber(jumpInput.Text)
			if val and val <= 70 then
				activeOptions["Jump"] = val
			end
		end)

	elseif tabName == "CONFIG" then
		local nameBox = Instance.new("TextBox", tabContent)
		nameBox.PlaceholderText = "Nome da config"
		nameBox.Size = UDim2.new(0, 200, 0, 30)
		nameBox.Position = UDim2.new(0, 0, 0, 40)
		Instance.new("UICorner", nameBox).CornerRadius = UDim.new(0, 6)

		local saveBtn = Instance.new("TextButton", tabContent)
		saveBtn.Text = "Salvar"
		saveBtn.Size = UDim2.new(0, 80, 0, 30)
		saveBtn.Position = UDim2.new(0, 210, 0, 40)
		Instance.new("UICorner", saveBtn).CornerRadius = UDim.new(0, 6)
		saveBtn.MouseButton1Click:Connect(function()
			if nameBox.Text ~= "" then
				writefile("wzn_config_"..nameBox.Text..".txt", game:GetService("HttpService"):JSONEncode(activeOptions))
			end
		end)

		local dropdown = Instance.new("TextBox", tabContent)
		dropdown.PlaceholderText = "Nome salvo para carregar"
		dropdown.Size = UDim2.new(0, 200, 0, 30)
		dropdown.Position = UDim2.new(0, 0, 0, 80)
		Instance.new("UICorner", dropdown).CornerRadius = UDim.new(0, 6)

		local loadBtn = Instance.new("TextButton", tabContent)
		loadBtn.Text = "Carregar Configurações"
		loadBtn.Size = UDim2.new(0, 200, 0, 30)
		loadBtn.Position = UDim2.new(0, 0, 0, 120)
		Instance.new("UICorner", loadBtn).CornerRadius = UDim.new(0, 6)
		loadBtn.MouseButton1Click:Connect(function()
			local name = dropdown.Text
			if name and isfile("wzn_config_"..name..".txt") then
				local data = readfile("wzn_config_"..name..".txt")
				activeOptions = game:GetService("HttpService"):JSONDecode(data)
			end
		end)
	end
end

-- CRIAR OS BOTÕES DAS ABAS NA PARTE INFERIOR
local tabContainer = Instance.new("Frame", main)
tabContainer.Size = UDim2.new(1, -20, 0, 120)
tabContainer.Position = UDim2.new(0, 10, 1, -120)
tabContainer.BackgroundTransparency = 1

for i, name in ipairs(tabs) do
	local tab = Instance.new("TextButton", tabContainer)
	tab.Size = UDim2.new(1, 0, 0, 35)
	tab.Position = UDim2.new(0, 0, 0, (i - 1) * 40)
	tab.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
	tab.Text = name
	tab.Font = Enum.Font.GothamBold
	tab.TextSize = 16
	tab.TextColor3 = Color3.fromRGB(255, 255, 255)
	Instance.new("UICorner", tab).CornerRadius = UDim.new(0, 6)

	game:GetService("RunService").RenderStepped:Connect(function()
		local hue = tick() % 5 / 5
		tab.TextColor3 = Color3.fromHSV(hue, 1, 1)
	end)

	tab.MouseButton1Click:Connect(function()
		openTab(name)
	end)
end
