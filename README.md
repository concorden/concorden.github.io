hs.window.animationDuration = 0

-- App focus hotkeys (alt + key)
-- Always activates, never toggles/hides
local bindings = {
	a = "Claude",
	d = "Codex",
	g = "Docker Desktop",
	b = "Google Chrome",
	o = "Obsidian",
	e = "Microsoft Outlook",
	s = "Microsoft Teams",
	n = "A Tale of Todos",
	m = "Spotify",
	c = "Visual Studio Code",
	t = "Ghostty",
	l = "Linear",
}

for key, app in pairs(bindings) do
	hs.hotkey.bind({ "alt" }, key, function()
		hs.application.launchOrFocus(app)
	end)
end

-- Auto-reload config on save
hs.pathwatcher
	.new(os.getenv("HOME") .. "/.config/hammerspoon/", function(files)
		hs.reload()
	end)
	:start()

-- ~/.hammerspoon/init.lua

local meh = { "ctrl", "alt", "shift" }
local hyper = { "ctrl", "alt", "cmd", "shift" }

-- Meh + HJKL: splits
hs.hotkey.bind(meh, "h", function()
	local win = hs.window.focusedWindow()
	local s = win:screen():frame()
	win:setFrame({ x = s.x, y = s.y, w = s.w / 2, h = s.h })
end)

hs.hotkey.bind(meh, "l", function()
	local win = hs.window.focusedWindow()
	local s = win:screen():frame()
	win:setFrame({ x = s.x + s.w / 2, y = s.y, w = s.w / 2, h = s.h })
end)

hs.hotkey.bind(meh, "k", function()
	local win = hs.window.focusedWindow()
	local s = win:screen():frame()
	win:setFrame({ x = s.x, y = s.y, w = s.w, h = s.h / 2 })
end)

hs.hotkey.bind(meh, "j", function()
	local win = hs.window.focusedWindow()
	local s = win:screen():frame()
	win:setFrame({ x = s.x, y = s.y + s.h / 2, w = s.w, h = s.h / 2 })
end)

-- Hyper + HJKL: directional focus
hs.hotkey.bind(hyper, "h", function()
	hs.window.focusedWindow():focusWindowWest(nil, true)
end)

hs.hotkey.bind(hyper, "l", function()
	hs.window.focusedWindow():focusWindowEast(nil, true)
end)

hs.hotkey.bind(hyper, "k", function()
	hs.window.focusedWindow():focusWindowNorth(nil, true)
end)

hs.hotkey.bind(hyper, "j", function()
	hs.window.focusedWindow():focusWindowSouth(nil, true)
end)

-- Hyper + Enter: fullscreen
hs.hotkey.bind(meh, "return", function()
	hs.window.focusedWindow():maximize()
end)

local function isExcludedApp()
	local app = hs.application.frontmostApplication():bundleID()
	return app == "com.mitchellh.ghostty" or app == "dev.zed.Zed"
end

hs.hotkey.bind({ "cmd" }, "h", function()
	if isExcludedApp() then
		-- Pass the keystroke through to the app
		hs.eventtap.keyStroke({ "cmd" }, "h", 0, hs.application.frontmostApplication())
	end
	-- Otherwise do nothing (suppress macOS Hide)
end)

hs.alert.show("Hammerspoon config loaded")



