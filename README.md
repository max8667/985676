-- Carrega a biblioteca Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Janela Principal
local Window = Rayfield:CreateWindow({
   Name = "Blox Fruits Hub 🏴‍☠️",
   LoadingTitle = "Carregando Scripts...",
   LoadingSubtitle = "por Seu Nome",
   ConfigurationSaving = { Enabled = false }
})

--- =================================================================
--- VARIÁVEIS DE CONTROLE (Para os loops)
--- =================================================================
_G.AutoFarm = false
_G.AutoChest = false

-- Funções Auxiliares de utilidade
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()

--- =================================================================
--- ABAS (TABS)
--- =================================================================
local TabFarm = Window:CreateTab("Auto Farm", 4483362458)
local TabTeleport = Window:CreateTab("Teleportes", 4483362458)
local TabStatus = Window:CreateTab("Status", 4483362458)

--- =================================================================
--- ABA: AUTO FARM
--- =================================================================
TabFarm:CreateSection("Farming Principal")

-- Toggle para Auto Farm (Exemplo lógico de como estruturar)
TabFarm:CreateToggle({
   Name = "Auto Farm Level (Simulado)",
   CurrentValue = false,
   Flag = "ToggleFarm",
   Callback = function(Value)
       _G.AutoFarm = Value
       
       -- Loop do Farm
       spawn(function()
           while _G.AutoFarm do
               task.wait(0.1)
               -- Aqui entraria a lógica de pegar a quest, voar até os NPCs e atacar.
               -- Exemplo visual de ataque (dispara o clique do estilo de luta/espada equipado):
               local VirtualUser = game:GetService('VirtualUser')
               VirtualUser:CaptureController()
               VirtualUser:ClickButton1(Vector2.new(851, 158), game.Workspace.CurrentCamera.CFrame)
           end
       end)
   end,
})

-- Toggle para Coletar Baús automaticamente (Auto Chest)
TabFarm:CreateToggle({
   Name = "Auto Coletar Baús",
   CurrentValue = false,
   Flag = "ToggleChests",
   Callback = function(Value)
       _G.AutoChest = Value
       
       spawn(function()
           while _G.AutoChest do
               task.wait(0.5)
               -- Procura baús no mapa e teleporta o jogador até eles
               for _, v in pairs(workspace:GetChildren()) do
                   if v:IsA("Part") and string.find(v.Name, "Chest") then
                       if _G.AutoChest and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
                           Player.Character.HumanoidRootPart.CFrame = v.CFrame
                           task.wait(0.3) -- Pequena pausa para o jogo registrar a coleta
                       end
                   end
               end
           end
       end)
   end,
})

--- =================================================================
--- ABA: TELEPORTES
--- =================================================================
TabTeleport:CreateSection("Ilhas - Primeira Sea (Exemplo)")

-- Botão de Teleporte usando CFrame
TabTeleport:CreateButton({
   Name = "Teleportar para a Ilha Inicial (Marines)",
   Callback = function()
       if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
           -- Coordenadas aproximadas da ilha inicial Marine (Modifique se necessário)
           Player.Character.HumanoidRootPart.CFrame = CFrame.new(-2855, 7, 5354)
           
           Rayfield:Notify({
              Title = "Teleporte",
              Content = "Você foi teleportado para a Ilha Marine!",
              Duration = 3
           })
       end
   end,
})

TabTeleport:CreateButton({
   Name = "Teleportar para a Ilha dos Macacos (Jungle)",
   Callback = function()
       if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
           -- Coordenadas aproximadas da Jungle
           Player.Character.HumanoidRootPart.CFrame = CFrame.new(-1611, 40, 42)
           
           Rayfield:Notify({
              Title = "Teleporte",
              Content = "Você foi teleportado para a Jungle!",
              Duration = 3
           })
       end
   end,
})

--- =================================================================
--- ABA: STATUS / UPGRADE
--- =================================================================
TabStatus:CreateSection("Distribuir Pontos Automaticamente")

local StatusSelecionado = "Melee"

TabStatus:CreateDropdown({
   Name = "Escolha o Status",
   Options = {"Melee", "Defense", "Sword", "Gun", "Blox Fruit"},
   CurrentOption = {"Melee"},
   MultipleOptions = false,
   Callback = function(Options)
       StatusSelecionado = Options[1]
   end,
})

TabStatus:CreateToggle({
   Name = "Auto Up Status",
   CurrentValue = false,
   Flag = "ToggleStats",
   Callback = function(Value)
       _G.AutoStats = Value
       spawn(function()
           while _G.AutoStats do
               task.wait(0.5)
               -- Dispara o evento remoto oficial do Blox Fruits para subir status
               -- Nota: O jogo frequentemente atualiza o nome desses remotes, certifique-se de testar.
               game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", StatusSelecionado, 1)
           end
       end)
   end,
})
