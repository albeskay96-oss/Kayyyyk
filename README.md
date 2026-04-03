local TweenService = game:GetService("TweenService")
local skipButton = script.Parent:WaitForChild("SkipButton") -- Botão de pular na sua UI
local eggModel = workspace:WaitForChild("EggOpening") -- O modelo do ovo no mundo ou Viewport

local isSkipped = false

-- Função para criar e gerenciar a animação
local function playHatchAnimation()
	isSkipped = false
	skipButton.Visible = true

	-- Define o movimento (ex: girar o ovo)
	local tweenInfo = TweenInfo.new(3, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut)
	local goal = {Orientation = Vector3.new(0, 360, 0)}
	local eggTween = TweenService:Create(eggModel, tweenInfo, goal)

	eggTween:Play()

	-- Aguarda a animação acabar OU o jogador clicar em pular
	local startTime = tick()
	repeat 
		task.wait(0.1)
	until (tick() - startTime >= 3) or isSkipped

	-- Interrompe a animação se o jogador pular
	if isSkipped then
		eggTween:Cancel() -- Para o Tween imediatamente
	end

	
end

-- Evento para ativar o pulo
skipButton.MouseButton1Click:Connect(function()
	isSkipped = true
end)
