[back to overview](/../..)

# VSCode Cheatsheet<!-- omit in toc -->
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Extension Help](#extension-help)
  - [APC Extension](#apc-extension)

# Keyboard shortcuts
- Option + Up/Down - move line or selection up or down one line
- CTRL + Enter [+ SHIFT] - move to newline below [above] without breaking current line
- CTRL + SHIFT + K - delete line
- CTRL + L - select entire line
- CTRL + G - search for current word and select
- CTRL + D - search for current word and select in addition to current selection
- F2 - refactor -> rename current variable

# Extension Help
## APC Extension
NO LONGER SUPPORTED: After VSCode update, do `sudo chown -R $(whoami) "/Applications/Visual Studio Code.app/Contents/Resources/app/out"` then from command palate `Enable APC extension`

Update file `/Applications/Visual Studio Code.app/Contents/Resources/app/out/vs/workbench/workbench.desktop.main.css`, replace entry for `.quick-input-widget{` with  `.quick-input-widget{left:5px !important;margin-left:0 !important;position:absolute;width: calc(100% - 60px) !important;top: 40px !important;z-index:2550;-webkit-app-region:no-drag;border-radius:6px}`
