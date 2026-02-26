local D6IS = RegisterMod("Dice synergies with Car Battery!", 1)

local D6 = CollectibleType.COLLECTIBLE_D6
local EternalD6 = CollectibleType.COLLECTIBLE_ETERNAL_D6
local D100 = CollectibleType.COLLECTIBLE_D100
local DInfinity = CollectibleType.COLLECTIBLE_D_INFINITY
local DSpindown = CollectibleType.COLLECTIBLE_SPINDOWN_DICE
local SpindownDouble = true
local useCard = false


if EID then

	local languageCode = "en_us"
	local DicesCarBattery = {
	[D6] = "Rerolls and gives a two item choice.",-- D6
  [EternalD6] = "Rerolls and gives a two item choice.",-- Eternal D6
  [D100] = "When rerolling pedestals, rerolls and gives a two item choice.",-- D100
  [DInfinity] = "If used as a D6 variant, rerolls and gives a two item choice.",-- DInfinity
  [DSpindown] = "| DEFAULT: 2 | Decreases the item internal ID by two or one.",-- Spindown Dice
	} EID:updateDescriptionsViaTable(DicesCarBattery, EID.descriptions[languageCode].carBattery)
end


function D6IS:UsarActivo(ItemActivo, rng, Jugador, useFlags, SlotActiva, VariableData)
  print(SlotActiva)
  if SlotActiva == -1 then return false end
  if (ItemActivo == D6) or (ItemActivo == EternalD6) or (ItemActivo == D100) then
    if Jugador:HasCollectible(CollectibleType.COLLECTIBLE_CAR_BATTERY) then
      useCard = true
      return true
    end
  end

  if (ItemActivo == DInfinity) and (Jugador:HasCollectible(CollectibleType.COLLECTIBLE_CAR_BATTERY)) then
    if (ActiveSlot.VarData == 2) or (ActiveSlot.VarData == 3) or (ActiveSlot.VarData == 9) then
      useCard = true
      return true
    end
  end
end

function D6IS:AntesUsarActivo(ItemActivo, rng, Jugador, useFlags, SlotActiva, VariableData)
  if (ItemActivo == DSpindown) and (SpindownDouble == false) then
    if useFlags & UseFlag.USE_CARBATTERY ~= 0 then
      return true
    end
  end
end

function D6IS:OnUpdate(Jugador)
  if useCard then
    Jugador:UseCard(81, UseFlag.USE_NOANNOUNCER | UseFlag.USE_NOANIM)
    useCard = false
  end
end

local function SaveModConfig()
  if SpindownDouble then
      D6IS:SaveData("true")
  else
      D6IS:SaveData("false")
  end
end

function D6IS:LoadStorage()
  if D6IS:HasData() then
    savedata = D6IS:LoadData()
    if savedata == "true" then
      SpindownDouble = true
    else
      SpindownDouble = false
    end
  end
end

if ModConfigMenu then

ModConfigMenu.AddSetting("Dice synergies with Car Battery", "General", {

  Type = ModConfigMenu.OptionType.BOOLEAN,
  CurrentSetting = function()
    return SpindownDouble
  end,

  Display = function()
  	if SpindownDouble then return "Spindown Dice subtractions: 2"
		else return "Spindown Dice subtractions: 1"
    end
  end,

  OnChange = function(valor)
    SpindownDouble = valor
    if SpindownDouble then
      print("Spindown Dice will decrease items internal ID by TWO.")
    else
      print("Spindown Dice will decrease items internal ID by ONE.")
    end

    SaveModConfig()
  end,

  Info = {
    "Number of rerolls from Spindown Dice with Car Battery",
  }
})
end


D6IS:AddCallback(ModCallbacks.MC_POST_PEFFECT_UPDATE, D6IS.OnUpdate)
D6IS:AddCallback(ModCallbacks.MC_USE_ITEM, D6IS.UsarActivo)
D6IS:AddCallback(ModCallbacks.MC_PRE_USE_ITEM, D6IS.AntesUsarActivo)
