# ahk-backup
windows hotkeys


#NoEnv 
; #Warn
SendMode Input  
SetWorkingDir %A_ScriptDir%  

; I hate navigating with a trackpad so I made this to make life easier
; My favourite minor change is switching the tilde key to backspace
; Makes control + backspace easier
; You can still access tilde functions with alt / shift alt

; -----------------
; WINDOW MANAGEMENT
; -----------------
; splits windows so that there is a a small gutter around each window so you can see the windows underneath and just click on them to focus

; split window to left side
^Home::
	WinMove A, , 6, 10, 950, 1060
return

; split window to right side
<^>!D::
^End::
	WinMove A, , 960, 10, 950, 1060
return

; split windows vertically
; this is so you can easily access 4 windows but still
; have enough vertical space on each window
^PgUp::
	WinMove A, , ,10 , 950, 800
return
^PgDn::
	WinMove A, , ,270 , 950, 800
return

; standard window resize and centers
^!Space::
	WinMove A, ,0,0, 1380, 900
	WinGetPos,,, Width, Height, A
    WinMove, A,, (A_ScreenWidth/2)-(Width/2), (A_ScreenHeight/2)-(Height/2), 1380, 900
return

; open taskbar view with mouse button
XButton2::#Tab
XButton1::#Tab

; ----------------------
; altGr window switching
; ----------------------

<^>!J::
	GroupAdd, chromeWindows, ahk_exe chrome.exe
	GroupActivate, chromeWindows, R
	IfWinExist, ahk_exe chrome.exe
	return
	IfWinNotExist, ahk_exe chrome.exe
		Run, chrome.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs
		WinWait, ahk_exe chrome.exe
		WinMove, ahk_exe chrome.exe,, (A_ScreenWidth/2)-(1400/2), (A_ScreenHeight/2)-(900/2), 1400, 900
	return

; start a new chrome window
#<^>!J::
	Run, chrome.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs
	WinWait, ahk_exe chrome.exe
	WinMove, ahk_exe chrome.exe,, (A_ScreenWidth/2)-(1400/2), (A_ScreenHeight/2)-(900/2), 1400, 900
return

+<^>!J::
	Run, Google Chrome, C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Google
	WinWait, ahk_exe chrome.exe
	WinMove, ahk_exe chrome.exe,, (A_ScreenWidth/2)-(1400/2), (A_ScreenHeight/2)-(900/2), 1400, 900
return

<^>!K::
	WinActivate, ahk_exe spotify.exe
	IfWinNotExist, ahk_exe spotify.exe
		Run, spotify.exe, C:\Users\Charlie\AppData\Roaming\Spotify
	return
return

<^>!L::
	WinActivate, ahk_exe notepad++.exe
	IfWinNotExist, ahk_exe notepad++.exe
		Run, notepad++.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs
	return
return
<^>!;::
	WinActivate, ahk_exe Brackets.exe
	IfWinNotExist, ahk_exe Brackets.exe
		Run, Brackets.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs
	return
return

; switch to previous window without moving from typing position
<^>!N::
Sleep, 10
SendInput !{Escape}
return

<^>!,::
sendinput ^+{Tab}
return

<^>!.::
sendinput ^{Tab}
return

; launch snipping tool with printscreen
PrintScreen::
	WinActivate, ahk_exe SnippingTool.exe
	IfWinNotExist, ahk_exe SnippingTool.exe
		Run, SnippingTool.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Accessories
	return
return

; ---------------
; Mouse functions
; ---------------

; Change volume with mouse buttons
+!Home::send {Volume_Up}
+!End::send {Volume_Down}

; Ctrl + Mouse Wheel left/right to skip song
^+Home::
	send {Media_Prev}
return

^+End::
	send {Media_Next}
return

; Reload Chrome in 1 click
Insert::
	IfWinActive, ahk_exe notepad++.exe
		WinActivate, ahk_exe chrome.exe
		send {F5}
	return
; ------------
; Key switches
; ------------

; puts backspace on left side
$`::Backspace

; fixes tilde and backslash 
!+Backspace:: send {\}
return
!Backspace:: send {```}
return

; changes altGr + delete to SuperF4 without closing explorer.exe
; https://stefansundin.github.io/superf4/

<^>!BS::
if (!WinActive("ahk_exe explorer.exe") & !WinActive("ahk_exe chrome.exe"))
{
send ^!{f4}
}
if WinActive("ahk_exe chrome.exe")
{
send ^+W ; stops from alt+f4ing chrome and creating a 'restore tabs' dialogue
}
return

; turns up volume by scrolling over the taskbar
#If MouseIsOver("ahk_class Shell_TrayWnd")
WheelUp::Send {Volume_Up}
WheelDown::Send {Volume_Down}

MouseIsOver(WinTitle) {
    MouseGetPos,,, Win
    return WinExist(WinTitle . " ahk_id " . Win)
	
	
;-----------------------------------------------------------


/* start of a gui
LControl & RAlt::
	Gui, New, +AlwaysOnTop, dik
	Gui, Show, X10 Y10 W300 H200, dik
	
	
	
	Loop {
		KeyWait, J, D T0.02
		if not ErrorLevel {
			GroupAdd, chromeWindows, ahk_exe chrome.exe
			GroupActivate, chromeWindows, R
			
			If (WinExist("ahk_exe chrome.exe") = 0) {
				Run, chrome.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs
				WinWait, ahk_exe chrome.exe
				WinMove, ahk_exe chrome.exe,, (A_ScreenWidth/2)-(1400/2), (A_ScreenHeight/2)-(900/2), 1400, 900
			}
		}
		KeyWait, K, D T0.02
		if not ErrorLevel {
			WinActivate, ahk_exe spotify.exe
			IfWinNotExist, ahk_exe spotify.exe
				Run, spotify.exe, C:\Users\Charlie\AppData\Roaming\Spotify
		}
		KeyWait, L, D T0.02
		if not ErrorLevel {
			WinActivate, ahk_exe notepad++.exe
			IfWinNotExist, ahk_exe notepad++.exe
				Run, notepad++.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs
		}
		KeyWait, `;, D T0.02
		if not ErrorLevel {
			WinActivate, ahk_exe Brackets.exe
			if (WinExist("ahk_exe Brackets.exe") = 0) {
				Run, Brackets.exe, C:\ProgramData\Microsoft\Windows\Start Menu\Programs
			}
		}
		KeyWait, RAlt, T0.02
		if not ErrorLevel
			break
	}

	
	
	Gui, Destroy

	
	
	
	
	return
return

*/
}
