local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local UIS=game:GetService("UserInputService")
local ReplicatedStorage=game:GetService("ReplicatedStorage")

local LP=Players.LocalPlayer
local Camera=workspace.CurrentCamera

local C={
	AimLock=false,
	AimSilent=false,
	AntiWall=true,
	AntiDead=true,
	FOV=120,
	FOVColor=Color3.fromRGB(170,80,255),
	StretchScreen=false,
	ESP=false,
	Box=false,
	Line=false,
	Name=false,
	Distance=false,
	Health=false,
	ESPColor=Color3.fromRGB(255,40,70),
	Speed=false,
	SpeedValue=32,
	Spinbot=false,
	SpinSpeed=50,
	InfiniteJump=false,
	StreamMode=false,
	PanelColor=Color3.fromRGB(20,20,28)
}

local Purple=Color3.fromRGB(125,0,255)
local Purple2=Color3.fromRGB(170,35,255)
local Dark=Color3.fromRGB(8,8,13)
local Dark2=Color3.fromRGB(22,22,31)
local White=Color3.fromRGB(245,245,250)

local Gui=Instance.new("ScreenGui")
Gui.Name="TeffModz"
Gui.ResetOnSpawn=false
Gui.IgnoreGuiInset=true
Gui.ZIndexBehavior=Enum.ZIndexBehavior.Sibling
Gui.Parent=LP:WaitForChild("PlayerGui")

local Main=Instance.new("Frame")
Main.Size=UDim2.fromOffset(430,300)
Main.Position=UDim2.new(.5,-215,.5,-150)
Main.BackgroundColor3=Dark
Main.BorderSizePixel=0
Main.Parent=Gui
Instance.new("UICorner",Main).CornerRadius=UDim.new(0,10)

local MainStroke=Instance.new("UIStroke")
MainStroke.Color=Purple
MainStroke.Thickness=2
MainStroke.Parent=Main

local Top=Instance.new("Frame")
Top.Size=UDim2.new(1,0,0,48)
Top.BackgroundColor3=Purple
Top.BorderSizePixel=0
Top.Parent=Main
Instance.new("UICorner",Top).CornerRadius=UDim.new(0,10)

local TopFix=Instance.new("Frame")
TopFix.Size=UDim2.new(1,0,0,12)
TopFix.Position=UDim2.new(0,0,1,-12)
TopFix.BackgroundColor3=Purple
TopFix.BorderSizePixel=0
TopFix.Parent=Top

local Title=Instance.new("TextLabel")
Title.Size=UDim2.new(1,-75,1,0)
Title.Position=UDim2.fromOffset(12,0)
Title.BackgroundTransparency=1
Title.Text="TEFF MODZ V1"
Title.TextColor3=White
Title.Font=Enum.Font.GothamBlack
Title.TextSize=18
Title.TextXAlignment=Enum.TextXAlignment.Left
Title.Parent=Top

local Close=Instance.new("TextButton")
Close.Size=UDim2.fromOffset(35,35)
Close.Position=UDim2.new(1,-42,0,6)
Close.BackgroundTransparency=1
Close.Text="×"
Close.TextColor3=White
Close.Font=Enum.Font.Gotham
Close.TextSize=28
Close.BorderSizePixel=0
Close.Parent=Top

local Tabs=Instance.new("Frame")
Tabs.Size=UDim2.fromOffset(68,230)
Tabs.Position=UDim2.fromOffset(8,60)
Tabs.BackgroundTransparency=1
Tabs.BorderSizePixel=0
Tabs.Parent=Main

local TabLayout=Instance.new("UIListLayout")
TabLayout.Padding=UDim.new(0,5)
TabLayout.HorizontalAlignment=Enum.HorizontalAlignment.Center
TabLayout.VerticalAlignment=Enum.VerticalAlignment.Top
TabLayout.Parent=Tabs

local Content=Instance.new("ScrollingFrame")
Content.Size=UDim2.new(1,-92,1,-70)
Content.Position=UDim2.fromOffset(85,62)
Content.BackgroundTransparency=1
Content.BorderSizePixel=0
Content.ScrollBarThickness=2
Content.AutomaticCanvasSize=Enum.AutomaticSize.Y
Content.CanvasSize=UDim2.fromOffset(0,0)
Content.Parent=Main

local ContentLayout=Instance.new("UIListLayout")
ContentLayout.Padding=UDim.new(0,5)
ContentLayout.Parent=Content

local function Clear()
	for _,v in ipairs(Content:GetChildren()) do
		if not v:IsA("UIListLayout") then
			v:Destroy()
		end
	end
end

local function Section(text)
	local l=Instance.new("TextLabel")
	l.Size=UDim2.new(1,-5,0,24)
	l.BackgroundTransparency=1
	l.Text=text
	l.TextColor3=White
	l.Font=Enum.Font.GothamBold
	l.TextSize=13
	l.TextXAlignment=Enum.TextXAlignment.Left
	l.Parent=Content
end

local function Check(text,key,callback)
	local b=Instance.new("TextButton")
	b.Size=UDim2.new(1,-5,0,34)
	b.BackgroundColor3=Dark2
	b.BorderSizePixel=0
	b.Text=""
	b.Parent=Content

	Instance.new("UICorner",b).CornerRadius=UDim.new(0,6)

	local label=Instance.new("TextLabel")
	label.Size=UDim2.new(1,-52,1,0)
	label.Position=UDim2.fromOffset(10,0)
	label.BackgroundTransparency=1
	label.Text=text
	label.TextColor3=White
	label.Font=Enum.Font.Gotham
	label.TextSize=11
	label.TextXAlignment=Enum.TextXAlignment.Left
	label.Parent=b

	local box=Instance.new("Frame")
	box.Size=UDim2.fromOffset(28,28)
	box.Position=UDim2.new(1,-36,.5,-14)
	box.BackgroundColor3=C[key] and Purple or Color3.fromRGB(34,34,45)
	box.BorderSizePixel=0
	box.Parent=b

	Instance.new("UICorner",box).CornerRadius=UDim.new(0,5)

	local mark=Instance.new("TextLabel")
	mark.Size=UDim2.fromScale(1,1)
	mark.BackgroundTransparency=1
	mark.Text=C[key] and "✓" or ""
	mark.TextColor3=White
	mark.Font=Enum.Font.GothamBold
	mark.TextSize=16
	mark.Parent=box

	b.MouseButton1Click:Connect(function()
		C[key]=not C[key]
		box.BackgroundColor3=C[key] and Purple or Color3.fromRGB(34,34,45)
		mark.Text=C[key] and "✓" or ""

		if callback then
			callback(C[key])
		end
	end)
end

local function Slider(text,key,min,max)
	local f=Instance.new("Frame")
	f.Size=UDim2.new(1,-5,0,43)
	f.BackgroundColor3=Dark2
	f.BorderSizePixel=0
	f.Parent=Content

	Instance.new("UICorner",f).CornerRadius=UDim.new(0,6)

	local label=Instance.new("TextLabel")
	label.Size=UDim2.new(1,-20,0,18)
	label.Position=UDim2.fromOffset(10,2)
	label.BackgroundTransparency=1
	label.TextColor3=White
	label.Font=Enum.Font.Gotham
	label.TextSize=11
	label.TextXAlignment=Enum.TextXAlignment.Left
	label.Parent=f

	local bar=Instance.new("Frame")
	bar.Size=UDim2.new(1,-20,0,5)
	bar.Position=UDim2.fromOffset(10,29)
	bar.BackgroundColor3=Color3.fromRGB(45,45,55)
	bar.BorderSizePixel=0
	bar.Parent=f

	Instance.new("UICorner",bar).CornerRadius=UDim.new(1,0)

	local fill=Instance.new("Frame")
	fill.BackgroundColor3=Purple
	fill.BorderSizePixel=0
	fill.Parent=bar

	Instance.new("UICorner",fill).CornerRadius=UDim.new(1,0)

	local function Set(x)
		local p=math.clamp(
			(x-bar.AbsolutePosition.X)/bar.AbsoluteSize.X,
			0,1
		)

		C[key]=math.floor(min+(max-min)*p)
		fill.Size=UDim2.fromScale(p,1)
		label.Text=text.."  "..C[key]

		if key=="FOV" then
			UpdateFOV()
		end
	end

	local p=math.clamp((C[key]-min)/(max-min),0,1)
	fill.Size=UDim2.fromScale(p,1)
	label.Text=text.."  "..C[key]

	local dragging=false

	bar.InputBegan:Connect(function(input)
		if input.UserInputType==Enum.UserInputType.MouseButton1
		or input.UserInputType==Enum.UserInputType.Touch then
			dragging=true
			Set(input.Position.X)
		end
	end)

	UIS.InputChanged:Connect(function(input)
		if dragging and (
			input.UserInputType==Enum.UserInputType.MouseMovement
			or input.UserInputType==Enum.UserInputType.Touch
		) then
			Set(input.Position.X)
		end
	end)

	UIS.InputEnded:Connect(function(input)
		if input.UserInputType==Enum.UserInputType.MouseButton1
		or input.UserInputType==Enum.UserInputType.Touch then
			dragging=false
		end
	end)
end

local function ColorButton(text,key)
	local b=Instance.new("TextButton")
	b.Size=UDim2.new(1,-5,0,34)
	b.BackgroundColor3=C[key]
	b.BorderSizePixel=0
	b.Text=text
	b.TextColor3=White
	b.Font=Enum.Font.GothamBold
	b.TextSize=11
	b.Parent=Content

	Instance.new("UICorner",b).CornerRadius=UDim.new(0,6)

	b.MouseButton1Click:Connect(function()
		C[key]=Color3.fromHSV(math.random(),.8,1)
		b.BackgroundColor3=C[key]

		if key=="FOVColor" then
			UpdateFOV()
		end
	end)
end

local FOV=Instance.new("Frame")
FOV.AnchorPoint=Vector2.new(.5,.5)
FOV.BackgroundTransparency=1
FOV.BorderSizePixel=0
FOV.Visible=false
FOV.Parent=Gui

local FOVStroke=Instance.new("UIStroke")
FOVStroke.Thickness=1
FOVStroke.Parent=FOV

local FOVCorner=Instance.new("UICorner")
FOVCorner.CornerRadius=UDim.new(1,0)
FOVCorner.Parent=FOV

function UpdateFOV()
	FOV.Visible=C.AimLock or C.AimSilent
	local size=math.max(C.FOV*2,2)
	FOV.Size=UDim2.fromOffset(size,size)
	FOV.Position=UDim2.new(.5,0,.5,0)
	FOVStroke.Color=C.FOVColor
end

local function ApplyStretch()
	if C.StretchScreen then
		Camera.FieldOfView=100
	else
		Camera.FieldOfView=70
	end
end

local function VisibleTarget(character,part)
	if not C.AntiWall then
		return true
	end

	local origin=Camera.CFrame.Position
	local direction=part.Position-origin

	local params=RaycastParams.new()
	params.FilterType=Enum.RaycastFilterType.Exclude
	params.FilterDescendantsInstances={LP.Character}

	local result=workspace:Raycast(origin,direction,params)

	return result and result.Instance:IsDescendantOf(character)
end

local function GetTarget()
	local best=nil
	local bestDistance=math.huge

	local center=Vector2.new(
		Camera.ViewportSize.X/2,
		Camera.ViewportSize.Y/2
	)

	for _,player in ipairs(Players:GetPlayers()) do
		if player~=LP then
			local character=player.Character
			local humanoid=character and character:FindFirstChildOfClass("Humanoid")
			local head=character and character:FindFirstChild("Head")

			if character and humanoid and head then
				if C.AntiDead and humanoid.Health<=0 then
					continue
				end

				local screen,visible=Camera:WorldToViewportPoint(head.Position)

				if visible and screen.Z>0 then
					local distance=(
						Vector2.new(screen.X,screen.Y)-center
					).Magnitude

					if distance<=C.FOV and distance<bestDistance then
						if VisibleTarget(character,head) then
							best=player
							bestDistance=distance
						end
					end
				end
			end
		end
	end

	return best
end

local function AimLock()
	if not C.AimLock then
		return
	end

	local target=GetTarget()

	if target then
		local character=target.Character
		local head=character and character:FindFirstChild("Head")

		if head then
			Camera.CFrame=CFrame.lookAt(
				Camera.CFrame.Position,
				head.Position
			)
		end
	end
end

local AimRemote=ReplicatedStorage:FindFirstChild("AimSilent")

local function AimSilent()
	if not C.AimSilent or not AimRemote then
		return
	end

	local target=GetTarget()

	if target then
		local character=target.Character
		local head=character and character:FindFirstChild("Head")

		if head then
			AimRemote:FireServer(target,head.Position,C.FOV)
		end
	end
end

local ESP={}

local function AddESP(player)
	if player==LP or ESP[player] then
		return
	end

	local h=Instance.new("Highlight")
	h.FillTransparency=1
	h.OutlineColor=C.ESPColor
	h.OutlineTransparency=0.2
	h.Enabled=false
	h.Parent=Gui

	local bb=Instance.new("BillboardGui")
	bb.Size=UDim2.fromOffset(1,1)
	bb.AlwaysOnTop=true
	bb.Enabled=false
	bb.Parent=Gui

	local tx=Instance.new("TextLabel")
	tx.Size=UDim2.fromScale(1,1)
	tx.BackgroundTransparency=1
	tx.Text=""
	tx.Visible=false
	tx.Parent=bb

	local ln=Instance.new("Frame")
	ln.AnchorPoint=Vector2.new(.5,.5)
	ln.BackgroundColor3=C.ESPColor
	ln.BorderSizePixel=0
	ln.Visible=false
	ln.Parent=Gui

	local hpbg=Instance.new("Frame")
	hpbg.AnchorPoint=Vector2.new(1,.5)
	hpbg.Size=UDim2.fromOffset(3,40)
	hpbg.BackgroundColor3=Color3.fromRGB(35,35,35)
	hpbg.BorderSizePixel=0
	hpbg.Visible=false
	hpbg.Parent=Gui

	local hpfill=Instance.new("Frame")
	hpfill.AnchorPoint=Vector2.new(0,1)
	hpfill.Position=UDim2.new(0,0,1,0)
	hpfill.Size=UDim2.fromScale(1,1)
	hpfill.BackgroundColor3=Color3.fromRGB(0,255,80)
	hpfill.BorderSizePixel=0
	hpfill.Parent=hpbg

	ESP[player]={
		H=h,
		B=bb,
		T=tx,
		L=ln,
		HB=hpbg,
		HF=hpfill
	}
end

local function RemoveESP(player)
	if ESP[player] then
		for _,v in pairs(ESP[player]) do
			v:Destroy()
		end
		ESP[player]=nil
	end
end

for _,p in ipairs(Players:GetPlayers()) do
	AddESP(p)
end

Players.PlayerAdded:Connect(AddESP)
Players.PlayerRemoving:Connect(RemoveESP)

local function UpdateESP()
	Camera=workspace.CurrentCamera

	for player,d in pairs(ESP) do
		local char=player.Character
		local root=char and char:FindFirstChild("HumanoidRootPart")
		local hum=char and char:FindFirstChildOfClass("Humanoid")

		d.H.OutlineColor=C.ESPColor
		d.L.BackgroundColor3=C.ESPColor

		if not C.ESP
		or not char
		or not root
		or not hum
		or hum.Health<=0 then
			d.H.Enabled=false
			d.B.Enabled=false
			d.L.Visible=false
			d.HB.Visible=false
			continue
		end

		d.H.Adornee=char
		d.H.Enabled=C.Box

		local screen,visible=Camera:WorldToViewportPoint(root.Position)

		if visible and screen.Z>0 then
			local hp=math.clamp(
				hum.Health/math.max(hum.MaxHealth,1),
				0,1
			)

			d.HF.Size=UDim2.fromScale(1,hp)

			if hp>.5 then
				d.HF.BackgroundColor3=Color3.fromRGB(0,255,80)
			elseif hp>.25 then
				d.HF.BackgroundColor3=Color3.fromRGB(255,200,0)
			else
				d.HF.BackgroundColor3=Color3.fromRGB(255,40,40)
			end

			d.HB.Position=UDim2.fromOffset(
				screen.X-24,
				screen.Y
			)

			d.HB.Size=UDim2.fromOffset(
				3,
				math.clamp(70/(screen.Z/10),25,70)
			)

			d.HB.Visible=C.Health

			if C.Line then
				local start=Vector2.new(
					Camera.ViewportSize.X/2,0
				)

				local finish=Vector2.new(
					screen.X,
					screen.Y
				)

				local difference=finish-start

				d.L.Position=UDim2.fromOffset(
					start.X+difference.X/2,
					start.Y+difference.Y/2
				)

				d.L.Size=UDim2.fromOffset(
					difference.Magnitude,1
				)

				d.L.Rotation=math.deg(
					math.atan2(difference.Y,difference.X)
				)

				d.L.Visible=true
			else
				d.L.Visible=false
			end
		else
			d.L.Visible=false
			d.HB.Visible=false
		end
	end
end

local function Movement()
	local char=LP.Character
	local hum=char and char:FindFirstChildOfClass("Humanoid")

	if not hum then
		return
	end

	hum.WalkSpeed=C.Speed and C.SpeedValue or 16

	if C.Spinbot then
		local root=char:FindFirstChild("HumanoidRootPart")

		if root then
			root.CFrame=root.CFrame*CFrame.Angles(0,math.rad(C.SpinSpeed),0)
		end
	end
end

UIS.JumpRequest:Connect(function()
	if C.InfiniteJump then
		local char=LP.Character
		local hum=char and char:FindFirstChildOfClass("Humanoid")

		if hum then
			hum:ChangeState(Enum.HumanoidStateType.Jumping)
		end
	end
end)

local function CombatTab()
	Clear()

	Section("COMBAT")
	Check("Ativar Aim Lock","AimLock",UpdateFOV)
	Check("Aim Silent","AimSilent",UpdateFOV)
	Check("Anti Wall","AntiWall")
	Check("Anti Dead","AntiDead")
	Check("Tela Esticada","StretchScreen",ApplyStretch)
	Slider("FOV","FOV",0,360)
	ColorButton("Mudar cor do FOV","FOVColor")
end

local function MovementTab()
	Clear()

	Section("MOVEMENT")
	Check("Speed","Speed")
	Slider("Velocidade","SpeedValue",32,100)
	Check("Spinbot","Spinbot")
	Slider("Spin Speed","SpinSpeed",0,100)
	Check("Infinite Jump","InfiniteJump")
end

local function VisualTab()
	Clear()

	Section("ESP")
	Check("Ativar ESP","ESP")
	Check("Box","Box")
	Check("Line","Line")
	Check("Name","Name")
	Check("Distance","Distance")
	Check("Health","Health")
	ColorButton("Mudar cor do ESP","ESPColor")
end

local Open

local function UpdatePanelColor()
	Main.BackgroundColor3=C.PanelColor

	if Open then
		Open.BackgroundColor3=C.PanelColor
	end
end

local function ConfigTab()
	Clear()

	Section("CONFIG")
	ColorButton("Mudar cor do painel","PanelColor")

	Check("Stream Mode","StreamMode",function(v)
		Main.Visible=not v
	end)
end

local function Tab(callback,icon)
	local b=Instance.new("TextButton")
	b.Size=UDim2.fromOffset(64,48)
	b.BackgroundColor3=Color3.fromRGB(90,0,190)
	b.BorderSizePixel=0
	b.Text=icon
	b.TextColor3=White
	b.Font=Enum.Font.GothamBold
	b.TextSize=21
	b.Parent=Tabs

	Instance.new("UICorner",b).CornerRadius=UDim.new(0,8)

	local stroke=Instance.new("UIStroke")
	stroke.Color=Purple2
	stroke.Thickness=1
	stroke.Parent=b

	b.MouseButton1Click:Connect(function()
		callback()

		for _,v in ipairs(Tabs:GetChildren()) do
			if v:IsA("TextButton") then
				v.BackgroundColor3=Color3.fromRGB(90,0,190)
			end
		end

		b.BackgroundColor3=Purple
	end)
end

Tab(CombatTab,"⚙")
Tab(MovementTab,"◉")
Tab(VisualTab,"◈")
Tab(ConfigTab,"⚙")

CombatTab()

Open=Instance.new("TextButton")
Open.Size=UDim2.fromOffset(58,58)
Open.Position=UDim2.fromOffset(18,160)
Open.BackgroundColor3=C.PanelColor
Open.Text="TEFF\nV1"
Open.TextColor3=Color3.fromRGB(225,205,255)
Open.Font=Enum.Font.GothamBlack
Open.TextSize=12
Open.BorderSizePixel=0
Open.Visible=false
Open.Parent=Gui

Instance.new("UICorner",Open).CornerRadius=UDim.new(1,0)

local OpenStroke=Instance.new("UIStroke")
OpenStroke.Color=Purple2
OpenStroke.Thickness=1.5
OpenStroke.Parent=Open

Close.MouseButton1Click:Connect(function()
	Main.Visible=false
	Open.Visible=true
end)

Open.MouseButton1Click:Connect(function()
	Main.Visible=true
	Open.Visible=false
	C.StreamMode=false
end)

local dragging=false
local dragStart
local startPos

Top.InputBegan:Connect(function(input)
	if input.UserInputType==Enum.UserInputType.MouseButton1
	or input.UserInputType==Enum.UserInputType.Touch then
		dragging=true
		dragStart=input.Position
		startPos=Main.Position
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging and (
		input.UserInputType==Enum.UserInputType.MouseMovement
		or input.UserInputType==Enum.UserInputType.Touch
	) then
		local delta=input.Position-dragStart

		Main.Position=UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset+delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset+delta.Y
		)
	end
end)

UIS.InputEnded:Connect(function(input)
	if input.UserInputType==Enum.UserInputType.MouseButton1
	or input.UserInputType==Enum.UserInputType.Touch then
		dragging=false
	end
end)

local openDragging=false
local openStart
local openPos

Open.InputBegan:Connect(function(input)
	if input.UserInputType==Enum.UserInputType.MouseButton1
	or input.UserInputType==Enum.UserInputType.Touch then
		openDragging=true
		openStart=input.Position
		openPos=Open.Position
	end
end)

UIS.InputChanged:Connect(function(input)
	if openDragging and (
		input.UserInputType==Enum.UserInputType.MouseMovement
		or input.UserInputType==Enum.UserInputType.Touch
	) then
		local delta=input.Position-openStart

		Open.Position=UDim2.new(
			openPos.X.Scale,
			openPos.X.Offset+delta.X,
			openPos.Y.Scale,
			openPos.Y.Offset+delta.Y
		)
	end
end)

UIS.InputEnded:Connect(function(input)
	if input.UserInputType==Enum.UserInputType.MouseButton1
	or input.UserInputType==Enum.UserInputType.Touch then
		openDragging=false
	end
end)

local lastSilent=0

RunService.RenderStepped:Connect(function()
	Camera=workspace.CurrentCamera

	UpdatePanelColor()
	UpdateESP()
	UpdateFOV()
	Movement()
	AimLock()
	ApplyStretch()

	if C.AimSilent and os.clock()-lastSilent>=0.1 then
		lastSilent=os.clock()
		AimSilent()
	end
end)
