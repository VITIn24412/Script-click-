if game.PlaceId == 9150032572 then
    -- Load
    local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

    -- Main
    local Window = OrionLib:MakeWindow({Name = "ZARK HUB", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})

    local AutoClickTab = Window:MakeTab({
        Name = "AutoClick",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local AutoClickSection = AutoClickTab:AddSection({
        Name = "AutoClick"
    })

    -- Variable to control AutoClick state
    local autoClickEnabled = false

    -- AutoClick Function
    local function AutoClick()
        while autoClickEnabled do
            workspace.Events.AddClick:FireServer()
            wait(0)  -- Set to 0 for the fastest click speed possible
        end
    end

    -- Add checkbox to toggle AutoClick
    AutoClickSection:AddToggle({
        Name = "Enable AutoClick",
        Default = false,
        Callback = function(value)
            autoClickEnabled = value
            if autoClickEnabled then
                AutoClick()
            end
        end
    })

    -- Create Teleportin Island Tab
    local TeleportinIslandTab = Window:MakeTab({
        Name = "Teleportin Island",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local TeleportinIslandSection = TeleportinIslandTab:AddSection({
        Name = "Teleport"
    })

    -- List of island positions with higher y-coordinates
    local islandPositions = {
        Beach = Vector3.new(-1898.85, 15, -8),
        Lava = Vector3.new(-2202.67, 15, -6.73963),
        Candy = Vector3.new(-2508.24, 15, -0.559545),
        Toy = Vector3.new(-2845.65, 15, 3.1412)
    }

    -- Variable to store selected island
    local selectedIsland = "Beach"

    -- Create Dropdown for selecting island to teleport
    TeleportinIslandSection:AddDropdown({
        Name = "Select Island",
        Default = "Beach",
        Options = {"Beach", "Lava", "Candy", "Toy"},
        Callback = function(selected)
            selectedIsland = selected
        end
    })

    -- Add button to teleport to selected island
    TeleportinIslandSection:AddButton({
        Name = "Tp Island",
        Callback = function()
            local player = game.Players.LocalPlayer
            player.Character.HumanoidRootPart.CFrame = CFrame.new(islandPositions[selectedIsland])
        end
    })

    -- Create AutoEgg Tab
    local AutoEggTab = Window:MakeTab({
        Name = "AutoEgg",
        Icon = "rbxassetid://4483345998",
        PremiumOnly = false
    })

    local AutoEggSection = AutoEggTab:AddSection({
        Name = "AutoEgg"
    })

    -- Variable to store selected island and AutoOpen state
    local selectedIsland = "Beach"
    local autoOpenEnabled = false

    -- Function to open eggs
    local function OpenEgg(island, type)
        local args = {
            [1] = island,
            [2] = type
        }
        game:GetService("ReplicatedStorage").RemoteEvents.EggOpened:InvokeServer(unpack(args))
    end

    -- List of islands
    local islands = {
        "Beach", "Lava", "Doce", "Toy", "Alienigena", "Void",
        "Prism", "Ocean", "Toxic", "Heaven", "Hell"
    }

    -- Create Dropdown for selecting island
    AutoEggSection:AddDropdown({
        Name = "Select Island",
        Default = "Beach",
        Options = islands,
        Callback = function(selected)
            selectedIsland = selected
        end
    })

    -- AutoOpen Function
    local function AutoOpen()
        while autoOpenEnabled do
            OpenEgg(selectedIsland, "Single")
            wait(1)  -- Adjust the wait time as needed
        end
    end

    -- Add checkbox to toggle AutoOpen
    AutoEggSection:AddToggle({
        Name = "Auto Open",
        Default = false,
        Callback = function(value)
            autoOpenEnabled = value
            if autoOpenEnabled then
                AutoOpen()
            end
        end
    })
end
