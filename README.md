local button
local player = { x = 50, y = 300, width = 30, height = 30 }
local bullet = { x = player.x, y = player.y, speed = 500, active = false }

function love.load()
    button = {
        x = 400,
        y = 200,
        width = 150,
        height = 50,
        isMoving = false,
        active = false -- Button starts as inactive
    }
end

function love.update(dt)
    if button.isMoving then
        local mx, my = love.mouse.getPosition()
        button.x = mx - button.width / 2
        button.y = my - button.height / 2
    end

    if bullet.active then
        bullet.x = bullet.x + bullet.speed * dt
        if bullet.x > love.graphics.getWidth() then
            bullet.active = false -- Reset bullet if it goes off-screen
        end
    end

    -- Check for collision with the button
    if bullet.active and 
       bullet.x >= button.x and bullet.x <= button.x + button.width and
       bullet.y >= button.y and bullet.y <= button.y + button.height then
        bullet.active = false -- Deactivate the bullet on hit
        button.active = not button.active -- Toggle button state
        print("Button toggled to " .. (button.active and "ON" or "OFF"))
    end
end

function love.draw()
    -- Draw the player (as a square)
    love.graphics.setColor(0, 1, 0)
    love.graphics.rectangle("fill", player.x, player.y, player.width, player.height)

    -- Draw the button
    love.graphics.setColor(button.active and {0, 1, 0} or {0, 0.5, 1}) -- Change color based on state
    love.graphics.rectangle("fill", button.x, button.y, button.width, button.height)
    love.graphics.setColor(1, 1, 1)
    love.graphics.printf("Click Me", button.x, button.y + button.height / 4, button.width, "center")

    -- Draw the bullet
    if bullet.active then
        love.graphics.setColor(1, 0, 0)
        love.graphics.rectangle("fill", bullet.x, bullet.y, 5, 5)
    end
end

function love.mousepressed(x, y, buttonNum)
    if buttonNum == 1 then
        if x >= button.x and x <= button.x + button.width and
           y >= button.y and y <= button.y + button.height then
            button.isMoving = true
        end
    elseif buttonNum == 2 then
        -- Shoot a bullet when right mouse button is clicked
        bullet.x = player.x + player.width / 2
        bullet.y = player.y + player.height / 2
        bullet.active = true
    end
end

function love.mousereleased(x, y, buttonNum)
    if buttonNum == 1 then
        button.isMoving = false
    end
end
