To fix the minimap appering in the create character and choose spawn follow these staps.

Remove any other hud script (even qb-hud)

-- TOMMY-HUD --

go to tommy-hud/client.

and change line (--REMOVE GTA V HEALTH AND ARMOUR BARS) with below 

--REMOVE GTA V HEALTH AND ARMOUR BARS
function removeMinimapBars()
Citizen.CreateThread(function()
    local minimap = RequestScaleformMovie("minimap")
    SetRadarBigmapEnabled(true, false)
    Wait(0)
    SetRadarBigmapEnabled(false, false)
    while true do
        Wait(0)
        BeginScaleformMovieMethod(minimap, "SETUP_HEALTH_ARMOUR")
        ScaleformMovieMethodAddParamInt(3)
        EndScaleformMovieMethod()
        HideHudComponentThisFrame(7)
				HideHudComponentThisFrame(9)
    end
end)
end

-- QB-MULTICHARACTER --

Go to QB-multicharacter, and change line (local function skyCam(bool)) with below:

local function skyCam(bool)
    TriggerEvent('qb-weathersync:client:DisableSync')
    if bool then
        DoScreenFadeIn(1000)
        SetTimecycleModifier('hud_def_blur')
        SetTimecycleModifierStrength(1.0)
        FreezeEntityPosition(PlayerPedId(), false)
        cam = CreateCamWithParams("DEFAULT_SCRIPTED_CAMERA", Config.CamCoords.x, Config.CamCoords.y, Config.CamCoords.z, 0.0 ,0.0, Config.CamCoords.w, 60.00, false, 0)
        SetCamActive(cam, true)
        RenderScriptCams(true, false, 1, true, true)
		DisplayRadar(false)
		DisplayHud(false)
    else
        SetTimecycleModifier('default')
        SetCamActive(cam, false)
        DestroyCam(cam, true)
        RenderScriptCams(false, false, 1, true, true)
        FreezeEntityPosition(PlayerPedId(), false)
    end
end

-- QB-SPAWN --

Go to QB-spawn, and change line () with below:

local function SetDisplay(bool)
    local translations = {}
		DisplayRadar(false)
		DisplayHud(false)
    for k in pairs(Lang.fallback and Lang.fallback.phrases or Lang.phrases) do
        if k:sub(0, #'ui.') then
            translations[k:sub(#'ui.' + 1)] = Lang:t(k)
        end
    end
    choosingSpawn = bool
    SetNuiFocus(bool, bool)
    SendNUIMessage({
        action = "showUi",
        status = bool,
        translations = translations
    })
end
