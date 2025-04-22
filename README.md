### Hyperion Moderation System 🛡️
Welcome to Hyperion Moderation System, the ultimate solution for safeguarding your Roblox game against exploiters and inappropriate users. Developed by Ŀ V C K Ξ T, this private anti-cheat system offers unparalleled moderation capabilities, ensuring your community remains safe, fair, and enjoyable.

## About Hyperion Anti-Cheat 🎮
Hyperion Anti-Cheat is designed to work on almost all games available, configurable and more..., Dynamic User Listing :)

> [!NOTE]
> This Anti-Cheat is NOT a commercial AC. You must DM the creator on Discord to receive the Anti-Cheat. ( This includes testing phases )

## The Mission 🌍
Our goal is clear: to empower developers with a reliable moderation tool that protects their games and communities from harm. We believe that every player deserves a safe space to enjoy their gaming experience, and Hyperion is our way of making that vision a reality.

> [!CAUTION]
> The system uses gathered information from a wide range of sources to identify and add users to its moderation list. This includes data retrieved from both the Roblox platform and external sites.

> [!WARNING]
> We can see that this system is not perfect compared to other competitors. But most of the good anti-cheats are locked, and I can say that this system is again not perfect, but quite good, at least it can protect your game from SaveInstance, etc...

>  [!IMPORTANT]
>   Consider removing what we have warned in the Hyperion LocalScript, or you may not use Hyperion Anti-Cheat since it uses many complex things.

# REPORT + APPEAL

> [!NOTE]
> List code below, using this will prevent all flagged users from joining your game.

https://discord.gg/nPeR5weKJd

```luau
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")

local userCache, groupCache, isUpdating = nil, nil, false

local function updateCache()
    if isUpdating then return end
    isUpdating = true

    local retries = 3
    while retries > 0 do
        local successUsers, resultUsers = pcall(HttpService.GetAsync, HttpService, "https://raw.githubusercontent.com/LVCK3T/Hyperion-Moderation/refs/heads/main/Users", true)
        local successGroups, resultGroups = pcall(HttpService.GetAsync, HttpService, "https://raw.githubusercontent.com/LVCK3T/Hyperion-Moderation/refs/heads/main/Groups", true)
        
        if successUsers and successGroups then
            userCache = resultUsers
            groupCache = resultGroups
            break
        elseif string.match(resultUsers, "exceeded") or string.match(resultGroups, "exceeded") then
            task.wait(30)
        else
            retries -= 1
            task.wait(1)
        end
    end

    isUpdating = false
end

local function checkUser(id)
    return userCache and userCache:find("," .. id .. ",")
end

local function checkGroup(id)
    return groupCache and groupCache:find("," .. id .. ",")
end

Players.PlayerAdded:Connect(function(player)
    if not userCache or not groupCache then updateCache() end
    
    if checkUser(player.UserId) or checkGroup(player.UserId) then
        player:Kick("Access denied. Refer to GitHub Hyperion-Moderation for appeal process.\nYou may reach out to Delta Corporation on Discord.")
    end
end)

task.spawn(function()
    while task.wait(3600) do
        updateCache()
    end
end)
```

That's the end for now.
