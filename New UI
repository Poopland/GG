local tweenService = game:GetService("TweenService")
local userInputService = game:GetService("UserInputService")

local tweenInfo = TweenInfo.new
local player = game:GetService("Players").LocalPlayer
local mouse = player:GetMouse()

local utility = {}
do
    function utility:create(instance, properties, children)
        local object = Instance.new(instance)
        for i, v in next, properties do
            object[i] = v
        end
        if children then
            for i, v in next, children do
                v.Parent = object
            end
        end
        return object
    end
    function utility:removeSpaces(str)
        return str:gsub(" ", "")
    end
    function utility:tween(object, duration, properties)
        tweenService:Create(object, tweenInfo(duration, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), properties):Play()
    end
    function utility:drag(instance)
        local dragging = nil
        local dragInput = nil
        local dragStart = nil
        local startPos = nil

        local function update(input)
            local delta = input.Position - dragStart
            tweenService:Create(instance, TweenInfo.new(0.2, Enum.EasingStyle.Quart, Enum.EasingDirection.Out, 0, false, 0), {Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)}):Play()
        end
        instance.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = true
                dragStart = input.Position
                startPos = instance.Position
                
                input.Changed:Connect(function()
                    if input.UserInputState == Enum.UserInputState.End then
                        dragging = false
                    end
                end)
            end
        end)
        instance.InputChanged:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
                dragInput = input
            end
        end)
        userInputService.InputChanged:Connect(function(input)
            if input == dragInput and dragging then
                update(input)
            end
        end)
    end
end
local library = {}
local page = {}
do
    library.__index = library
    page.__index = page
    function library.new(title, theme)
        theme = theme or "Dark"
        local gui = utility:create("ScreenGui", {
            Name = utility:removeSpaces(title),
            Parent = game.CoreGui
        }, {
            utility:create("Frame", {
                Name = "Container",
                BackgroundColor3 = theme == "Dark" and Color3.fromRGB(25, 25, 25) or Color3.fromRGB(240, 240, 240),
                BorderSizePixel = 0,
                Position = UDim2.new(0.072, 0, 0.2, 0),
                Size = UDim2.new(0, 500, 0, 0),
                ClipsDescendants = true
            }, {
                utility:create("UICorner", {
                    CornerRadius = UDim.new(0, 5)
                }),
                utility:create("TextLabel", {
                    Name = "TitleLabel",
                    BackgroundTransparency = 1,
                    Position = UDim2.new(0, 8, 0, 0),
                    Size = UDim2.new(0, 492, 0, 30),
                    Font = Enum.Font.SourceSans,
                    Text = title,
                    TextColor3 = theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                    TextSize = 18,
                    TextXAlignment = Enum.TextXAlignment.Left
                }),
                utility:create("TextButton", {
                    Name = "ThemeButton",
                    BackgroundTransparency = 1,
                    AnchorPoint = Vector2.new(1, 0),
                    Position = UDim2.new(1, -8, 0, 0),
                    Size = UDim2.new(0, 32, 0, 30),
                    Font = Enum.Font.SourceSans,
                    Text = theme,
                    TextColor3 = theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                    TextSize = 18,
                    TextXAlignment = Enum.TextXAlignment.Right
                }),
                utility:create("Frame", {
                    Name = "TabList",
                    BackgroundColor3 = theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255),
                    BorderSizePixel = 0,
                    Position = UDim2.new(0, 0, 0, 30),
                    Size = UDim2.new(0, 500, 0, 30)
                }, {
                    utility:create("UIListLayout", {
                        FillDirection = Enum.FillDirection.Horizontal,
                        SortOrder = Enum.SortOrder.LayoutOrder
                    })
                }),
                utility:create("Frame", {
                    Name = "PagesContainer",
                    BackgroundTransparency = 1,
                    Position = UDim2.new(0, 8, 0, 68),
                    Size = UDim2.new(0, 484, 0, 324)
                })
            })
        })
        local open = false
        utility:tween(gui.Container, 0.25, {
            Size = UDim2.new(0, 500, 0, 400)
        })
        open = true
        utility:drag(gui.Container)
        userInputService.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.RightShift then
                utility:tween(gui.Container, 0.25, {
                    Size = open and UDim2.new(0, 500, 0, 0) or UDim2.new(0, 500, 0, 400)
                })
                open = not open
            end
        end)
        return setmetatable({
            gui = gui,
            theme = theme,
            themePage = nil,
            pages = {}
        }, library)
    end
    function page.new(library, name)
        local button = utility:create("TextButton", {
            Name = utility:removeSpaces(name),
            BackgroundTransparency = 1,
            Parent = library.gui.Container.TabList,
            Size = UDim2.new(1, 0, 0, 30),
            Font = Enum.Font.GothamSemibold,
            Text = name,
            TextSize = 13,
            TextColor3 = library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
            TextTransparency = 0.3
        })
        button.Size = UDim2.new(0, button.TextBounds.X + 8, 0, 30)
        local container = utility:create("ScrollingFrame", {
            Name = utility:removeSpaces(name) .. "Page",
            Parent = library.gui.Container.PagesContainer,
            BackgroundTransparency = 1,
            Size = UDim2.new(1, 0, 0, 0),
            CanvasSize = UDim2.new(0, 0, 0, 0),
            ScrollBarThickness = 0,
            ScrollingDirection = Enum.ScrollingDirection.Y
        }, {
            utility:create("UIListLayout", {
                SortOrder = Enum.SortOrder.LayoutOrder,
                Padding = UDim.new(0, 8)
            })
        })
        container.UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            container.CanvasSize = UDim2.new(0, 0, 0, container.UIListLayout.AbsoluteContentSize.Y)
        end)
        return setmetatable({
            library = library,
            button = button,
            container = container,
            items = {}
        }, page)
    end
    function library:addPage(...)
        local page2 = page.new(self, ...)
        local button = page2.button
        table.insert(self.pages, page2)
        if #self.pages == 1 then
            self.themePage = page2
        end
        self.gui.Container.ThemeButton.MouseButton1Click:Connect(function()
            if self.themePage == page2 then
                self.theme = self.theme == "Dark" and "Light" or "Dark"
                self.gui.Container.ThemeButton.Text = self.theme
            end
            wait(0.1)
            utility:tween(self.gui.Container, 0.5, {
                BackgroundColor3 = self.theme == "Dark" and Color3.fromRGB(25, 25, 25) or Color3.fromRGB(240, 240, 240)
            })
            utility:tween(self.gui.Container.TitleLabel, 0.5, {
                TextColor3 = self.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
            utility:tween(self.gui.Container.ThemeButton, 0.5, {
                TextColor3 = self.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
            utility:tween(self.gui.Container.TabList, 0.5, {
                BackgroundColor3 = self.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255)
            })
            utility:tween(button, 0.5, {
                TextColor3 = self.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
        end)
        button.MouseButton1Click:Connect(function()
            self:selectPage(page2)
        end)
        return page2
    end
    function page:addToggle(name, default, callback)
        default = default or false
        local frame = utility:create("Frame", {
            Name = utility:removeSpaces(name) .. "Toggle",
            BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255),
            Parent = self.container,
            Size = UDim2.new(1, 0, 0, 30)
        }, {
            utility:create("UICorner", {
                CornerRadius = UDim.new(0, 4)
            }),
            utility:create("TextButton", {
                Name = "ToggleButton",
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 8, 0, 0),
                Size = UDim2.new(1, -8, 1, 0),
                ZIndex = 2,
                Font = Enum.Font.GothamSemibold,
                Text = name,
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextSize = 14,
                TextXAlignment = Enum.TextXAlignment.Left
            }),
            utility:create("Frame", {
                Name = "Status",
                BackgroundColor3 = default and Color3.fromRGB(53, 255, 53) or Color3.fromRGB(255, 53, 53),
                AnchorPoint = Vector2.new(1, 0),
                Position = UDim2.new(1, -6, 0.5, -10),
                Size = UDim2.new(0, 20, 0, 20)
            }, {
                utility:create("UICorner", {
                    CornerRadius = UDim.new(0, 4)
                })
            })
        })
        table.insert(self.items, frame)
        local active = default
        self.library.gui.Container.ThemeButton.MouseButton1Click:Connect(function()
            wait(0.1)
            utility:tween(frame, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255)
            })
            utility:tween(frame.ToggleButton, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
        end)
        frame.ToggleButton.MouseButton1Click:Connect(function()
            active = not active
            self:updateToggle(frame, active)
            if callback then
                callback(active)
            end
        end)
        return frame
    end
    function page:addButton(name, callback)
        local frame = utility:create("Frame", {
            Name = utility:removeSpaces(name) .. "Button",
            BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255),
            Parent = self.container,
            Size = UDim2.new(1, 0, 0, 30)
        }, {
            utility:create("UICorner", {
                CornerRadius = UDim.new(0, 4)
            }),
            utility:create("TextButton", {
                Name = "Button",
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 0, 0, 0),
                Size = UDim2.new(1, 0, 1, 0),
                ZIndex = 2,
                Font = Enum.Font.GothamSemibold,
                Text = name,
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextSize = 14
            })
        })
        table.insert(self.items, frame)
        self.library.gui.Container.ThemeButton.MouseButton1Click:Connect(function()
            wait(0.1)
            utility:tween(frame, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255)
            })
            utility:tween(frame.Button, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
        end)
        frame.Button.MouseButton1Click:Connect(function()
            if callback then
                callback()
            end
        end)
    end
    function page:addSlider(name, default, min, max, callback)
        default = default or 10
        min = min or 10
        max = max or 100
        local frame = utility:create("Frame", {
            Name = utility:removeSpaces(name) .. "Slider",
            BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255),
            Parent = self.container,
            Size = UDim2.new(1, 0, 0, 30)
        }, {
            utility:create("UICorner", {
                CornerRadius = UDim.new(0, 4)
            }),
            utility:create("TextLabel", {
                Name = "SliderLabel",
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 8, 0, 0),
                Size = UDim2.new(1, -8, 1, 0),
                ZIndex = 2,
                Font = Enum.Font.GothamSemibold,
                Text = name,
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextSize = 14,
                TextXAlignment = Enum.TextXAlignment.Left
            }),
            utility:create("Frame", {
                Name = "Bar",
                AnchorPoint = Vector2.new(1, 0),
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(55, 55, 55) or Color3.fromRGB(230, 230, 230),
                Position = UDim2.new(1, -10, 0.5, -3),
                Size = UDim2.new(0, 120, 0, 6)
            }, {
                utility:create("UICorner", {
                    CornerRadius = UDim.new(0, 4)
                }),
                utility:create("Frame", {
                    Name = "Selector",
                    AnchorPoint = Vector2.new(0.5, 0.5),
                    BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(65, 65, 65) or Color3.fromRGB(220, 220, 220),
                    Position = UDim2.new(0.5, 0, 0.5, 0),
                    Size = UDim2.new(0, 12, 0, 12)
                }, {
                    utility:create("UICorner", {
                        CornerRadius = UDim.new(1, 0)
                    })
                })
            }),
            utility:create("TextButton", {
                Name = "DetectFrame",
                BackgroundTransparency = 1,
                AnchorPoint = Vector2.new(1, 0),
                Position = UDim2.new(1, 0, 0, 0),
                Size = UDim2.new(0, 150, 1, 0),
                Text = ""
            }),
            utility:create("TextLabel", {
                Name = "ValueLabel",
                BackgroundTransparency = 1,
                AnchorPoint = Vector2.new(1, 0),
                Position = UDim2.new(1, -138, 0, 0),
                Size = UDim2.new(1, -138, 1, 0),
                Font = Enum.Font.Gotham,
                Text = default,
                TextSize = 14,
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextXAlignment = Enum.TextXAlignment.Right
            })
        })
        table.insert(self.items, frame)
        self.library.gui.Container.ThemeButton.MouseButton1Click:Connect(function()
            wait(0.1)
            utility:tween(frame, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255)
            })
            utility:tween(frame.SliderLabel, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
            utility:tween(frame.Bar, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(55, 55, 55) or Color3.fromRGB(230, 230, 230)
            })
            utility:tween(frame.Bar.Selector, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(65, 65, 65) or Color3.fromRGB(220, 220, 220)
            })
            utility:tween(frame.ValueLabel, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
        end)
        local value = default
        local dragging
        self:updateSlider(frame, value, min, max)
        userInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)
        frame.DetectFrame.MouseButton1Down:Connect(function()
            dragging = true
            while dragging do
                value = self:updateSlider(frame, nil, min, max)
                if callback then
                    callback(value)
                end
                wait()
            end
        end)
        return frame
    end
    function page:addTextBox(name, callback)
        local frame = utility:create("Frame", {
            Name = utility:removeSpaces(name) .. "TextBox",
            BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255),
            Parent = self.container,
            Size = UDim2.new(1, 0, 0, 30)
        }, {
            utility:create("UICorner", {
                CornerRadius = UDim.new(0, 4)
            }),
            utility:create("TextLabel", {
                Name = "TextBoxLabel",
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 8, 0, 0),
                Size = UDim2.new(1, -8, 1, 0),
                ZIndex = 2,
                Font = Enum.Font.GothamSemibold,
                Text = name,
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextSize = 14,
                TextXAlignment = Enum.TextXAlignment.Left
            }),
            utility:create("TextBox", {
                Name = "TextBox",
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(55, 55, 55) or Color3.fromRGB(230, 230, 230),
                BorderSizePixel = 0,
                AnchorPoint = Vector2.new(1, 0),
                Position = UDim2.new(1, -6, 0.5, -10),
                Size = UDim2.new(0, 120, 0, 20),
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                Text = ""
            })
        })
        table.insert(self.items, frame)
        self.library.gui.Container.ThemeButton.MouseButton1Click:Connect(function()
            wait(0.1)
            utility:tween(frame, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255)
            })
            utility:tween(frame.TextBoxLabel, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
            utility:tween(frame.TextBox, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(55, 55, 55) or Color3.fromRGB(230, 230, 230),
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
        end)
        frame.TextBox.FocusLost:Connect(function(enterPressed)
            if enterPressed and callback then
                callback(frame.TextBox.Text)
            end
        end)
        return frame
    end
    function page:addDropdown(name, options, defaultindex, callback)
        local frame = utility:create("Frame", {
            Name = utility:removeSpaces(name) .. "Dropdown",
            BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255),
            Size = UDim2.new(1, 0, 0, 30),
            Parent = self.container,
            ClipsDescendants = true
        }, {
            utility:create("UICorner", {
                CornerRadius = UDim.new(0, 4)
            }),
            utility:create("TextButton", {
                Name = "DropdownButton",
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 8, 0, 0),
                Size = UDim2.new(1, -8, 0, 30),
                ZIndex = 2,
                Font = Enum.Font.GothamSemibold,
                Text = name .. ": " .. options[defaultindex],
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextSize = 14,
                TextXAlignment = Enum.TextXAlignment.Left
            }),
            utility:create("TextLabel", {
                Name = "ArrowLabel",
                BackgroundTransparency = 1,
                AnchorPoint = Vector2.new(1, 0),
                Position = UDim2.new(1, 0, 0, 0),
                Size = UDim2.new(0, 30, 0, 30),
                Font = Enum.Font.GothamSemibold,
                Text = "v",
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextSize = 14
            }),
            utility:create("ScrollingFrame", {
                Name = "OptionsFrame",
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 0, 0, 30),
                Size = UDim2.new(1, 0, 0, 120),
                ScrollBarThickness = 0,
                ScrollingDirection = Enum.ScrollingDirection.Y,
                CanvasSize = UDim2.new(0, 0, 0, 0)
            }, {
                utility:create("UIListLayout", {
                    SortOrder = Enum.SortOrder.LayoutOrder
                })
            })
        })
        table.insert(self.items, frame)
        self.library.gui.Container.ThemeButton.MouseButton1Click:Connect(function()
            wait(0.1)
            utility:tween(frame, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255)
            })
            utility:tween(frame.DropdownButton, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
            utility:tween(frame.ArrowLabel, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
            for i, v in next, frame.OptionsFrame:GetChildren() do
                if v:IsA("TextButton") then
                    utility:tween(v, 0.5, {
                        TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
                    })
                end
            end
        end)
        local open = false
        for i, v in next, options do
            local button = utility:create("TextButton", {
                Name = utility:removeSpaces(v) .. "Option",
                Parent = frame.OptionsFrame,
                BackgroundTransparency = 1,
                Size = UDim2.new(1, 0, 0, 40),
                Font = Enum.Font.GothamSemibold,
                Text = "  " .. v,
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextSize = 14,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            button.MouseButton1Click:Connect(function()
                local str = button.Text:gsub("  ", "")
                frame.DropdownButton.Text = name .. ": " .. str
                utility:tween(frame, 0.25, {
                    Size = UDim2.new(1, 0, 0, 30)
                })
                utility:tween(frame.ArrowLabel, 0.25, {
                    Rotation = 0
                })
                if callback then
                    callback(str)
                end
                open = not open
            end)
            frame.OptionsFrame.CanvasSize = UDim2.new(0, 0, 0, frame.OptionsFrame.UIListLayout.AbsoluteContentSize.Y)
        end
        frame.DropdownButton.MouseButton1Click:Connect(function()
            utility:tween(frame, 0.25, {
                Size = open and UDim2.new(1, 0, 0, 30) or UDim2.new(1, 0, 0, 150)
            })
            utility:tween(frame.ArrowLabel, 0.25, {
                Rotation = open and 0 or 180
            })
            open = not open
        end)
        return frame
    end
    function page:addColorPicker(name, default, callback)
        default = default or Color3.fromRGB(255, 255, 255)
        local h, s, v = default:ToHSV()
        local frame = utility:create("Frame", {
            Name = utility:removeSpaces(name) .. "ColorPicker",
            BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255),
            Size = UDim2.new(1, 0, 0, 30),
            Parent = self.container,
            ClipsDescendants = true
        }, {
            utility:create("UICorner", {
                CornerRadius = UDim.new(0, 4)
            }),
            utility:create("TextButton", {
                Name = "ColorPickerButton",
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 8, 0, 0),
                Size = UDim2.new(1, -8, 0, 30),
                ZIndex = 2,
                Font = Enum.Font.GothamSemibold,
                Text = name,
                TextSize = 14,
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0),
                TextXAlignment = Enum.TextXAlignment.Left
            }),
            utility:create("TextButton", {
                Name = "HueFrame",
                Position = UDim2.new(0, 8, 0, 31),
                Size = UDim2.new(1, -16, 0, 30),
                Text = "",
                AutoButtonColor = false
            }, {
                utility:create("UICorner", {
                    CornerRadius = UDim.new(0, 4)
                }),
                utility:create("UIGradient", {
                    Color = ColorSequence.new({ColorSequenceKeypoint.new(0, Color3.fromHSV(1, 1, 1)), ColorSequenceKeypoint.new(0.2, Color3.fromHSV(0.2, 1, 1)), ColorSequenceKeypoint.new(0.4, Color3.fromHSV(0.4, 1, 1)), ColorSequenceKeypoint.new(0.6, Color3.fromHSV(0.6, 1, 1)), ColorSequenceKeypoint.new(0.8, Color3.fromHSV(0.8, 1, 1)), ColorSequenceKeypoint.new(1, Color3.fromHSV(1, 1, 1))})
                }),
                utility:create("Frame", {
                    Name = "Selector",
                    AnchorPoint = Vector2.new(0.5, 0.5),
                    BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Position = UDim2.new(1 / h, 0, 0.5, 0),
                    Size = UDim2.new(0, 4, 1, 2)
                })
            }),
            utility:create("TextButton", {
                Name = "SaturationFrame",
                Position = UDim2.new(0, 8, 0, 69),
                Size = UDim2.new(1, -16, 0, 30),
                Text = "",
                AutoButtonColor = false
            }, {
                utility:create("UICorner", {
                    CornerRadius = UDim.new(0, 4)
                }),
                utility:create("UIGradient", {
                    Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 1))
                }),
                utility:create("Frame", {
                    Name = "Selector",
                    AnchorPoint = Vector2.new(0.5, 0.5),
                    BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Position = UDim2.new(1-(1 / s), 0, 0.5, 0),
                    Size = UDim2.new(0, 4, 1, 2)
                })
            }),
            utility:create("TextButton", {
                Name = "BrightnessFrame",
                Position = UDim2.new(0, 8, 0, 107),
                Size = UDim2.new(1, -16, 0, 30),
                Text = "",
                AutoButtonColor = false
            }, {
                utility:create("UICorner", {
                    CornerRadius = UDim.new(0, 4)
                }),
                utility:create("UIGradient", {
                    Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 0))
                }),
                utility:create("Frame", {
                    Name = "Selector",
                    AnchorPoint = Vector2.new(0.5, 0.5),
                    BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Position = UDim2.new(1-(1 / v), 0, 0.5, 0),
                    Size = UDim2.new(0, 4, 1, 2)
                })
            }),
            utility:create("Frame", {
                Name = "Status",
                BackgroundColor3 = Color3.fromHSV(h, s, v),
                AnchorPoint = Vector2.new(1, 0),
                Position = UDim2.new(1, -6, 0, 5),
                Size = UDim2.new(0, 20, 0, 20)
            }, {
                utility:create("UICorner", {
                    CornerRadius = UDim.new(0, 4)
                })
            })
        })
        table.insert(self.items, frame)
        local open = false
        frame.ColorPickerButton.MouseButton1Click:Connect(function()
            utility:tween(frame, 0.25, {
                Size = open and UDim2.new(1, 0, 0, 30) or UDim2.new(1, 0, 0, 145)
            })
            open = not open
        end)
        self.library.gui.Container.ThemeButton.MouseButton1Click:Connect(function()
            wait(0.1)
            utility:tween(frame, 0.5, {
                BackgroundColor3 = self.library.theme == "Dark" and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(255, 255, 255)
            })
            utility:tween(frame.ColorPickerButton, 0.5, {
                TextColor3 = self.library.theme == "Dark" and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
            })
        end)
        local hdragging, sdragging, vdragging
        frame.HueFrame.MouseButton1Down:Connect(function()
            hdragging = true
            while hdragging do
                local perc = self:getPercentage(frame.HueFrame)
                h = perc
                utility:tween(frame.HueFrame.Selector, 0.2, {
                    Position = UDim2.new(h, 0, 0.5, 0)
                })
                frame.SaturationFrame.UIGradient.Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 1))
                frame.BrightnessFrame.UIGradient.Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 0))
                frame.Status.BackgroundColor3 = Color3.fromHSV(h, s, v)
                if callback then
                    callback(Color3.fromHSV(h, s, v))
                end
                wait()
            end
        end)
        userInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                hdragging = false
            end
        end)
        frame.SaturationFrame.MouseButton1Down:Connect(function()
            sdragging = true
            while sdragging do
                local perc = self:getPercentage(frame.HueFrame)
                s = 1 - perc
                utility:tween(frame.SaturationFrame.Selector, 0.2, {
                    Position = UDim2.new(1 - s, 0, 0.5, 0)
                })
                frame.SaturationFrame.UIGradient.Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 1))
                frame.BrightnessFrame.UIGradient.Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 0))
                frame.Status.BackgroundColor3 = Color3.fromHSV(h, s, v)
                if callback then
                    callback(Color3.fromHSV(h, s, v))
                end
                wait()
            end
        end)
        userInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                sdragging = false
            end
        end)
        frame.BrightnessFrame.MouseButton1Down:Connect(function()
            vdragging = true
            while vdragging do
                local perc = self:getPercentage(frame.HueFrame)
                v = 1 - perc
                utility:tween(frame.BrightnessFrame.Selector, 0.2, {
                    Position = UDim2.new(1 - v, 0, 0.5, 0)
                })
                frame.SaturationFrame.UIGradient.Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 1))
                frame.BrightnessFrame.UIGradient.Color = ColorSequence.new(Color3.fromHSV(h, s, v), Color3.fromHSV(0, 0, 0))
                frame.Status.BackgroundColor3 = Color3.fromHSV(h, s, v)
                if callback then
                    callback(Color3.fromHSV(h, s, v))
                end
                wait()
            end
        end)
        userInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                vdragging = false
            end
        end)
        return frame
    end
    function library:selectPage(page)
        if self.selectedPage == page then return end

        local button = page.button
        if self.selectedPage then
            utility:tween(self.selectedPage.container, 0.25, {
                Size = UDim2.new(1, 0, 0, 0)
            })
            utility:tween(self.selectedPage.button, 0.25, {
                TextTransparency = 0.3
            })
        end
        utility:tween(page.button, 0.25, {
            TextTransparency = 0
        })
        wait(0.25)
        utility:tween(page.container, 0.25, {
            Size = UDim2.new(1, 0, 1, 0)
        })
        self.selectedPage = page
    end
    function page:updateToggle(frame, active)
        utility:tween(frame.Status, 0.5, {
            BackgroundColor3 = active and Color3.fromRGB(53, 255, 53) or Color3.fromRGB(255, 53, 53)
        })
    end
    function page:updateSlider(frame, value, min, max)
        local pos = frame.Bar.Selector
        local bar = frame.Bar
        local percentage = value or ((mouse.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X)

        percentage = value and (value - min) / (max - min) or math.clamp(percentage, 0, 1)
        value = value or math.floor(min + (max - min) * percentage)

        frame.ValueLabel.Text = value
        utility:tween(pos, 0.2, {
            Position = UDim2.new(percentage, 0, 0.5, 0)
        })

        return value
    end
    function page:getPercentage(frame)
        local percentage = ((mouse.X - frame.AbsolutePosition.X) / frame.AbsoluteSize.X)
        return math.clamp(percentage, 0, 1)
    end
end
return library
