-- Variável para armazenar a força e velocidade do chute
local superKickForce = 1000000
local superKickVelocity = 1000000

-- Função para aumentar a força e velocidade do chute
local function onKickBall(ball)
    -- Verifica se o objeto chutado é uma bola
    if ball:IsA("Part") and ball.Name == "Ball" then
        -- Encontra o gol do adversário
        local opponentGoal = game.Workspace:FindFirstChild("OpponentGoal")
        if opponentGoal then
            -- Calcula a direção do gol do adversário
            local direction = (opponentGoal.Position - ball.Position).unit
            
            -- Aplica a força definida na bola
            local force = Instance.new("BodyVelocity")
            force.Velocity = direction * superKickVelocity
            force.P = superKickForce
            force.MaxForce = Vector3.new(1e9, 1e9, 1e9)
            force.Parent = ball
            
            -- Remove a força após algum tempo para que a bola não continue acelerando indefinidamente
            game.Debris:AddItem(force, 0.1)
        else
            warn("OpponentGoal não encontrado no Workspace")
        end
    end
end

-- Conecta a função ao evento de chute da bola
game.Players.LocalPlayer.CharacterAdded:Connect(function(character)
    local humanoid = character:WaitForChild("Humanoid")
    humanoid.Touched:Connect(onKickBall)
end)
