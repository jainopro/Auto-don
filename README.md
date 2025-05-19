-- Dịch vụ
local HttpService = game:GetService("HttpService")
local player = game:GetService("Players").LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Đường dẫn file theo tên người dùng
local fileName = "don_data_" .. player.Name .. ".json"

-- ======== XỬ LÝ FILE JSON =========

-- Tải dữ liệu từ file JSON
local function loadFromFile()
    if isfile and isfile(fileName) then
        local content = readfile(fileName)
        local success, data = pcall(function()
            return HttpService:JSONDecode(content)
        end)
        if success and data and data.don then
            return data.don
        end
    end
    return ""
end

-- Lưu dữ liệu vào file JSON
local function saveToFile(text)
    if writefile then
        local data = {
            don = text
        }
        writefile(fileName, HttpService:JSONEncode(data))
    end
end

-- ======== TẠO GUI =========

-- Ẩn một phần tên
local function hideName(name)
    local visibleLength = math.max(3, math.floor(#name * 0.5))
    local hiddenPart = string.rep("*", #name - visibleLength)
    return string.sub(name, 1, visibleLength) .. hiddenPart
end

-- Giao diện chính
local nameHub = Instance.new("ScreenGui")
nameHub.Name = "NameHub"
nameHub.Parent = playerGui

local mainFrame = Instance.new("Frame")
mainFrame.Parent = nameHub
mainFrame.Size = UDim2.new(0.3, 0, 0.1, 0)
mainFrame.Position = UDim2.new(0.5, 0, 0.1, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BackgroundTransparency = 0.2
mainFrame.BorderSizePixel = 0
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.Active = true
mainFrame.Draggable = true

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0.15, 0)
uiCorner.Parent = mainFrame

-- Label hiển thị tên (ẩn bớt)
local nameLabel = Instance.new("TextLabel")
nameLabel.Parent = mainFrame
nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
nameLabel.Position = UDim2.new(0, 0, 0, 0)
nameLabel.BackgroundTransparency = 1
nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
nameLabel.TextScaled = true
nameLabel.Font = Enum.Font.GothamBold
nameLabel.Text = "👤 Tên :" .. hideName(player.Name)

-- Khung chứa "Đơn"
local jobFrame = Instance.new("Frame")
jobFrame.Parent = mainFrame
jobFrame.Size = UDim2.new(1, 0, 0.5, 0)
jobFrame.Position = UDim2.new(0, 0, 0.5, 0)
jobFrame.BackgroundTransparency = 1

-- Label "Đơn:"
local jobTitle = Instance.new("TextLabel")
jobTitle.Parent = jobFrame
jobTitle.Size = UDim2.new(0.3, 0, 1, 0)
jobTitle.Position = UDim2.new(0, 0, 0, 0)
jobTitle.BackgroundTransparency = 1
jobTitle.TextColor3 = Color3.fromRGB(255, 223, 88)
jobTitle.TextScaled = true
jobTitle.Font = Enum.Font.GothamBold
jobTitle.Text = "📌 Đơn :"

-- Ô nhập nội dung đơn
local jobBox = Instance.new("TextBox")
jobBox.Parent = jobFrame
jobBox.Size = UDim2.new(0.7, 0, 1, 0)
jobBox.Position = UDim2.new(0.3, 0, 0, 0)
jobBox.BackgroundTransparency = 1
jobBox.TextColor3 = Color3.fromRGB(255, 255, 255)
jobBox.TextScaled = true
jobBox.Font = Enum.Font.GothamBold
jobBox.ClearTextOnFocus = false
jobBox.TextWrapped = true

-- Load dữ liệu từ file vào TextBox
jobBox.Text = loadFromFile()

-- Auto save mỗi khi text thay đổi
jobBox:GetPropertyChangedSignal("Text"):Connect(function()
    saveToFile(jobBox.Text)
end)
print("Thanh cong")
