# intro-reversing

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/138242812/7f296cbe-122d-4a26-b39a-8e25e2df19e8)

Dùng tool để check thông tin cơ bản của file.

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/138242812/d47ead49-37f0-427b-8f72-db6c642a91cc)

DÙng ida64 để phần tích. Ở đây tôi thấy rằng chương trình là 1 vòn for dùng để in ra đoạn flag. 

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/138242812/89d548ff-3544-467b-87b6-97b95d299b66)

Nhìn qua trông có vẻ là vô tri nhưng tôi đã search và thấy rằng đây là ASCII art. Dùng chat gpt để viết lại và tôi có flag 

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/138242812/38d5cecf-e516-4262-8bb2-7ac2b21098c3)

 `vsctf{1nTr0_r3v3RS1ing!}`

# awa-jelly

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/138242812/f012af1f-f931-40d1-8b0f-069dc8098dd0)

2 tài liệu của bài này là: `https://jellyc.tf/` và `https://github.com/TempTempai/AWA5.0`. Đây là ngôn ngữ được thiết kế cho những người theo chủ nghĩa vô thần.

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/138242812/48d85fb3-2586-482b-93aa-fdca6071082e)

Sau 1 thời gian đọc tài liệu và tìm hiểu tôi đã viết ra được đoạn code của chương trình này

```
awa awa awawawa                            red     
awa awawawawa                              pop      
awa awawawa awa | awa awa awawa awa        sbm 2   
awa awawawa awa | awa awa awawawa          sbm 3    
awa awawawa awa | awa awawa awa awa        sbm 4   
awa awawawa awa | awa awa awa awawa        sbm 1
awa awawawa awa | awa awawawa awa          sbm 6
awa awawawa awa | awa awawa awawa          sbm 5
awa awawawa awa | awa awa awawawa          sbm 3
awa awawawa awa | awawa awawa awa          sbm 10
awa awawawa awa | wa awawa awa awa         sbm 20
awa awawawa awa | wa awawawa awa           sbm 22
awa awawawa awa | wawa awa awawa           sbm 25
awa awawawa awa | awa awa awawawa          sbm 3
awa awawawa awa | awa awa awa awa awa      sbm 0
awa awawawa awa | awa awa awa awa awa      sbm 0
awa awawawa awa | awa awa awawa awa        sbm 2
awa awawawa awa | awa awa awawawa          sbm 3
awa awawawa awa | awa awawa awa awa        sbm 4
awa awawawa awa | awa awa awa awawa        sbm 1
awa awawawa awa | awa awawawa awa          sbm 6
awa awawawa awa | awa awawa awawa          sbm 5
awa awawawa awa | awa awa awawawa          sbm 3
awa awawawa awa | awawa awawa awa          sbm 10
awa awawawa awa | wa awawa awa awa         sbm 20
awa awawawa awa | wa awawawa awa           sbm 22
awa awawawa awa | wawa awa awawa           sbm 25
awa awawawa awa | awa awa awawawa          sbm 3
awa awawawa awa | awa awa awa awa awa      sbm 0
awa awawawa awa | awa awa awa awa awa      sbm 0
awa awawawa awa | awa awa awa awa awa      sbm 0
awa awawawa awa | wa awa awa awa awa       sbm 16
awa awawawa awa | wawa awawa awa           sbm 26
awa awawawa awa | wawawawawa               sbm 31
awa awa awa awawa                          prn       
awa awa awa awawa                          prn       
awa awa awa awawa                          prn       
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
awa awa awa awawa                          prn
```

Giải thích cơ bản thì có chỉ là swap cá kí tụ trong đoạn mã đó rồi in ra. Ở đây tôi viết 1 scrip cơ bản để thực thi ngược chuyện đó. 

```
ouput = list("1o1i_awlaw_aowsay3wa0awa!iJlooHi")
swap = [2, 3, 4, 1, 6, 5, 3, 10, 20, 22, 25, 3, 0, 0, 2, 3, 4, 1, 6, 5, 3, 10, 20, 22, 25, 3, 0, 0, 0, 16, 26, 31]

for i in subm[::-1]:
    if i != 0:
        data = [data[i]] + data[0:i] + data[i+1:]
    else:
        data.insert(0, data[-1])
        data = data[:-1]

print("".join(data))

```
Và tôi có flag là : `vsctf{J3lly_0oooosHii11i_awawawawaawa!}`



 
