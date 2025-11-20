# SCRIPTlocal AdminUserId = 9861435699

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local AdminAction = ReplicatedStorage:WaitForChild("AdminAction")

-- IDs das armas no ServerStorage
local Weapons = {
    MK = "MK",
    HK = "HK",
    AKM = "AKM",
    PARA = "PARA"
}

local function GiveWeapon(player, weaponName)
    local tool = game.ServerStorage:FindFirstChild(weaponName)
    if tool then
        tool:Clone().Parent = player.Backpack
    end
end

-- AUTO FARM DE ROTAS
local function StartAutoFarmRotas(player)
    task.spawn(function()
        while player:FindFirstChild("AutoFarmRotasAtivo") do
            
            -- Teleporta para o ponto 1
            player.Character:MoveTo(Vector3.new(120, 4, 80))
            task.wait(1.5)

            -- Teleporta para ponto final
            player.Character:MoveTo(Vector3.new(260, 4, 190))
            task.wait(1.5)

            -- Recompensa
            if player:FindFirstChild("Leaderstats") then
                local dinheiro = player.Leaderstats:FindFirstChild("Dinheiro")
                if dinheiro then
                    dinheiro.Value += 300
                end
            end

            task.wait(1.5)
        end
    end)
end

-- AUTO FARM DE UBER
local function StartAutoFarmUber(player)
    task.spawn(function()
        while player:FindFirstChild("AutoFarmUberAtivo") do
            
            -- Pegar passageiro
            player.Character:MoveTo(Vector3.new(500, 4, 330))
            task.wait(2)

            -- Levar passageiro
            player.Character:MoveTo(Vector3.new(700, 4, 520))
            task.wait(2)

            -- Pagamento
            if player:FindFirstChild("Leaderstats") then
                local dinheiro = player.Leaderstats:FindFirstChild("Dinheiro")
                if dinheiro then
                    dinheiro.Value += 650
                end
            end

            task.wait(2)
        end
    end)
end


-- RECEBENDO DO CLIENTE
AdminAction.OnServerEvent:Connect(function(player, action, value)
    if player.UserId ~= AdminUserId then return end

    if action == "GiveWeapon" then
        GiveWeapon(player, value)

    elseif action == "AutoFarmRotas" then
        if value == true then
            local tag = Instance.new("BoolValue")
            tag.Name = "AutoFarmRotasAtivo"
            tag.Parent = player
            StartAutoFarmRotas(player)
        else
            local tag = player:FindFirstChild("AutoFarmRotasAtivo")
            if tag then tag:Destroy() end
        end

    elseif action == "AutoFarmUber" then
        if value == true then
            local tag = Instance.new("BoolValue")
            tag.Name = "AutoFarmUberAtivo"
            tag.Parent = player
            StartAutoFarmUber(player)
        else
            local tag = player:FindFirstChild("AutoFarmUberAtivo")
            if tag then tag:Destroy() end
        end
    end
end)
