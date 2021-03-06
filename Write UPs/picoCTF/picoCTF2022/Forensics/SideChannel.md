Name: SideChannel

Category: Forensics

Challenge description:

![image](https://user-images.githubusercontent.com/95076839/161078211-052f8dff-0bb2-444d-b408-6c3f32372ee7.png)


In this write up I'll explain the methodology and techniques I used it to solve this challenge.

He give me an executable named pin_checker, the program wants us to enter an 8-digit number for it.

When go back to the hints he told me to search about timing-based side-channel attacks 

Ok I will put a simple explanation and sources that helped me knowing what this attack is.

Timing-based attack, this attack depends on time analysis (The attacker must analize the time taken to execute) to read more you'll find it <a href="https://en.wikipedia.org/wiki/Timing_attack">here</a>

I read <a href="https://medium.com/spidernitt/introduction-to-timing-attacks-4e1e8c84b32b">this article</a> about how the pin_checker works.

and know after we know what the type of programs we deal with we need to try to exploit the pin_checker, let's find out how to perform this type of attacks
using <a href="https://www.geeksforgeeks.org/time-command-in-linux-with-examples/">time.</a>
```bash 
bash $ time key word 
```
I Tried with this command to pass the input to the pin checker and measure the time.
```bash
$ time echo -n 12345678 | ./pin_checker
```
After we know what attack is and how to do it, we must start tracing the pattern of time to find the correct number

![image](https://user-images.githubusercontent.com/95076839/161085315-8c743293-6252-4ccf-b8bb-044373d7025e.png)

Notice this change in time indicates that the number 4 is correct, the code probably check if the first number of your pin is equal to the first number of the correct pin and this checking spend some time, if the first number is correc, it will check the next, and this checking also spends some time.
So i can guess what is the correct number of each position by analyzing this time differece.

performing some manual brute force on the pin and finally got it 
![image](https://user-images.githubusercontent.com/95076839/161090059-f85bd62c-24d7-4a4a-86ac-a80824e04d70.png)

I used this script to help me with manual brute force
``` python3
for i in range(0,10):
        print(f"time echo -n {i}0000000 |./pin_checker")
```
