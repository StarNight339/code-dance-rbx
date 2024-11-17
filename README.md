# code-dance-rbx

สร้าง localScript ไว้ใน StarterPlayerScripts นะคับผม 
StarterPlayerScripts อยู่ในหมวดของ StarterPlayer น้าาา

--ตังค่าตัวแปร
local UserInputService = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

--ตั่งค่าเมชั่น
local animation = Instance.new("Animation")
animation.AnimationId = "rbxassetid://78099404317429"
local animationTrack = humanoid:LoadAnimation(animation)

local isHolding = false

--ปิดการขยับ
local function disableMovement()
	humanoid.WalkSpeed = 0
	humanoid.JumpPower = 0
	humanoid.JumpHeight = 0 
end

--ปรับให้เหมือนเดิม
local function enableMovement()
	humanoid.WalkSpeed = 16
	humanoid.JumpPower = 50 
	humanoid.JumpHeight = 7.2
end

--เล่นอนิเมชั่น
local function playAnimation()
	if not animationTrack.IsPlaying then
		animationTrack.Priority = Enum.AnimationPriority.Action4 
		disableMovement() 
		animationTrack:Play()
	end
end

--หยุดเล่นอนิเมชั่น
local function stopAnimation()
	if animationTrack.IsPlaying then
		animationTrack:Stop()
		enableMovement() 
	end
end

--รับค่าคียบอร์ดจากปุ่มตอนกด//ไม่ได้ทำของมือถือ
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end 
	if input.KeyCode == Enum.KeyCode.E then
		isHolding = true
		playAnimation()
	end
end)

--รับค่าคียบอร์ดจากปุ่มตอนปล่อย//ไม่ปล่อยมือแน่นอน อุ้ยๆ
UserInputService.InputEnded:Connect(function(input, gameProcessed)
	if input.KeyCode == Enum.KeyCode.E then
		isHolding = false
		stopAnimation()
	end
end)
