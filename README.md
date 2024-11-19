local Fluent = loadstring(game:HttpGet("https://raw.githubusercontent.com/Lucas-Lua/ui/main/m"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/ZAZA.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/InterfaceManager.lua.txt"))()


local Window = Fluent:CreateWindow({
	Title = "Savage_Hub",
	SubTitle = "Anime World TD",
	TabWidth = 160,
	Size = UDim2.fromOffset(580, 460),
	Acrylic = false, -- The blur may be detectable, setting this to false disables blur entirely
	Theme = "Dark",
	MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

local Tabs = {
	Main = Window:AddTab({ Title = "Main", Icon = "home" }),
	Game = Window:AddTab({ Title = "Game", Icon = "gamepad" }),
	Macro = Window:AddTab({ Title = "Macro", Icon = "cloud-moon" }),
	Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local interact = function(v2)
	local events = {"Activated","MouseButton1Down","MouseButton1Click","MouseButton1Up"}
	for _,v in pairs(events) do
		for _,connection in pairs(getconnections(v2[v])) do
			connection.Function()
		end
	end
end





local Options = Fluent.Options

do

	Tabs.Main:AddParagraph({

		Title = "Main",

		Content = "Create Wap"
	})
end

local StageSelect = Tabs.Main:AddDropdown("StageSelect", {
	Title = "Select Stage",
	Values = {"Esper City", "Shinobi Battleground", "String Kingdom","Tomb of the Star","Soul Hall", "Katana Revenge","Pillar Cave",
		"Spider MT. Raid", "Katamura City Raid", "Hero City Raid", "MarineFord Raid", "Exploding Planet", "Charuto Bridge",
		"Training Field", "Boss Rush", "Random Unit", "Metal Rush", "Blue Element", "Green Element", "Purple Element",
		"Yellow Element", "Red Element", "Endless Spider Forest", "Endless Snow Hill", "Random Enemy", "Darkness Tower",
		"Fishman Island", "Paradox Invasion", "Victory Valley", "Dream Island", "Ruin Society", "Chaos Return","Shadow Realm", "Idol Concert",
	"Evil Pink Dungeon"},
	Multi = false,
	Default = _G.StageSelect,
	Callback = function(selected)
		_G.StageSelect = selected
	end
})







local DifficultSelect = Tabs.Main:AddDropdown("DifficultSelect", {
	Title = "Select Difficult",
	Values = {"Normal", "Insane", "Nightmare", "Challenger"},
	Multi = false,
	Default = _G.DifficultSelect,
	Callback = function(Difficult)
		_G.DifficultSelect = Difficult
	end
})

local Friendonly = false
local only = false

local toggleOnly = Tabs.Main:AddToggle("toggleOnly", {Title = "Friend Only", Default = _G.FriendOnly })
toggleOnly:OnChanged(function(only)
	_G.FriendOnly = only
end)

spawn(function()
	while task.wait() do
		if _G.FriendOnly then
			only = true
		else
			only = false
		end
	end
end)



local toggleJoin = Tabs.Main:AddToggle("toggleJoin", {Title = "Auto Create", Default = _G.Create })
toggleJoin:OnChanged(function(bool)
	_G.Create = bool
end)


spawn(function()
	while task.wait(1.25) do
		if _G.Create then
	local args = {
		[1] = {
			["StageSelect"] = _G.StageSelect,
			["Image"] = "rbxassetid://122545551626967",
			["FriendOnly"] = only,
			["Difficult"] = _G.DifficultSelect
		}
	}
	game:GetService("ReplicatedStorage").Remote.CreateRoom:FireServer(unpack(args))
	end
	end
end)

function lermnaja()
	local lerm = game:GetService("Players").LocalPlayer.PlayerGui.InRoomUi.RoomUI:FindFirstChild("QuickStart")
	if lerm and lerm.Visible == true then
		lerm.Size = UDim2.new(5000, 5000, 5000, 5000)
		wait(.7)
		local VirtualUser = game:GetService('VirtualUser')
		wait(.7)
		VirtualUser:CaptureController()
		VirtualUser:ClickButton1(Vector2.new(851, 158), game:GetService("Workspace").Camera.CFrame)
	end
end




local toggleStart = Tabs.Main:AddToggle("toggleStart", {Title = "Auto Start", Default = _G.Start })
toggleStart:OnChanged(function(boom)
	_G.Start = boom
end)


spawn(function()
	while true do
		wait(1)
		if _G.Start then
			local lerm = game:GetService("Players").LocalPlayer.PlayerGui.InRoomUi.RoomUI:FindFirstChild("QuickStart")
			
			if lerm and lerm.Visible == true then
				lermnaja()
			else
				repeat
					wait(1)
					local lerm = game:GetService("Players").LocalPlayer.PlayerGui.InRoomUi.RoomUI:FindFirstChild("QuickStart")
				until lerm and lerm.Visible == true 
				
				lermnaja()
			end
		end
	end
end)



local toggleSkip = Tabs.Game:AddToggle("toggleSkip", {Title = "Auto Skip", Default = _G.Skip })
toggleSkip:OnChanged(function(Skip)
	_G.Skip = Skip
end)

spawn(function()
	while task.wait() do
		if _G.Skip then
			if game:GetService("Players").LocalPlayer.PlayerGui.InterFace.Skip.Visible == true then
			game:GetService("ReplicatedStorage").Remote.SkipEvent:FireServer()
			end
		end
	end
end)


local togglespeed = Tabs.Game:AddToggle("togglespeed", {Title = "Auto speedx2", Default = _G.speed })
togglespeed:OnChanged(function(speed)
	_G.speed = speed
end)

spawn(function()
	while task.wait() do
		if _G.speed then
			local bruh = game:GetService("Players").LocalPlayer.PlayerGui.InterFace.Equip.ChangeSpeed:FindFirstChild("tex")
			if bruh.Text == "x1 Speed" then
				game:GetService("ReplicatedStorage").Remote.x2Event:FireServer("x1 Speed")
			end
		end
	end
end)


--macro

local function checkLine(s)
	local a = 0
	for i in string.gmatch(s, "[^\n]+") do
		a = a+1
	end
	return a
end
local start = 1
local function addspace(a)
	local st = ""
	for i = 0,a*2 do
		st = st.." "
	end
	return st
end
local function usd(v,i)
	local turn

	if typeof(v) == "Axes" then
		turn = "Axes.new("..tostring(v)..")"
	elseif typeof(v) == "BrickColor" then
		turn = "BrickColor.new("..tostring(v)..")"
	elseif typeof(v) == "CFrame" then
		turn = "CFrame.new("..tostring(v)..")"
	elseif typeof(v) == "Vector3" then
		turn = "Vector3.new("..tostring(v)..")"
	elseif typeof(v) == "Color3" then
		if v.r<=1 and v.g<= 1 and v.b<= 1 and v.r >= 0 and v.g >= 0 and v.b >= 0 then
			local R = v.r * 255
			local G = v.g * 255
			local B = v.b * 255
			turn = 'Color3.fromRGB('..R..", "..G..", "..B..")"..'    --Color3.new('..tostring(v)..")"
		else
			turn = "Color Error"
		end
	elseif typeof(v) == "Instance" then
		turn = "game."..tostring(v:GetFullName())
	elseif typeof(v) == "Enum" then
		turn = tostring(v).."   -- Enum"
	else
		turn = tostring(typeof(v))..".new("..tostring(v)..")"
	end
	if i then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(turn)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(turn)
	end
	return tostring(turn)
end

local convert
local startSpace = 1
function convert(v,i,a)
	if not a then
		a = startSpace    
	end
	if type(v) == "string" then
		if checkLine(v) >= 2 then
			if type(i) == "number" then 
				return '['..tostring(i)..']'.. " = "..'[['..tostring(v)..']]'
			end
			return '["'..tostring(i)..'"]'.. " = "..'[['..tostring(v)..']]'
		else
			if type(i) == "number" then 
				return '['..tostring(i)..']'.. " = "..'"'..tostring(v)..'"'
			end
			return '["'..tostring(i)..'"]'.. " = "..'"'..tostring(v)..'"'
		end
	elseif type(v) == "number" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	elseif type(v) == "boolean" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	elseif type(v) == "nil" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = nil"
	elseif type(v) == "function" then
		local Name = ""
		if debug.getinfo(v).name == "" then
			Name = tostring(v)
		else
			Name = debug.getinfo(v).name
		end
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..'function()end  '..'--'.."[["..tostring(Name).."]]"
		end
		return '["'..tostring(i)..'"]'.. " = "..'function()end  '..'--'.."[["..tostring(Name).."]]"
	elseif type(v) == "userdata" or type(v) == "vector" then
		return usd(v,i)
	elseif type(v) == "table" then
		if i~= nil then
			if type(i) == "number" then
				stt = '['..tostring(i)..']'.." = "..'{'
			else
				stt = '["'..tostring(i)..'"]'.." = "..'{'
			end

		else
			stt = '{'
		end
		local count_table = 0
		for i,v in pairs(v) do
			count_table = count_table+1
			if count_table == 1 then
				stt = stt .. "\n" .. tostring(addspace(a)) .. tostring(convert(v, i, a + 1))
			else    
				stt = stt .. ",\n" .. tostring(addspace(a)) .. tostring(convert(v, i, a + 1))
			end
		end
		return stt.."\n}"
	elseif type(v) == "thread" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	end
end



local Options = Fluent.Options

do

	Tabs.Macro:AddParagraph({

		Title = "Macro",

		Content = ""
	})
end

local RecordMacroTable = {}

local path = "Ladies_Hub/AWTD/Macro/"
makefolder(path)
local ye = {}
for i,v in pairs(listfiles("Ladies_Hub/AWTD/Macro/")) do 
	table.insert(ye,({v:gsub("Ladies_Hub/AWTD/Macro/","")})[1])
end

local Input = Tabs.Macro:AddInput("Input", {
	Title = "Create Macro",
	Default = _G.nameconfig,
	Placeholder = "Name Macro",
	Numeric = false, -- Only allows numbers
	Finished = true, -- Only calls callback when you press enter
	Callback = function(text)
		print("Input changed:", text)
		_G.nameconfig = text
		writefile(path.._G.nameconfig ..".txt","")
	end
})

local Select_Macro = Tabs.Macro:AddDropdown(" Select_Macro", {
	Title = "Select Macro",
	Values = ye,
	Multi = false,
	Default = selectconfig,
	Callback = function(tab)
		selectconfig = tab
		print(selectconfig)
	end
})

local function Refresh()
	ye = {}
	for i,v in pairs(listfiles("Ladies_Hub/AWTD/Macro/")) do 
		table.insert(ye,({v:gsub("Ladies_Hub/AWTD/Macro/","")})[1])
		Select_Macro:SetValues(ye)
	end
end

Tabs.Macro:AddButton({
	Title = "Refresh Macro",
	Description = "",
	Callback = Refresh
})

Refresh()

local basetime = 0
spawn(function()
	while true do 
		local realtime_wait = wait()
		if game:GetService("Players").LocalPlayer.PlayerGui.InterFace.Skip.Visible == false or workspace.Enemy:FindFirstChildOfClass("Part") then 
			basetime = basetime + realtime_wait
		end
	end
end)


-- ตัวแปรเก็บสถานะว่า Toggle เคยเปิดหรือไม่
local wasRecordStarted = false

local toggleRecord = Tabs.Macro:AddToggle("toggleRecord", {Title = "Start Record", Default = startrecord })
toggleRecord:OnChanged(function(boom)
	startrecord = boom

	-- ถ้า Toggle ถูกเปิด
	if startrecord then
		RecordMacroTable = {}  
		wasRecordStarted = true  -- เก็บสถานะว่าถูกเปิดแล้ว
		print("Recording started")
	else
		-- เช็คว่า Toggle เคยถูกเปิดมาก่อนแล้วเพิ่งถูกปิด
		if wasRecordStarted then
			writefile(path..selectconfig, convert(RecordMacroTable))
			print("Recording stopped")
			wasRecordStarted = false  -- รีเซ็ตสถานะว่า Toggle เคยถูกเปิด
		end
	end
end)


local remote_record = {
	"SpawnUnit",
	"UpgradeUnit",
	"SellUnit"
}
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt,false)
mt.__namecall = function(self,...)
	if not checkcaller() then 
		local args = {...}
		if startrecord then
			if self.Name == "SpawnUnit" then

				table.insert(RecordMacroTable,{
					['time'] = basetime,
					['type'] = self.Name,
					['data'] = {
						['name'] = args[1],
						['position'] = args[2]
					}
				})

			elseif table.find(remote_record,self.Name) then 
				table.insert(RecordMacroTable,{
					['time'] = basetime,
					['type'] = self.Name,
					['data'] = {
						['position'] = args[1].HumanoidRootPart.CFrame
					}
				})
			end
		end
	end
	return old(self,...)
end







local Options = Fluent.Options

do
	local check = false

	if _G.playmacro then
		abc = "Playing"
		check = true
	else
		if check then
			abc = "Stoped"
			check = false
		end
	end


	Tabs.Macro:AddParagraph({

		Title = "Status",

		Content = abc
	})
end




local togglePlay = Tabs.Macro:AddToggle("togglePlay", {Title = "Start Play", Default = _G.playmacro })
togglePlay:OnChanged(function(boo)
	_G.playmacro = boo
	if _G.playmacro then 
		local datamacro = readfile(path..selectconfig)

		local real = loadstring("return "..datamacro)()

		for i,v in pairs(real) do 
			if _G.playmacro then 
				repeat wait() until basetime >= v.time

				if v["type"] == "SpawnUnit" then 
					local args = {
						[1] = v.data.name,
						[2] = v.data.position
					}

					game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("SpawnUnit"):InvokeServer(unpack(args))
				elseif v["type"] == "UpgradeUnit" then

					local current = 10
					local current_instance = nil

					for i,unit in pairs(workspace:WaitForChild("Units"):GetChildren()) do 
						local dis = (v.data.position.Position-unit.HumanoidRootPart.Position).Magnitude
						if dis < current then
							current = dis
							current_instance = unit
						end
					end
					if current_instance then
						print("UPDISAD")
						local args = {
							[1] = current_instance
						}

						game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("UpgradeUnit"):InvokeServer(unpack(args))
					end

				elseif v["type"] == "SellUnit" then
					local current = 10
					local current_instance = nil

					for i,unit in pairs(workspace:WaitForChild("Units"):GetChildren()) do 
						local dis = (v.data.position.Position-unit.HumanoidRootPart.Position).Magnitude
						if dis < current then
							current = dis
							current_instance = unit
						end
					end
					if current_instance then
						local args = {
							[1] = current_instance
						}

						game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("SellUnit"):InvokeServer(unpack(args))
					end
				end
			end
		end
	end
end)


InterfaceManager:SetLibrary(Fluent)
InterfaceManager:SetFolder("Savage_Hub")
InterfaceManager:BuildInterfaceSection(Tabs.Settings)
