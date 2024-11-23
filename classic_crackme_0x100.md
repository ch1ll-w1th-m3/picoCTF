# Challenge
## Classic Crackme 0x100
Link Challenge: https://play.picoctf.org/practice/challenge/409?category=3&page=1&search=

Đầu tiên, mình đọc và thấy đề bảo rằng cần phải recover lại để tìm ra password. 

Mình tải file binary về, chạy thử file crackme100. 
![image](https://github.com/user-attachments/assets/eb6263f4-59c7-429b-b43f-ba48f6b44060)

Có vẻ là mình cần tìm ra password sao cho output là gì đó liên quan tới flag. 

Mình mở file crackme100 bằng IDA, sau đó tìm tới hàm main. 
![image](https://github.com/user-attachments/assets/664f89b0-1642-4ca6-81de-2b7be6287e5b)

Ở dòng 35 có hàm memcmp, tức là input của mình sau khi qua giai đoạn mã hóa phải giống với output ở dòng 15. 

Tiếp đó, mình dịch, code phần giải mã của đoạn code trên và thu được flag. 
```c++
#include <bits/stdc++.h>

using namespace std;

char current_char;
string input = "xjagpediegzqlnaudqfwyncpvkqneusycourkguerjpzcbstcc";
int secret1 = 85;
int secret2 = 51;
int secret3 = 15;
int fix = 97;
int random1, random2, retval;
int len;

int main() {
    len = input.size();
    for (int i = 0; i < 3; i++)
        for (int j = len - 1; j >= 0; j--) {
            random1 = (secret1 & (j % 255)) + (secret1 & ((j % 255) >> 1));
            random2 = (secret2 & random1) + (secret2 & (random1 >> 2));
            current_char = input[j];
            retval = ((int(current_char - fix) + 26) % 26) - ((int)(random2 >> 4) & secret3) - (random2 & secret3);
            input[j] = char(retval + fix);
        }
    cout << input;
}
```
![image](https://github.com/user-attachments/assets/9cb68828-214e-4f74-87a9-86aa26319e4c)

Flag: `picoCTF{s0lv3_angry_symb0ls_31b29976}`
