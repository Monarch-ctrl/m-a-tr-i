-- ShadowMonarchHub - ShadowMain.lua

-- Khởi tạo GUI và các biến cần thiết
local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "ShadowMonarchGUI"

-- Nút bật/tắt mưa trái ác quỷ
local fruitRainButton = Instance.new("TextButton")
fruitRainButton.Text = "🌧️ Bật Mưa Trái Ác Quỷ"
fruitRainButton.Position = UDim2.new(0, 100, 0, 100)
fruitRainButton.Size = UDim2.new(0, 200, 0, 50)
fruitRainButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
fruitRainButton.TextColor3 = Color3.fromRGB(255, 255, 255)
fruitRainButton.Parent = gui

-- Danh sách tất cả các trái ác quỷ
local AllFruits = {
    "Leopard", "Dragon", "Venom", "Dough", "Shadow", "Sound", "Dark", 
    "Blizzard", "Kitsune", "Yeti", "Gas", "Phoenix", "Rumble", "Control", 
    "Light", "Magma", "Flame", "Quake", "String", "Barrier", "Ice", 
    "Spider", "Buddha", "Sand", "Pain", "Portal", "Gravity", "Ghost", 
    "Diamond", "Love", "Rubber", "Spring", "Smoke", "Falcon", "Spin", 
    "Spike", "Rocket", "Chop", "Bomb"
}

-- Biến trạng thái mưa trái
local isFruitRainActive = false

-- Giỏ lưu trữ trái ác quỷ đã nhặt
local inventory = {}

-- Hàm tạo trái rơi trong Delta X
local function dropFruit()
    local fruitName = AllFruits[math.random(1, #AllFruits)] -- Chọn ngẫu nhiên trái từ danh sách

    local fruit = Instance.new("Tool")
    fruit.Name = fruitName

    -- Tạo phần Handle cho trái, cho phép rơi từ trên trời xuống
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(2, 2, 2)
    handle.Position = Vector3.new(math.random(-100, 100), 100, math.random(-100, 100))
    handle.Anchored = false
    handle.CanCollide = true
    handle.BrickColor = BrickColor.Random()
    handle.Parent = fruit

    fruit.RequiresHandle = true
    fruit.Parent = workspace

    -- Tạo sự kiện nhặt trái ác quỷ
    fruit.Touched:Connect(function(hit)
        local character = hit.Parent
        if character and character:FindFirstChild("Humanoid") and character.Humanoid.Health > 0 then
            -- Nhặt trái
            if not table.find(inventory, fruitName) then
                table.insert(inventory, fruitName)
                print(fruitName .. " đã được thêm vào giỏ!")
                fruit:Destroy()  -- Hủy trái sau khi nhặt
            end
        end
    end)
end

-- Hàm ăn trái ác quỷ từ giỏ lưu trữ và kích hoạt hiệu ứng
local function eatFruit(fruitName)
    if table.find(inventory, fruitName) then
        -- Đảm bảo trái chưa ăn
        print("Đã ăn trái " .. fruitName)

        -- Kích hoạt kỹ năng hoặc buff tương ứng với trái ác quỷ
        if fruitName == "Leopard" then
            -- Kích hoạt kỹ năng trái Leopard
            print("Sử dụng kỹ năng Leopard!")
            -- Thêm mã hiệu ứng cụ thể cho trái Leopard
        elseif fruitName == "Dragon" then
            -- Kích hoạt kỹ năng trái Dragon
            print("Sử dụng kỹ năng Dragon!")
            -- Thêm mã hiệu ứng cụ thể cho trái Dragon
        elseif fruitName == "Venom" then
            -- Kích hoạt kỹ năng trái Venom
            print("Sử dụng kỹ năng Venom!")
            -- Thêm mã hiệu ứng cụ thể cho trái Venom
        elseif fruitName == "Dough" then
            -- Kích hoạt kỹ năng trái Dough
            print("Sử dụng kỹ năng Dough!")
            -- Thêm mã hiệu ứng cụ thể cho trái Dough
        -- Cập nhật cho tất cả các trái ác quỷ khác
        else
            print("Sử dụng kỹ năng " .. fruitName)
        end

        -- Sau khi ăn, xóa trái khỏi giỏ lưu trữ
        for i, name in ipairs(inventory) do
            if name == fruitName then
                table.remove(inventory, i)
                break
            end
        end
    else
        print("Không có trái này trong giỏ!")
    end
end

-- Hàm mưa trái liên tục
task.spawn(function()
    while true do
        if isFruitRainActive then
            dropFruit()
        end
        wait(10) -- thời gian giữa các lần rơi trái
    end
end)

-- Sự kiện khi nhấn nút bật/tắt mưa trái
fruitRainButton.MouseButton1Click:Connect(function()
    isFruitRainActive = not isFruitRainActive
    if isFruitRainActive then
        fruitRainButton.Text = "🛑 Tắt Mưa Trái Ác Quỷ"
    else
        fruitRainButton.Text = "🌧️ Bật Mưa Trái Ác Quỷ"
    end
end)

-- Tạo một nút để ăn trái ác quỷ
local eatFruitButton = Instance.new("TextButton")
eatFruitButton.Text = "🍽️ Ăn Trái Ác Quỷ"
eatFruitButton.Position = UDim2.new(0, 100, 0, 160)
eatFruitButton.Size = UDim2.new(0, 200, 0, 50)
eatFruitButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
eatFruitButton.TextColor3 = Color3.fromRGB(255, 255, 255)
eatFruitButton.Parent = gui

-- Sự kiện khi nhấn nút ăn trái ác quỷ
eatFruitButton.MouseButton1Click:Connect(function()
    if #inventory > 0 then
        local fruitToEat = inventory[1]  -- Chọn trái đầu tiên trong giỏ
        eatFruit(fruitToEat)
    else
        print("Giỏ trống! Không có trái ác quỷ để ăn.")
    end
end)
