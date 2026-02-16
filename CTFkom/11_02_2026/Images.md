## Title: Images ##

**Category:** Steganography

**Description:** I love pictures!
Flag-format: CTFkom{fake_flag}

-----

In this challenge, we are given a single image of the famous rockband Queens:

![image](https://github.com/user-attachments/assets/49324f25-77b4-42d3-a444-82b60575b888)

The image might seem a little suspicous at first because of the color and the lights on the ceiling, so we check for any potential hints or 
solution in the metadata like we normally do in any steganography challenge.

I choose to run the exiftool command in the kali bash to extract the metadata.
```
exiftool image.jpg
```
Output: <br>
<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/15d7d29d-a0b1-4fe5-8a56-00e99a6a5d01" />
<br>
The output doesn't tell a lot which is suspicious in itself. Since the title "images" is given in plural, it seems a little weird that we are only 
given a single picture, which probably suggests that there is something hidden deeper inside.

### Solution

To further test our theory we can try some steganography tools in the bash terminal to check for hidden embedded files or pictures. 
A clear candiate for this is to use steghide command since the image is of type JPEG. The steghide command is an open-source commandline 
tool used to hide files inside JPEGs and BMP, or audio files like WAV and AU.

Just to check if we are on the right track, we can use steghide info in the commandline to look if there is any files hidden in the image.
```
steghide info image.jpg
```
Output:
<br>
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/70294082-8f40-4c83-8e49-3c3afa1469a1" />
<br>
We can tell from the capacity that there is definitely something hidden inside the image, however a passphrase is needed to extract the hidden data.
The quickest way to search and potentially crack a stegfile with an unknown passphrase is to run it through a huge wordlist.
Stegseek is a tool that lets you do exactly this, it even extracts the hidden data if it manages to find the valid passphrase.

We try it out in the bash:
```
stegseek --crack image.jpg
```
Output:
<br>
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/0275c469-98c5-4ba7-b6e7-84e1d0536891" />
<br>
In under a second the tool managed to succesfully find a valid passphrase and extract the data.

The hidden data was extracted as a jpg file named "image1", so we open the image and take a look. 
```
open image1.jpg
```

Output: <br>
![image1](https://github.com/user-attachments/assets/51aa1aa8-11b7-4516-8cfa-38a514ba8124) 
<br>
It seems like we found "the flag". 
We take a look in the metadata, but the flag doesnt seem to be there. Therefore we can assume that it's hidden in the image again.
We do a check with the steghide info command to test our theory: <br>
```
steghide info image1.jpg
```

Output: <br>
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/20d18efb-2862-4f32-9e98-18f551a930fa" />

Seems like theres something hidden again. We repeat the extraction process by using stegseek --crack that iterates over the rockyou.txt wordlist 
for valid passphrases, and extracts the hidden embedded data.
```
stegseek --crack image1.jpg
```
Output: <br>
<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/2f255598-f58d-47df-aa4c-fd50b1a9d959" />
<br>
The output gives a flag.txt file that contains the flag:
**CTFkom{W0W_U_f1gUr3d_m3_0uT}**

----

### Conclusion

In this challenge it's important to pay attention to the details in the description and title. The callenge required a multi-step extraction
process using steganography tools like [steghide](https://www.kali.org/tools/steghide/)
 and [stegseek](https://github.com/RickdeJager/stegseek). The stegseek passphrase bypass is a vital lesson that shows how bad actors can brute-force
 weak passwords in seconds using massive wordlists. Furthermore, Steghide could also be used maliciously by moving sensitive data or even malicious scripts without detection. These hidden payloads can then be extracted at a later time and executed.




