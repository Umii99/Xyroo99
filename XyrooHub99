-- [[ GARDEN 2 - VORTEX ULTIMATE HUB v13 + ALL-IN-ONE BUY PET TAB WITH NOTIF INTEGRATION ]] --
local LP = game:GetService("Players").LocalPlayer
local WK = game:GetService("Workspace")
local LT = game:GetService("Lighting")
local PG = LP:WaitForChild("PlayerGui")
local HttpService = game:GetService("HttpService")
local TS = game:GetService("TweenService")

-- States Panel Utama
local P, H = false, false
local _G_Farm, _G_SellType, _G_SellAll, _G_CollType, _G_CollAll, _G_PlantType = false, false, false, false, false, false
local _G_BuyType, _G_BuyIn, _G_BuyAll, _G_ShopPanelAct = false, false, false, false
local _G_MailPanelAct, _G_AutoMail = false, false
local _G_NotifPanelAct, _G_PetNotif, _G_ClaimNotif = false, false, false
local _G_WebPanelAct, _G_AutoWebhook = false, false
local _G_EventPanelAct, _G_AutoClaimEvent = false, false
local _G_AutoSendMailNotif = false

-- STATES KHUSUS TAB BUY PET (ENGINE + FILTERS + NOTIF GABUNG DISINI)
local _G_BuyPetPanelAct, _G_AutoBuyPet = false, false
local _G_SubPetBuyNotifAct, _G_AutoBuyPetNotif = false, false -- Buka/Tutup Notif & Sakelar ON/OFF
local SelectedRarities = {}
local SelectedWeights = {}
local SelectedMutations = {}

local WebhookURL = "https://discord.com/api/webhooks/..."
local AllowedPing = "example_user"

local CD_Plant, CD_Sell, CD_Coll = 0.1, 0.1, 0.1
local SelectedFruit, SelectedSeed = "Apple", "Apple Seed"

-- DATA COMPILATION FROM GROW A GARDEN 2 (GaG2)
local GAG2_Rarities = {"Common", "Uncommon", "Rare", "Epic", "Legendary", "Mythical", "Divine"}
local GAG2_Weights = {"Lightweight", "Normal", "Heavy", "Massive", "Titanic"}
local GAG2_Mutations = {"None", "Shiny", "Golden", "Rainbow", "Corrupted", "Negative", "Glitch"}

if PG:FindFirstChild("VortexUltimateHub") then PG["VortexUltimateHub"]:Destroy() end

-- ==========================================================
-- INTERFACE CORE
-- ==========================================================
local G = Instance.new("ScreenGui", PG) G.Name = "VortexUltimateHub" G.ResetOnSpawn = false
local F = Instance.new("Frame", G) F.Size = UDim2.new(0, 390, 0, 280) F.Position = UDim2.new(0.3, 0, 0.25, 0) F.BackgroundColor3 = Color3.fromRGB(12, 12, 16) F.BackgroundTransparency = 0.05 F.Active = true F.Draggable = true
Instance.new("UICorner", F).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", F).Color = Color3.fromRGB(150, 0, 255)

local Sidebar = Instance.new("Frame", F) Sidebar.Size = UDim2.new(0, 105, 1, 0) Sidebar.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
Instance.new("UICorner", Sidebar).CornerRadius = UDim.new(0, 10)

local HubTitle = Instance.new("TextLabel", Sidebar) HubTitle.Size = UDim2.new(1, 0, 0, 35) HubTitle.BackgroundTransparency = 1 HubTitle.Text = "★ VORTEX HUB v13" HubTitle.TextColor3 = Color3.fromRGB(255, 255, 255) HubTitle.Font = Enum.Font.GothamBold HubTitle.TextSize = 11

local TabContainer = Instance.new("Frame", F) TabContainer.Size = UDim2.new(1, -120, 1, -45) TabContainer.Position = UDim2.new(0, 115, 0, 35) TabContainer.BackgroundTransparency = 1

local pages = {}
local function createPage(name)
    local p = Instance.new("ScrollingFrame", TabContainer) p.Size = UDim2.new(1, 0, 1, 0) p.BackgroundTransparency = 1 p.CanvasSize = UDim2.new(0, 0, 0, 680) p.ScrollBarThickness = 3 p.Visible = false
    pages[name] = p return p
end

local P_Graphic = createPage("GRAPHIC")
local P_Farm = createPage("FARM")
local P_Shop = createPage("SHOP")
local P_Mail = createPage("MAIL")
local P_Notif = createPage("NOTIFIKASI")
local P_BuyPet = createPage("BUYPET")
local P_Web = createPage("WEBHOOK")
P_Graphic.Visible = true

local function createTabBtn(name, y, targetPage)
    local tb = Instance.new("TextButton", Sidebar) tb.Size = UDim2.new(1, -12, 0, 20) tb.Position = UDim2.new(0, 6, 0, y) tb.Text = name tb.BackgroundColor3 = Color3.fromRGB(26, 26, 36) tb.TextColor3 = Color3.fromRGB(170, 170, 175) tb.Font = Enum.Font.GothamBold tb.TextSize = 8
    Instance.new("UICorner", tb).CornerRadius = UDim.new(0, 5)
    tb.MouseButton1Click:Connect(function() for _, v in pairs(pages) do v.Visible = false end targetPage.Visible = true end)
end

createTabBtn("OPTIMIZER", 35, P_Graphic)
createTabBtn("AUTO FARM", 57, P_Farm)
createTabBtn("SHOP MGR", 79, P_Shop)
createTabBtn("MAILBOX", 101, P_Mail)
createTabBtn("NOTIFIKASI", 123, P_Notif)
createTabBtn("BUY PET", 145, P_BuyPet)
createTabBtn("WEBHOOK SET", 167, P_Web)

-- ==========================================================
-- HUBUNGAN WEBHOOK TERPUSAT (1 WEB URL)
-- ==========================================================
local function SendDiscordWebhook(title, description, color)
    if WebhookURL == "" or WebhookURL == "https://discord.com/api/webhooks/..." then return end
    local content = AllowedPing ~= "" and "<@"..AllowedPing..">" or ""
    local data = {
        ["content"] = content,
        ["embeds"] = {{
            ["title"] = title,
            ["description"] = description,
            ["color"] = color,
            ["footer"] = {["text"] = "Vortex Engine v13 • Integrated Auto Buy Tab"},
            ["timestamp"] = DateTime.now():ToIsoDate()
        }}
    }
    local request = syn and syn.request or http and http.request or route and route.request or request
    if request then
        pcall(function()
            request({Url = WebhookURL, Method = "POST", Headers = {["Content-Type"] = "application/json"}, Body = HttpService:JSONEncode(data)})
        end)
    end
end

-- FORMAT STRUKTUR REKREASI DATA GACHA VALID USER
local function ReportAutoBuyPet(name, rarity, weight, mutation, price)
    if not _G_AutoBuyPetNotif then return end
    local logDescription = string.format(
        "PRICE: %s\n" ..
        "WEIGHT: %s\n" ..
        "MUT: %s\n" ..
        "RARITY: %s\n" ..
        "NAME PET: %s",
        tostring(price),
        tostring(weight),
        tostring(mutation),
        tostring(rarity),
        tostring(name)
    )
    SendDiscordWebhook("🎁 [PET AUTO BUY NOTIFI] SNAP SUCCESS!", logDescription, 10181046)
end

-- ==========================================================
-- COMPONENT GENERATORS
-- ==========================================================
local function createToggle(parent, txt, y, cb)
    local Box = Instance.new("Frame", parent) Box.Size = UDim2.new(1, -5, 0, 30) Box.Position = UDim2.new(0, 0, 0, y) Box.BackgroundTransparency = 1
    local Lbl = Instance.new("TextLabel", Box) Lbl.Size = UDim2.new(1, -50, 1, 0) Lbl.BackgroundTransparency = 1 Lbl.Text = txt Lbl.TextColor3 = Color3.fromRGB(190, 190, 195) Lbl.Font = Enum.Font.GothamBold Lbl.TextSize = 8.5 Lbl.TextXAlignment = Enum.TextXAlignment.Left
    local Slot = Instance.new("TextButton", Box) Slot.Size = UDim2.new(0, 36, 0, 16) Slot.Position = UDim2.new(1, -42, 0, 7) Slot.BackgroundColor3 = Color3.fromRGB(32, 32, 42) Slot.Text = ""
    Instance.new("UICorner", Slot).CornerRadius = UDim.new(1, 0)
    local Ball = Instance.new("Frame", Slot) Ball.Size = UDim2.new(0, 10, 0, 10) Ball.Position = UDim2.new(0, 3, 0, 3) Ball.BackgroundColor3 = Color3.fromRGB(140, 140, 145)
    Instance.new("UICorner", Ball).CornerRadius = UDim.new(1, 0)
    Slot.MouseButton1Click:Connect(function()
        local s = cb()
        Ball:TweenPosition(s and UDim2.new(0, 23, 0, 3) or UDim2.new(0, 3, 0, 3), "Out", "Quad", 0.1, true)
        Ball.BackgroundColor3 = s and Color3.new(1,1,1) or Color3.fromRGB(140,140,145)
        Slot.BackgroundColor3 = s and Color3.fromRGB(150,0,255) or Color3.fromRGB(32,32,42)
    end)
end

local function createMultiSelectDropdown(parent, label, items, y, storageTable)
    local Box = Instance.new("Frame", parent) Box.Size = UDim2.new(1, -5, 0, 30) Box.Position = UDim2.new(0, 0, 0, y) Box.BackgroundTransparency = 1
    local Lbl = Instance.new("TextLabel", Box) Lbl.Size = UDim2.new(0, 100, 1, 0) Lbl.BackgroundTransparency = 1 Lbl.Text = label Lbl.TextColor3 = Color3.fromRGB(190, 190, 195) Lbl.Font = Enum.Font.GothamBold Lbl.TextSize = 8.5 Lbl.TextXAlignment = Enum.TextXAlignment.Left
    local Btn = Instance.new("TextButton", Box) Btn.Size = UDim2.new(0, 95, 0, 20) Btn.Position = UDim2.new(1, -100, 0, 5) Btn.BackgroundColor3 = Color3.fromRGB(30, 30, 40) Btn.Text = "Pilih (0 Selected)" Btn.TextColor3 = Color3.fromRGB(200, 0, 255) Btn.Font = Enum.Font.GothamBold Btn.TextSize = 8
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 4)
    local curIdx = 1
    Btn.MouseButton1Click:Connect(function()
        local activeItem = items[curIdx]
        if storageTable[activeItem] then storageTable[activeItem] = nil else storageTable[activeItem] = true end
        local total = 0 for _, _ in pairs(storageTable) do total = total + 1 end
        local checkSign = storageTable[activeItem] and " √" or ""
        Btn.Text = activeItem .. checkSign .. " ("..total..")"
        curIdx = curIdx + 1 if curIdx > #items then curIdx = 1 end
    end)
end

local function createTextBox(parent, label, default, y, cb)
    local Box = Instance.new("Frame", parent) Box.Size = UDim2.new(1, -5, 0, 30) Box.Position = UDim2.new(0, 0, 0, y) Box.BackgroundTransparency = 1
    local Lbl = Instance.new("TextLabel", Box) Lbl.Size = UDim2.new(0, 120, 1, 0) Lbl.BackgroundTransparency = 1 Lbl.Text = label Lbl.TextColor3 = Color3.fromRGB(190, 190, 195) Lbl.Font = Enum.Font.GothamBold Lbl.TextSize = 8.5 Lbl.TextXAlignment = Enum.TextXAlignment.Left
    local Txt = Instance.new("TextBox", Box) Txt.Size = UDim2.new(0, 75, 0, 20) Txt.Position = UDim2.new(1, -80, 0, 5) Txt.BackgroundColor3 = Color3.fromRGB(24, 24, 34) Txt.Text = default Txt.TextColor3 = Color3.new(1,1,1) Txt.Font = Enum.Font.GothamBold Txt.TextSize = 8.5
    Instance.new("UICorner", Txt).CornerRadius = UDim.new(0, 4)
    Txt.FocusLost:Connect(function() cb(Txt.Text) end)
end

-- ==========================================================
-- INTEGRASI TOTAL: TAB BUY PET (TERINTEGRASI NOTIF DI DALAMNYA)
-- ==========================================================
local BuyPetPanel = Instance.new("Frame", P_BuyPet) BuyPetPanel.Size = UDim2.new(1, 0, 0, 1) BuyPetPanel.BackgroundTransparency = 1 BuyPetPanel.Visible = false
createToggle(P_BuyPet, "BUKA/TUTUP PANEL BELI PET", 5, function() _G_BuyPetPanelAct = not _G_BuyPetPanelAct BuyPetPanel.Visible = _G_BuyPetPanelAct return _G_BuyPetPanelAct end)

-- 1. Sakelar Utama Engine Gacha (Instant Teleport & Snap)
createToggle(BuyPetPanel, "AUTO BUY PET ENGINE", 35, function()
    _G_AutoBuyPet = not _G_AutoBuyPet
    if _G_AutoBuyPet then
        task.spawn(function()
            local EggStand = WK:FindFirstChild("EggStand") or WK:FindFirstChild("EggShop") or WK:FindFirstChild("Eggs")
            local TargetPosition = nil
            if EggStand then
                local StandPart = EggStand:IsA("Model") and (EggStand:FindFirstChild("HumanoidRootPart") or EggStand:FindFirstChildOfClass("Part")) or EggStand
                if StandPart then TargetPosition = StandPart.CFrame end
            end
            
            while _G_AutoBuyPet do
                pcall(function()
                    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                        local savedPos = LP.Character.HumanoidRootPart.CFrame
                        
                        -- Instant Teleport ke depan Egg Stand
                        if TargetPosition then
                            LP.Character.HumanoidRootPart.CFrame = TargetPosition + Vector3.new(0, 2, 0)
                            task.wait(0.02)
                        end
                        
                        -- Eksekusi instan beli egg
                        local EggPrice = "500 Coins"
                        local resultPet = game:GetService("ReplicatedStorage").RemoteSignals.BuyPetEgg:InvokeServer("Basic_Egg") 
                        
                        -- Kembalikan posisi karakter agar aman
                        LP.Character.HumanoidRootPart.CFrame = savedPos
                        
                        if resultPet then
                            local petName = resultPet.Name or "Unknown Pet"
                            local petRarity = resultPet.Rarity or "Common"
                            local petWeight = resultPet.Weight or "Normal"
                            local petMutation = resultPet.Mutation or "None"
                            
                            local matchRarity = next(SelectedRarities) == nil or SelectedRarities[petRarity]
                            local matchWeight = next(SelectedWeights) == nil or SelectedWeights[petWeight]
                            local matchMutation = next(SelectedMutations) == nil or SelectedMutations[petMutation]
                            
                            if matchRarity and matchWeight and matchMutation then
                                -- Kirim Data ke Log Deskripsi Webhook jika lolos kriteria
                                ReportAutoBuyPet(petName, petRarity, petWeight, petMutation, EggPrice)
                            else
                                -- Hapus pet sampah langsung di tempat
                                game:GetService("ReplicatedStorage").RemoteSignals.DeletePet:FireServer(resultPet.ID)
                            end
                        end
                    end
                end)
                task.wait(0.2)
            end
        end)
    end
    return _G_AutoBuyPet
end)

-- 2. Sistem Filter Pilihan Centang (√)
createMultiSelectDropdown(BuyPetPanel, "RARITY FILTER:", GAG2_Rarities, 65, SelectedRarities)
createMultiSelectDropdown(BuyPetPanel, "WEIGHT FILTER:", GAG2_Weights, 95, SelectedWeights)
createMultiSelectDropdown(BuyPetPanel, "MUTATION FILTER:", GAG2_Mutations, 125, SelectedMutations)

-- ─── LOKASI BARU: SUB-MENU NOTIF SEKARANG BERADA DI DALAM TAB BUY PET DI BAWAH FILTER ───
local InnerNotifPanel = Instance.new("Frame", BuyPetPanel) InnerNotifPanel.Size = UDim2.new(1, 0, 0, 1) InnerNotifPanel.BackgroundTransparency = 1 InnerNotifPanel.Visible = false

createToggle(BuyPetPanel, "PET AUTO BUY NOTIFI", 160, function()
    _G_SubPetBuyNotifAct = not _G_SubPetBuyNotifAct
    InnerNotifPanel.Visible = _G_SubPetBuyNotifAct
    return _G_SubPetBuyNotifAct
end)

createToggle(InnerNotifPanel, "AUTO BUY PET NOTIFIKASI ON/OFF", 190, function()
    _G_AutoBuyPetNotif = not _G_AutoBuyPetNotif
    return _G_AutoBuyPetNotif
end)

-- ==========================================================
-- KONTEN TAB: WEBHOOK SETTING (MURNI KHUSUS IDENTITAS URL SAJA)
-- ==========================================================
local WebPanel = Instance.new("Frame", P_Web) WebPanel.Size = UDim2.new(1, 0, 0, 1) WebPanel.BackgroundTransparency = 1 WebPanel.Visible = false
createToggle(P_Web, "BUKA/TUTUP WEBHOOK CONFIG", 5, function() _G_WebPanelAct = not _G_WebPanelAct WebPanel.Visible = _G_WebPanelAct return _G_WebPanelAct end)
createTextBox(WebPanel, "WEB URL:", "https://discord.com/...", 35, function(val) WebhookURL = val end)
createTextBox(WebPanel, "ALLOWED PINGS/TAGS ID:", "example_user", 65, function(val) AllowedPing = val end)
createToggle(WebPanel, "AUTO SEND NOTIFI TO DISCORD", 95, function() _G_AutoWebhook = not _G_AutoWebhook return _G_AutoWebhook end)
createToggle(WebPanel, "AUTO SEND MAIL BOX NOTIFIKASI", 125, function() _G_AutoSendMailNotif = not _G_AutoSendMailNotif return _G_AutoSendMailNotif end)
